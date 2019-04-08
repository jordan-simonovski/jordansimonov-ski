---
title: "Systems Observability - What To Consider Before Writing Your First Line of Code"
date: 2019-04-07T11:30:20+11:00
summary: "Before even beginning to set things up, we should understand what makes good monitoring, logging, and alerting."
tags: ["infra","observability","monitoring","logging"]
header:
  image: "https://media.giphy.com/media/jsl2rkThL2Pjq/giphy.gif"
  caption: "Setting Up Observable Systems - What To Consider Before Writing Your First Line of Code"
---

Observability is still a relatively novel concept to many organisations. While it's easy to say that you want to do "observability", it is in fact a state that your systems are in. Monitoring, logging, and tracing of systems are things that we should do in order to achieve *observable systems*.

This is the first post I'll be making in a series of post centred around observability practices, and will be covering the basics of practices to help you achieve more observable systems. The next post in this series will be covering observability as code, and how I've used Terraform to make setting up DataDog a breeze for new applications.
Something I'll also want to cover is SLIs, and how they should be determined when making applications production-ready.

I am also paraphrasing a few things from Google's Site Reliability Handbook (freely available [here](https://landing.google.com/sre/sre-book/toc/)) which I have found incredibly useful in determining what good observability looks like.

When considering observability, it is useful to know the following:

- No complex system is ever fully healthy.
- Distributed systems are pathologically unpredictable.
- It's impossible to predict the different states of partial failure that various part s of the system might end up in.
- Failure needs to be embraced at every phase, from system design to implementation, testing, deployment, and operation.

In this post, I'll aim to cover a few things we should be doing, such as:

- [Good Coding and Testing Practicess](#good-coding-and-testing-practicess)
- [What Should We Be Monitoring?](#what-should-we-be-monitoring)
- [What Should We Be Logging?](#what-should-we-be-logging)
- [What Should We Be Alerting?](#what-should-we-be-alerting)
- [Some Notes](#some-notes)

## Good Coding and Testing Practicess

Historically, the idea of testing was something that was only ever referred to as a pre-production or pre-release activity. This school of thought is slowly being phased out as development teams are now responsible for developing, testing, and operating the services they make.

The idea that production environments are sacred and not meant to be fiddled around with also means that our pre-production environments are at best a pale imitation of what production actually looks like. The [12 Factor App](https://12factor.net/) manifesto focuses on the applications we write having minimal divergence between development and production.

While pre-production testing is very much a common practice in modern software development, the idea of experimenting with live traffic is seen as something alarming. This requires not only a change in mindset, but also importantly requires an overhaul in system design, along with a solid investment in release engineering practices and tooling.

In essence, not only do we want to architect for failure, but also coding and testing for failure when the default is to code and test for success.

We should aspire to look to expand the reach of our testing. The following diagram mentions many of the ways we can begin writing more resilient systems:

{{< image src="/img/testing.webp" alt="Testing" position="center" style="border-radius: 8px;" >}}

## What Should We Be Monitoring?

Observability is a superset of both monitoring and testing. It provides information about unpredictable failure modes that couldn't be monitored or tested.

{{< image src="/img/monitoring.webp" alt="Monitoring" position="center" style="border-radius: 8px;" >}}

That being said, we should still focus on having a minimal set of requirements for monitoring our systems.
A good set of metrics to begin with for monitoring are the [USE Method](http://www.brendangregg.com/usemethod.html) and [RED Metrics](https://www.weave.works/blog/the-red-method-key-metrics-for-microservices-architecture/). Depending on the use case, we should be able to monitor some if not all of these metrics.
Monitoring data should at all times provide a bird’s-eye view of the overall health of a system by recording and exposing high-level metrics over time across all components of the system. This includes, but is not limited to:

- load balancers
- caches
- queues
- databases
- stateless services

Monitoring data accompanying an alert should provide the ability to drill down into components and units of a system as a first port of call in any incident response to diagnose the scope and coarse nature of any fault.

It is also worth noting that good monitoring means metrics are being shipped out of our hosts, ideally to a Time Series Database like [Prometheus](https://prometheus.io/). If you find yourself SSH'ing into a box to debug issues, this usually means that you're not shipping enough information from your hosts to make your systems observable.

**USE Metrics (For every resource, check utilisation, saturation, and errors):**

- **Utilisation:** 100% utilisation is usually a sign of a bottleneck (check saturation and its effect to confirm). High utilisation (eg, beyond 70%) can begin to be a problem for a couple of reasons:
  - When utilisation is measured over a relatively long time period (multiple seconds or minutes), a total utilisation of, say, 70% can hide short bursts of 100% utilisation.
  - Some system resources, such as hard disks, cannot be interrupted during an operation, even for higher-priority work. Once their utilisation is over 70%, queueing delays can become more frequent and noticeable. Compare this to CPUs, which can be interrupted ("preempted") at almost any moment.
- **Saturation:** any degree of saturation can be a problem (non-zero). This may be measured as the length of a wait queue, or time spent waiting on the queue.
- **Errors:** non-zero error counters are worth investigating, especially if they are still increasing while performance is poor.
  
**RED Metrics (Rate, Errors, Duration):**

- **Request Rate** - the number of requests, per second, you services are serving.
- **Request Errors** - the number of failed requests per second.
- **Request Duration** - distributions of the amount of time each request takes.

Application tracing works its way into this setup as well, as traces give us some valuable information that could otherwise be lost with basic USE or RED metrics.
One thing to look out for is not jumping onto the easiest thing to measure, which is often the _mean of some quantity_. 
We can't necessarily monitor and alert on the mean usage of something like CPU utilisation, as it can be utilised in a very imbalanced way. The same can be said about latency.

Example:

- Let's say we have an application that sends some information to a front-end that we have, with an average latency of 100ms at 1000 requests per second.
- 1% of those requests may easily take 5 seconds.
- The 99th percentile of this can easily become the median response to our front-end.
- If we're running what we think is a highly-performant system, information on the latency of the 99th percentile serves as a useful indicator for working out _when_ our systems aren't as performant as we may outline in our [SLIs](https://cloud.google.com/blog/products/gcp/sre-fundamentals-slis-slas-and-slos).

As long as we are monitoring what is referred to as the [Four Golden Signals in Google's SRE Handbook](https://landing.google.com/sre/sre-book/chapters/monitoring-distributed-systems/#xref_monitoring_golden-signals), we are covering what is considered to be a minimal, yet effective set of metrics for determining bottlenecks in our systems or potential outages. The metrics above should cover those four.

## What Should We Be Logging?

While logging gives is a great high level overview, it is only really useful if we are getting useful information from our logs. It's not really worth logging literally everything that your application does, as there are much better ways of introspecting application behaviour such as tracing.

When writing our own applications, we must consider what we could use the logs for, and how we can enable more verbose logging (DEBUG). The requirements for logging should include, but not be limited to:

- **Structured logs over plaintext logs (unstructured/common log formats) for applications we maintain** - Typically these days, structured logs are in JSON format, which makes them easier to digest, process, and query without the need of having to set up additional logging infrastructure to transform logs.
- **WARN and ERROR logs being captured in production-like environments (staging/production)** - INFO level logs such as access logs will be captured by ingress controllers (routers/reverse proxies/load balancers). DEBUG logs should be turned off in environments that aren't a local/development environment.
- **Personally Identifiable Information (PII) should be obfuscated or removed from log messages** - logging should never include PCI or PII. If we need some kind of audits in place, references to information should be used instead (think transaction ID, application ID, etc). Many companies fall into this trap including [Twitter](https://arstechnica.com/information-technology/2018/05/twitter-advises-users-to-reset-passwords-after-bug-posts-passwords-to-internal-log/).
- **Logging every major entry and exit point of a request** - Which is helpful when identifying any potential failures when executing particular types of requests. Depending on the observability platform you're working with, injecting trace IDs into these logs can prove to be incredibly useful.
- **Log every decision point of a request** - This should give us more information about the codepath taken with a request, and where a potential failure may have occured. Grafana's [Loki](https://grafana.com/loki) has made tracing log events to metrics and vice-versa incredibly useful.

For the last two points (logging major entry, exit, and decision points), the idea is to have them all semantically linked in a way (request ID), that allows us to:

- Reconstruct codepaths taken
- Derive request or error ratios from any single point in the codepath

## What Should We Be Alerting?

While each team is able to tweak what they would like to get alerted on, alerts should at a minimum contain the **USE** and **RED** metrics (mentioned in [the monitoring section](#what-should-we-be-monitoring)). This includes at a minimum:

- **Resource Utilisation** - are we seeing a large spike in resource utilisation?
- **Saturation** - how long are our tasks/messages waiting in a queue?
- **Error Rates** - Have error rates spiked?
- **Request Rates** - Are we seeing an abnormal amount of traffic?
- **Request Durations** - are requests timing out/taking too long
- **System Availability** - Uptime and ping checks, otherwise known as [synthetic checks](https://smartbear.com/learn/performance-monitoring/what-is-synthetic-monitoring/).

We should also keep in mind that we do not create too much noise with our alerting, which dilutes the effectiveness of an alert which should be informing us of abnormalities in our systems.
If we are setting scaling policies on our services at 70% utilisation, we shouldn't be alerted on this, but only when the pressure exerted on our systems fails to release as part of a scaling activity (i.e. cumulative resource utilisation hitting 80%+ for X amount of time).

There are two common events that may be sent as alerts: Warnings and Breaches:

- **Warning** - A warning may be issued when values for a SLI begin to approach on the limits of a Service Level Agreement. Sending a Warning allows support personnel to proactively respond.
- **Breach** - Once an Objective is missed, a Breach Alert should be sent to those on-call. Breach Alerts are indicative of something having gone very wrong and also indicative of the need to immediately rectify a situation.

The human cost of alerting is something that should come into consideration when creating alerting policies. A lot of the time, these alerts are "paging events" which either disrupt someone's day-to-day work or their sleep schedules. As mentioned in Google's SRE Book (paraphrasing):

> ...a balanced on-call work distribution limits potential drawbacks of excessive on-call work, such as burnout or inadequate time for project work.
([Chapter 11. Being On-Call](https://landing.google.com/sre/sre-book/chapters/being-on-call/) - _Andrea Spadaccini_)

## Some Notes

In Part Two of this series, I'll be covering monitoring as code (as all good things should be) in DataDog, and how robust Infrasturcture as Code Tooling such as Terraform allows for idempotency and repeatability in setting all of this up.

I wouldn't have been able to write this without all of the great material already available:

- [Site Reliability Engineering](https://landing.google.com/sre/sre-book/toc/)
- [12 Factor App](https://12factor.net/)
- [SLIs, SLAs and SLOs](https://cloud.google.com/blog/products/gcp/sre-fundamentals-slis-slas-and-slos)