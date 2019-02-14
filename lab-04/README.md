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

3. We can now see our deployment in GKE, so switch back to the Google Console and start investigating your application. From here, you can inspect your application services, metrics, and logs. You can even load your application in your browser and begin interacting with it!

* Visit http://admin.spc.<INGRESS_IP>.nip.io for the Spring Boot Admin control panel.
* Visit http://spc.<INGRESS_IP>.nip.io for the application itself!