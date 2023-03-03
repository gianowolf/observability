# 15. Observability and Monitorting

## Monitoring

> In a DevOps context, monitoring mostly mean _automated monitoring_.

**Automated Monitoring** is checking the availability or behavior of a website or service, in some programmatic eway, usually on a regular schedule, with automated way of alerting engiineers if there's a problem.

### Closed Box Monitoring

```txt
Example: Static Website

The simplest monitoring check is the HTTP status code 200 successful request. 

But suppose something went wrong with the web server configuration, and although the server is working and responding HTTP 200 OK status, is actually serving a blank page. 

Simplistic monitoring check won't detect any issue because HTTP request succeeds; however, the site is not working as expected for users.
```

#### Beyond static pages

```txt
Example: More complex websites

A more complex websites might need more complex monitoring. The monitoring check might also try to log in with a known test-suer account and alert if the loggin fails. Or if the site had a search funciton, simulate clicking the search button.
```

##### Static Websites

- **For simple websites, a yes/no answer to the question 'is working?' may be sufficient**

##### Native Cloud Applications

- Is the application available everywhere in the world?
- How long does it take to load for most of users?
- What about users who mayt have slow download speeds?
- Are all the features on the website working?
- Are certain features working slowly?
- What happens to the application when a third-party service is unavailable?
- What happens when the cloud provider has an outage?

No matter how complicated these checks get, they fall into the same category of monitoring: _closed box monitoring_.

##### Limits of Closed-Box Monitoring

> **Closed-box checks observe only the external behacior of a system, without any attempt to observe what is going on inside it**.

- Only detect perdictable failures.
- Only check the behavior of the parts of the system _exposed_ to the outside.
- Are passive and reactive. Only tell you about a problem _after_ it is happened
- They can answer "what is broken" but not the more important question: "WHY?".

## Uptime

> In operations we're used to measuring the resilience and availability of our applications in _uptime_

Uptime is usually measured as a percentage, for example 99%, 99.9%, and so on. But if the service is not working for users, it does not matter what your metrics say, the service is down. _"Nines don't matter if users aren't happy"_ Charity Majors [https://red.ht/2FMZcMZ]

> _Cloud native applications are never "up"_

Distributed systems like cloud native applications are never up, they exist in a constant state of partially degraded service. This class of problems are called _gray_ failures.

Gray failure are by definition hard to detect, especially from a single point of view or witha single observation.

**So while closed-box monitoring may be a good place to start your observability journey, it's important to recognize that you should not stop there.**

## Logging 

**Logs are a series of records**, usually with timestamps to indicate when records were written, and in what order. 

### Logging in Kubernetes

Viewing a container's log is a useful debugging technique for individual containers.. You should use some form of structured data, like JSON, wich can be automatically parsed.

### Limits of Logging

- Like closed-box checks, logs can only answer questions or detect problems that can be predicted in advance.
- It can also be hard to extract information form logs, because every aplication writes logs in a different format.

> Using a standard format like JSON with a known structure will make much easier and can gratly improve the usefullness.

Logs can not give us all the information we need for a true observability, we need to look beyond logs to something much more powerfull.

## Metrics

Metrics are a more sophisticated way of gathering informatin about the services.

> A metric is a numerical measure of something.

Depending on thje app, relevant metrics might include

- Number of requests being processed
- Number of requests handled per minute
- Number of errors encountered when handling requests
- Average time it took to serve requests

It is also useful gather metrics about infrastructure:

- CPU usage of individual processes or containers
- Disk I/O activity of nodes and servers
- Inbound and outbound network traffic of machines ,clusters or load balancers.

### Metrics help answer the "why?" question

Metrics open a new dimension of monitoringg beyond simpy finding out if something is or not working. Metrics give numerical information about what is happening.

Unlike logs, metrics can easily processed in all sorts of useful ways: graphs, statistics, alerting on predefined thresholds.

```txt
For example, the monitoring system might alert you if the error rate of an application exceeds 10% for a given time period.
```

### Metrics help predict problems

> Metrics can be predictive.

Metrics can be predictive: when things go wrong, it usually does not happen all at once. Before a problem is noticeable to you or the users, an increse in some metric may indicate that trouble is on the way.

Some system even use machine learning to analyze metrics, detect anomalies, and reason about the couse. This can be helpful in complex distributed systems.

### Metrics monitor application from the inside

By contrast with closed-box checks, _metrics allow application developers to export key information about the hidden aspects of the system,_ based on their knowledge of how it actually works, and how it fails.

```txt
Tools like Prometheus, StatsD, and Graphite, or hosted services such as Datadog, New Relic, and Dynatrace, are widely used to gather and manage metrics data
```

## Tracing

Tracing follows a single user request through its whole life cycle.

When you trace an individual request from the moment the user's connection is opened to the moment it's closed, you will get a picture of how that overall latency breaks down for each stage of the request's journerry through the system.

```txt
Example: A problem to excessive packet loss oever one particular network link between the application servers and the datbase server. Withouth the request's eye view provided by distributed tracing, it is hard to find problems like this.
```

## Observability is for modern systems

> A production software system is observable to the extent that you cand understand new internal system states without havin to make arbitrary guesses, predict those failure modes in advance, or ship new code to understand that state.

In distributed systems, the ratio of somewhat predictable failure modes to novel and never-before-seen failure modes is heavily weighted, toward the bizarre and unpredictable. Those unpredictable failure modes happen so commonly and repeat rarely enough, that they outpace the ability for most teams to set up appropiate and relevant enough monitoring dashboards to easily show that state to the engineering teams for ensuring the continuous uptime, reliability, and acceptable performance of their production applications. Modern systems introduced such additional complexity that failures are harder than ever to predict, detect, and troubleshoot. 

To mitigate that complexity, engineering teams must now be able to constantly gather telemetry in flexible ways that allow them to debug issues without first needing to predict how failures may ocurr. 

Observability enables engineers to slice and dice that telemetry data in flexible ways that allow them to get to the root of any issues that occur in unparalleled ways.



```txt
some popular distributed tracing tools cinlude Zipkin, Jaeger, Pixie and Lightstep. 
```

```txt
OpenTracing framework aims to provide a standard set of APIs and libraries for distributed tracing.
```

## Observability

The observability of a system is a measure of how well instrumented it is, and how easily you can find out what is going on inside it. Some people sayt that observability is a superset of monitoring, others that observability reflects a completely diffrerent mindset from traditional monitoring.

> Monitoring tells you whether the system is working
>
> Observability prompts you to ask whyt it's not working, along with considering how well is performing
>
> Observability is about understanding what your system does and how it does it 
>
> Observability is about data: What data generate, what to collect and how to aggregate it
>
> Observability is about culture: It's a key tenet of the DevOps philosophy to close the loop between developing code and running it at scale in production