# Observability Reading List

Background reading for our logging, metrics and alerting setup. Each entry says what it explains and where we used the idea. For how our setup actually works, see [azure-app-service-logging-setup.md](azure-app-service-logging-setup.md) and the "Logging" section of [CONTRIBUTING.md](../CONTRIBUTING.md).

---

## Start here (if you read only three)

1. **Google SRE Book, "Monitoring Distributed Systems"**: https://sre.google/sre-book/monitoring-distributed-systems/
   The **four golden signals**: latency, traffic, errors, saturation. This is the backbone of our alerts: slow API, 5xx spike, CPU/memory.
2. **Brendan Gregg, "The USE Method"**: https://www.brendangregg.com/usemethod.html
   How to check CPU, memory and other resources: **U**tilization, **S**aturation, **E**rrors. It's the right way to read our CPU and memory alerts.
3. **OWASP Logging Cheat Sheet**: https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html
   What to log, what **never** to log (secrets, personal data), and which security events matter. This is the basis for Phases 1, 4 and 5.

---

## Logging

| Read | What it explains | Where we used it |
|---|---|---|
| **The Twelve-Factor App, "Logs"**: https://12factor.net/logs | Apps write logs to stdout as a stream, and the platform collects them | Why we log to stdout and let App Service and Log Analytics collect it |
| **OpenTelemetry, Logs Data Model**: https://opentelemetry.io/docs/specs/otel/logs/data-model/ | A standard shape for a log record (timestamp, severity, body, attributes, trace id) | Our JSON line shape and field names (Phase 1) |
| **NIST SP 800-92, Guide to Computer Security Log Management**: https://csrc.nist.gov/pubs/sp/800/92/final | Retention, protecting logs, reviewing them | 30-day retention, access control, the masked-data check |
| **Book: *Observability Engineering*** (Majors, Fong-Jones, Miranda; O'Reilly) | Why structured "wide events" beat plain text logs for debugging | Structured JSON, request IDs, "debug in minutes" |

---

## Request IDs and tracing

| Read | What it explains |
|---|---|
| **Google's Dapper paper**: https://research.google/pubs/dapper-a-large-scale-distributed-systems-tracing-infrastructure/ | The original paper behind request and trace IDs: following one request through every step. Our `httpRequestId` and "Ref" are a small version of this idea (Phase 2) |

---

## CPU, memory, latency and spikes

| Read | What it explains |
|---|---|
| **Tom Wilkie, "The RED Method"** (Grafana blog): https://grafana.com/blog/2018/08/02/the-red-method-how-to-instrument-your-services/ | **R**ate, **E**rrors, **D**uration per endpoint, the service-side partner to USE. It's what our `http_request` access line records |
| **"The Tail at Scale"** (Dean & Barroso, Google): https://research.google/pubs/the-tail-at-scale/ | Why **p95/p99** matter more than averages: rare slow requests are what users feel. It's why our slow-API alert uses p95 |
| **Gil Tene, "How NOT to Measure Latency"** (conference talk; search the title on YouTube) | Common mistakes with averages, percentiles and spikes. Very practical |
| **Node.js, "Don't Block the Event Loop"**: https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop | Why one slow synchronous task can freeze a Node server. That's what CPU spikes and slow-API alerts often point to in our stack |

---

## Alerting (what to alert on, without noise)

| Read | What it explains |
|---|---|
| **Google SRE Workbook, "Alerting on SLOs"**: https://sre.google/workbook/alerting-on-slos/ | Alert on what users feel (errors, latency), not on every internal blip. Thresholds, windows, burn rates |
| **Rob Ewaschuk, "My Philosophy on Alerting"** (search the title; it's also folded into the SRE book) | Every alert should be urgent, actionable and real. Why our alerts include "what to check first" |

---

## Azure-specific (how our setup works)

- **Monitor App Service** (metrics like CPU, memory, 5xx): https://learn.microsoft.com/en-us/azure/app-service/web-sites-monitor
- **Health check**: https://learn.microsoft.com/en-us/azure/app-service/monitor-instances-health-check
- **Types of Azure Monitor alerts** (metric vs log-search alerts): https://learn.microsoft.com/en-us/azure/azure-monitor/alerts/alerts-types

---

## Suggested reading order (about 3 hours)

1. The SRE "Monitoring" chapter (45 min).
2. The USE and RED methods (30 min together).
3. The OWASP Logging Cheat Sheet (30 min).
4. "The Tail at Scale" (30 min).
5. SRE "Alerting on SLOs" (45 min).