Welcome to Lab 04
---

In this lab, we will deploy our microservice application and explore some of the tooling available to use to monitor and manage our application out of the box.

1. In order for our application to work, we must deploy configuration which will instruct the `nginx-ingress` service to route and service requests at certain endpoints. This script will also deploy the database for our application. You can see the script and resources being deployed here:

* Deploy Dependency Script: https://gitlab.com/opentracing-workshop/spring-petclinic-kubernetes/blob/master/scripts/deploy_backend.sh
* Ingress Rules: https://gitlab.com/opentracing-workshop/spring-petclinic-kubernetes/blob/master/helm/spring-petclinic-ingress-rules/templates/ingress.yaml
* Database Configuration: https://gitlab.com/opentracing-workshop/spring-petclinic-kubernetes/blob/master/helm/spring-petclinic-database-server/values.yaml

We can trigger the `deploy_backend.sh` script by clicking on `CI/CD -> Pipelines` in the left nav, click on the `>>` button, and click the _Play_ button next to `deploy-dependencies`. If we click the same bubble again, we can watch the job run in real time by clicking on `deploy-dependencies`. Navigating / Using the CI/CD functionality can be a bit clunky at first pass, but once you've clicked around a few things it should become familiar enough.

![Deploy Dependencies](lab-04/images/img01.png)

2. Once the job is complete and successful, we are ready to deploy our microservice application. This next step will execute a series of actions which will deploy the Helm chart for the Spring Petclinic services. 

* Deploy Service Script: https://gitlab.com/opentracing-workshop/spring-petclinic-kubernetes/blob/master/scripts/deploy_services.sh
* Spring Petclinic Helm Chart: https://gitlab.com/opentracing-workshop/spring-petclinic-kubernetes/tree/master/helm/spring-petclinic-kubernetes

During the build process, we generated a `modules.info` file from `pom.xml`, which is used by the deploy script to determine the services to deploy via Helm.

Similar to how we deployed the Dependencies, we're going to Deploy the application this time. Click the last bubble in the stages column, and then click the _Play_ button next to `deploy-services`. You can watch this job run in real time by clicking on `deploy-services` after you've clicked `Play`. This job will take a few minutes to run, but once it's done move onto the next step!

![Deploy Services](lab-04/images/img02.png)

3. We can now see our deployment in GKE, so switch back to the Google Console and start investigating your application. From here, you can inspect your application services, metrics, and logs. You can even load your application in your browser and begin interacting with it! *NOTE* Be sure to replace `INGRESS_IP` with the Ingress IP address you saved from `gitlab-creds` earlier.

* Visit http://admin.spc.INGRESS_IP.nip.io for the Spring Boot Admin control panel.
* Visit http://spc.INGRESS_IP.nip.io for the application itself!

![GKE Has Services](lab-04/images/img03a.png)
![Spring Boot Admin](lab-04/images/img03b.png)
![Spring Boot Admin 2](lab-04/images/img03d.png)
![Spring Pet Clinic](lab-04/images/img03c.png)

Feel free to spend some time here and please ask questions! We just completed roughly two weeks worth of integration work in less than an hour, there is a lot to ingest here. Once you've soaked in the awesomeness, let's check out what it's going to take to add monitoring and tracing to our application.

4. We're going to create a new merge request in Gitlab and deploy changes which will begin exporting metrics with [Micrometer](https://micrometer.io/) and distributed tracing data with [OpenTracing](https://opentracing.io/).

Micrometer provides one important aspects of modern application observability, which is the ability to export the runtime characteristics of our application such as memory and CPU usage, along with business specific metrics such as Requests Per Second and custom metrics we can implement within our applications. We'll use a tool called [Prometheus](https://prometheus.io/) to collect those metrics, and another tool called [Grafana](https://grafana.com/) to visualize them.

Opentracing provides an additional capability, which is exporting the timing and metadata around the requests which are being served by the services within your microservice application. This data can be used to quickly determine if a particular service is taking longer than expected to respond to requests. There are other use-cases for this data, such as analyzing diffs between requests and drilling down into the performance characteristics for a given user or entry point. We can also analyze the performance of 3rd party systems and APIs to determine the impact they are having on our own services. We'll be utilizing [Jaeger](https://www.jaegertracing.io/) to analyze the data exported from our applications.

Let's create the merge request, so head back to Gitlab and follow these steps.

* Click `Repository` on the left-nav, then click `Compare`
* Change `Source` to `add-oss-monitoring`
* Click `Create merge request`
* Leave all settings at their default, scroll and click `Submit merge request`

![Create Merge Request](lab-04/images/img04.png)
![Submit Merge Request](lab-04/images/img04a.png)

5. Code Review Time!