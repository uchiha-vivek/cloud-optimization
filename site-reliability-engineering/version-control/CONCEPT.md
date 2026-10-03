## Entire GIT Workflow



## Branching Strategies


## Merge Conflicts


## PULL Requests


## CODE REVIEW


## GIT HOOKS


## REPOSITORY SECURITY



## LFS MANAGEMENT



File Size Limits


- GitHub limits the size of files allowed in repositories. If you attempt to add or update a file that is larger than 50 MiB, you will receive a warning from Git. The changes will still successfully push to your repository, but you can consider removing the commit to minimize performance impact. For more information



- GitHub blocks files larger than 100 MiB.

- If you need to distribute large files within your repository, you can create releases on GitHub.com instead of tracking the files


**About Git Large File Storage**

Git LFS handles large files by storing references to the file in the repository, but not the actual file itself. To work around Git's architecture, Git LFS creates a pointer file which acts as a reference to the actual file (which is stored somewhere else). GitHub manages this pointer file in your repository. When you clone the repository down, GitHub uses the pointer file as a map to go and find the large file for you.

