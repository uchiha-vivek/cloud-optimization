Namespace isolation in Linux allows you to virtualize system resources, giving a process its own isolated view of the system (such as process IDs, network interfaces, or mount points). This is the underlying mechanism that powers container platforms like Docker.



## Step-by-Step: Creating an Isolated PID & Network Namespace

1. Low-Level Implementation via ctypes


```python

import ctypes
import os
import subprocess
import sys

# System call constants from <bits/sched.h>
CLONE_NEWNS   = 0x00020000  # Mount namespace
CLONE_NEWUTS  = 0x04000000  # UTS (hostname) namespace
CLONE_NEWIPC  = 0x08000000  # IPC namespace
CLONE_NEWUSER = 0x10000000  # User namespace
CLONE_NEWPID  = 0x20000000  # PID namespace
CLONE_NEWNET  = 0x40000000  # Network namespace

libc = ctypes.CDLL("libc.so.6", use_errno=True)

def isolate_process():
    # 1. Unshare User, UTS, PID, and Network namespaces
    flags = CLONE_NEWUSER | CLONE_NEWUTS | CLONE_NEWPID | CLONE_NEWNET
    
    res = libc.unshare(flags)
    if res != 0:
        errno = ctypes.get_errno()
        raise OSError(errno, os.strerror(errno))

    # 2. PID namespaces only affect child processes.
    # We must fork so the child process becomes PID 1 inside the new namespace.
    pid = os.fork()

    if pid == 0:
        # --- CHILD PROCESS (Inside isolated namespace) ---
        print(f"[Child] Inside namespace. Real PID: {os.getpid()}")

        # Set a custom hostname inside the isolated UTS namespace
        libc.sethostname(b"isolated-container", 18)

        # Verify isolated hostname and network interfaces
        subprocess.run(["hostname"])
        subprocess.run(["ip", "link"])

        sys.exit(0)
    else:
        # --- PARENT PROCESS ---
        os.waitpid(pid, 0)
        print("[Parent] Child exited. Parent hostname remains intact:")
        subprocess.run(["hostname"])

if __name__ == "__main__":
    isolate_process()

```



### When we should liux namespace isolation


1. Running untrusted or User Submitted Code

**Scenario**: Building an online code execution platform (like LeetCode, HackerRank, or Jupyter Notebook hosting) where users submit arbitrary Python scripts.

**WHY NAMESPACES**

- Network (CLONE_NEWNET): Prevents user code from making external network requests, making port scans against your internal network, or accessing host metadata services (like AWS IAM endpoint 169.254.169.254).

- PID (CLONE_NEWPID): Prevents user scripts from seeing, signaling, or killing other processes running on the host machine.

- Mount (CLONE_NEWNS): Prevents user code from reading or modifying the host filesystem.



## Automated Network & Systems Testing


**SCENARIO** : Testing distributed systems, network services, microservices, or custom routing logic locally without setting up multiple virtual machines.


## Lightweight Micro-Agent & Worker Isolation

**SCENARIO**: Building a custom worker runner or daemon in Python that executes tasks for multiple clients (multi-tenancy) on shared compute nodes.

