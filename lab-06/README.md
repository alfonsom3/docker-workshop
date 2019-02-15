Welcome to Lab 06
---

In this lab we'll be covering distributed tracing and briefly cover Alertmanager. The goal of this is lab is to help you understand the visualizations in Jaeger and how to break down the service to service communication within the timeline graph. We'll cover some basics on Alertmanager, how it works, and how you can start creating your own Alerts.

1. Open the Jaeger URL which was printed to the Google terminal shell in your browser and apply the following values to the fields on the left side nav bar:

* _Purple_ :: Set minimum duration to `50ms`
* _Red_ :: Increase results limit to to `200`
* _Green_ :: Click find traces


![Here's Jaeger!](lab-06/images/img01a.png)