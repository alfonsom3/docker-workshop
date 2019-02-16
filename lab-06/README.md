Welcome to Lab 06
---

The goal of this is lab is to help you understand the visualizations in Jaeger and how to break down the service to service communication within the timeline graph. Once we've covered some basics, we'll dive into some advanced use cases supplied through the Instana APM tool, as I built and manage a production microservice application called Single Music and monitor it with a commercial tool since we are an extremely small team and don't have the budget nor personal to maintain our own monitoring solution. There are several vendors today which can provide similar functionality, and what it boils down to is your budget, technology requirements, and enterprise-ready requirements (on-premise, multi-tenant, saml, etc). We'll cover some of the best vendors in this space near the end of this lab.

1. Open the Jaeger URL which was printed to the Google terminal shell in your browser and apply the following values to the fields on the left side nav bar:

* _Yellow_ :: Set service to `owners-service`
* _Purple_ :: Set minimum duration to `50ms`
* _Red_ :: Increase results limit to to `200`
* _Green_ :: Click find traces

We need to break down what's here in the UI before we go any further. Let's focus on the plot chart for now. You can observe frequency of requests across the Y axis, along with complexity and latency on the X axis. A larger bubble represents a call which has more complexity (calls) in the trace. We're going to click on a call which has both latency and complexity and analyze it on the next screen shot.

![Trace Lists](lab-06/images/img01a.png)

The very first span, also called a root span, represents the entire request body. At the end of this trace the call is considered complete and the request has been closed. Now, in the advance cases there can be additional processing which occurs for async transactions. We'll cover that use case later where we go over examples of a real production application.

Child spans are rendered within the length of the root span, their colors can help differentiate different services being called during the duration of the request. For instance, in this call, we can see teal - which represents the `customers-service` and dark brown - which represents `visits-service`. We can click the drop downs on the individual spans to investigate the endpoints that are being accessed, along with any log data collected during the transaction.

![Call Analysis](lab-06/images/img01b.png)

As we dig deeper into the calls, we can see the span for `visits-service` is abnormally long, the subsequent span shows `49.08ms` for the database response, but the entire call took 620ms. By examining the logs we can see that by the 254ms mark, the request had been prepared for the database transaction. The reason why it took so long is we're likely related to i/o warm up on the database. If this were a consistent issue, we might do some deep profiling or database troubleshooting to see if there are any root causes.

Distributed Tracing gives you visibility into large, complex microservice environments to help you understand the behavior of your applications _in production_. If problems start manifesting, you can hopefully analyze them before they bring the entire system down.

![Deep Call Analysis](lab-06/images/img01c.png)

2. Advanced Use Cases with Distributed Tracing analytics

3. Vendor Solutions that offer Distributed Tracing solutions (and more)

* [Instana](https://instana.com)
* [DataDog](https://datadog.com)
* [Honeycomb](https://honeycomb.io)
* [Elastic](https://elastic.co)
* [DynaTrace](https://dynatrace.com)

