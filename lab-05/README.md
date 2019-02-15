Welcome to Lab 05
---

In this lab we must now provide the infrastructure necessary to collect, manage, and visualize the observability data (metrics and traces) that our applications are now emitting. In this lab we will deploy and configure the following technologies:

* [Prometheus](https://prometheus.io/)
* [Grafana](https://grafana.com/)
* [Jaeger](https://www.jaegertracing.io/)

At the end of this exercise we'll have deployed several dashboards to Grafana and traced transactions through our microservice end-to-end with Jaeger.

Before we get started, let's open up `Spring Boot Admin` again or refresh the page, and click on one of the application services. You'll immediatly notice a tremendous amount of more data (see screen shot below). The configuration changes we've made and libraries we introduced to our application has also enabled data to be collected automatically by `Spring Boot Admin`. There is no historical or analytic capabilities in this application, so it's only useful for "real-time" monitoring at best.

![Spring Boot Admin Reboot](lab-05/images/img00a.png)

The benefit to using the aforementioned tools is historical analytics, but it comes at the price of operational complexity. You must ensure your applications are configured and built for observability, and you must budget for the operation of the above systems - which have significant CPU/Memory/Disk requirements. We'll cover alternatives after this lab, and the benefits of using a managed solution can have when running systems that aren't proof of concept/demo environments.

1. Deploy infrastructure for monitoring

In this step we'll go back to the Google Console and run the `setup-monitoring` command (similar to the `setup-cluster` step in Lab 2-7). Open the console terminal and run this command in the terminal: 

`docker run --rm -it -v "$HOME/.kube:/root/.kube" registry.gitlab.com/opentracing-workshop/build-tools setup-monitoring`

At the conclusion of this command you'll see a list of URLs which should begin working within a minute or two. While you're waiting you can [view the script](https://gitlab.com/opentracing-workshop/build-tools/blob/master/bin/setup-monitoring) which installed these services. It's a fairly straight-forward setup, the complexity is baked into the helm charts - which we could spend two hours on talking about alone.

![Ingress URLs](lab-05/images/img01.png)

2. In this task, we'll be getting familiar with Grafana, logging in, and exploring some of the pre-installed dashboards.

* Visit the Grafana URL provided by the installation script - https://grafana.spc.INGRESS_IP.nip.io
* Login to the service with the username and password: `admin`

![Grafana Login is not UX](lab-05/images/img02a.png)

Once logged in, feel free to explore a few of the pre-installed dashboards, these were installed via the Prometheus operator.

![Kubernetes Capacity Planning](lab-05/images/img02b.png)
![Node Stats](lab-05/images/img02c.png)

3. Now that we have the hang of Grafana and the strange UI, we can setup some additional dashboards which will instrument our Java applications and MySQL.

* Hover over the `+` symbol on the left nav, and click Import.
* Input `4701` into the `Grafana.com Dashboard`, and click on any empty space on the page (this will load Options)
* Select `prometheus` from the `Prometheus` dropdown and click `Import`

![Import Micrometer Dashboard Pt 1](lab-05/images/img03a.png)
![Import Micrometer Dashboard Pt 2](lab-05/images/img03b.png)

**Optional**

4. If you click on the `Instance` drop-down, you'll notice IP addresses. These are the individual services which make up the Spring Petclinic Microservice demo. It's not very practical to have just IP addresses in here, you'd have to go look up the Pod IP every time you wanted to check your service, and those Pod IDs are ephemeral, they'll change every time you do a deployment.

We can band-aid this within Prometheus, although the fix is far from optimal (more on that later).

![Micrometer Metrics](lab-05/images/img04a.png)

Band-aid steps:

* Click the Settings Wheel in the upper right corner of the dashboard
* _Teal_ :: Click `Variables` on the left settings panel
* _Purple_ :: Click `New`

![Adding Pod Names 1](lab-05/images/img04b.png)

* _Red_ :: Input `pod` into the `Name` and `Label` field
* _Purple_ :: Input `label_values(jvm_memory_used_bytes{application="$application", instance="$instance"}, pod)` into the `Query` field
* _Teal_ :: Click `Add`

![Add Pod Names 2](lab-05/images/img04c.png)

* _Yellow_ :: Move the `$pod` variable under `$instance`
* _Red_ :: Click `Save` on the left settings panel
* _Teal_ :: Click `Back` button on the upper right corner of the dashboard

![Add Pod Names 3](lab-05/images/img04d.png)

As you switch between `Instance` IP addresses the `pod` name will update accordingly. As you can guess, a more optimal solution would be to populate the `pod` name with all the instances running micrometer. This repository is open to contributions/MRs. :)

![Pod Name Added](lab-05/images/img04e.png)

5. It's Jaeger Time!