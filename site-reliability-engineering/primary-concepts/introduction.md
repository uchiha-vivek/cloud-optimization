## INTRODUCTION

Site Reliability Engineering applies software engineering principles to infrastructure and operations, replacing manual administration with scalable automation and code-driven systems management. Organizations use these practices to treat operational tasks as software problems, building robust platforms that can absorb failures, scale gracefully, and minimize toil. By establishing clear operational boundaries and measurable service health targets, teams maintain a sustainable balance between rapid feature delivery and platform stability.


## ERROR BUDGET

Allocating an allowable amount of unreliability to a software system lets development teams balance the speed of shipping new features against the cost of downtime. Expressed as a percentage over a rolling window, such as 99.9% uptime over thirty days, this numerical ceiling dictates how many failures, failed requests, or outages are acceptable before deployment freezes take effect to prioritize stabilization.


## SERVICE LEVEL INDICATORS

Service Level Indicators quantify the reliability and performance of a system by measuring specific operational metrics, such as request latency, error rates, or system throughput. These indicators are expressed as a ratio of successful events to total events over a given time window, providing the raw telemetry needed to evaluate user experience. By establishing clear thresholds on these metrics, engineering teams can objectively assess whether a service is meeting its operational goals.


## SERVICE LEVEL OBJECTIVE

Continuous measurement of service-level indicators against predefined thresholds determines whether a system meets reliability targets over rolling time windows. Engineers analyze these aggregated error rates and latency measurements to calculate remaining error budgets and trigger alerts before user experience degrades significantly.

In simple words : The target goal you want to achieve, like 99.9% availability over a month.



## SERVICE LEVEL AGREEMENT

Contractually binding agreements between a service provider and its customers establish quantitative metrics for uptime, availability, and response times, alongside financial penalties or service credits if those thresholds are breached. Operating within the boundaries defined by underlying service level objectives and indicators, these legal and operational frameworks align technical reliability targets with business expectations.