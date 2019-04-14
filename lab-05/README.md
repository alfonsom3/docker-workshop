Welcome to Lab 06 - Observe It!
===

In this lab we will dive in and begin to understand the visualizations in Jaeger and how to break down the service to service communication within the timeline graph. In addition we'll cover several advanced use cases for microservice performance management utilizing distributed tracing technology.

Tasks:

- [ ] 1 :: [Explore Jaeger](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-06#explore-jaeger)
  - [ ] 1.1 :: [Finding Traces](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-06#11-finding-traces)
  - [ ] 1.2 :: [Understanding Traces](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-06#12-understanding-traces)
  - [ ] 1.3 :: [Analyzing Calls](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-06#13-analyzing-calls)
- [ ] 2 :: [Advanced Use Cases](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-06#advanced-use-cases-with-distributed-tracing-analytics)
  - [ ] 2.1 :: [Single Music Overview](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-06#21-single-music-overview)
  - [ ] 2.2 :: [Screenshotpalooza](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-06#22-screenshotapoolza)
- [ ] 3 :: [Vendor Solution Overview](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-06#vendor-solution-overview)

Explore Jaeger
---

### 1.1 Finding Traces


> Open the Jaeger URL which was printed to the Google terminal shell in your browser and apply the following values to the fields on the left side nav bar:

* _Yellow_ :: Set service to `owners-service`
* _Purple_ :: Set minimum duration to `50ms`
* _Red_ :: Increase results limit to to `200`
* _Green_ :: Click find traces

### 1.2 Understanding Traces

> We need to break down what's here in the UI before we go any further. Let's focus on the plot chart for now. You can observe frequency of requests across the Y axis, along with complexity and latency on the X axis. A larger bubble represents a call which has more complexity (calls) in the trace. We're going to click on a call which has both latency and complexity and analyze it on the next screen shot.

![Trace Lists](lab-06/images/img01a.png)

> The very first span, also called a root span, represents the entire request body. At the end of this trace the call is considered complete and the request has been closed. Now, in the advance cases there can be additional processing which occurs for async transactions. We'll cover that use case later where we go over examples of a real production application.

> Child spans are rendered within the length of the root span, their colors can help differentiate different services being called during the duration of the request. For instance, in this call, we can see teal - which represents the `customers-service` and dark brown - which represents `visits-service`. We can click the drop downs on the individual spans to investigate the endpoints that are being accessed, along with any log data collected during the transaction.

### 1.3 Analyzing Calls

![Call Analysis](lab-06/images/img01b.png)

> As we dig deeper into the calls, we can see the span for `visits-service` is abnormally long, the subsequent span shows `49.08ms` for the database response, but the entire call took 620ms. By examining the logs we can see that by the 254ms mark, the request had been prepared for the database transaction. The reason why it took so long is we're likely related to i/o warm up on the database. If this were a consistent issue, we might do some deep profiling or database troubleshooting to see if there are any root causes.

> Distributed Tracing gives you visibility into large, complex microservice environments to help you understand the behavior of your applications _in production_. If problems start manifesting, you can hopefully analyze them before they bring the entire system down.

![Deep Call Analysis](lab-06/images/img01c.png)

Advanced Use Cases (with Distributed Tracing analytics)
---

### 2.1 Single Music Overview

> Now that we've covered some basics, we'll dive into some advanced use cases supplied through the Instana APM tool. The reason I'm using this tool for examples:
  
* I operate and manage a small microservice application (around 20 services) for Single Music - a small startup that I launched with two friends, and I monitor it with Instana. It's been in operation for a year, and I've collected **a lot** of screenshots / use cases.
* Current Open Source technologies do not offer the deep analytic capabilities I'm about to discuss; Jaeger is starting to > build some of these capabilities out (and in some cases they're building features which aren't yet available in Instana).
* Our team is really small, we don't have the time or budget to maintain our own monitoring solution. Running multiple solutions is also not in our budget. 

### 2.2 Screenshotapoolza

@todo add the screenshots

Vendor Solution Overview
---

Vendor Solutions that offer Distributed Tracing solutions (and more)

There are several vendors today which can provide similar functionality, and what it boils down to is your budget, technology requirements, and enterprise-ready requirements (on-premise, multi-tenant, saml, etc). I'll mention what I consider industry leaders in this space. If you're considering utilizing this technology in your stack you should be prepared to do several proof of concepts and quite a bit of research.

* [Instana](https://instana.com) <-- Caveat: I work here, and that's why it's #1 on the list
* [DataDog](https://datadog.com)
* [Honeycomb](https://honeycomb.io)
* [Elastic](https://elastic.co)
* [Lightstep](https://lightstep.com)
* [DynaTrace](https://dynatrace.com)

