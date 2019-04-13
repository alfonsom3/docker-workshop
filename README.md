# Observability Workshop: Kubernetes, Tracing, and Metrics

This repository holds labs which can be delivered as a self-paced or instructor-led workshop. You can read more about this workshop in the summary below.

Time Requirements:

- w/ Audience Participation: 2-3 hours
- Instructor Only: 1.5 hours

---

## Lab Overview

- [Lab 01 - Setting up Kubernetes in Docker Enterprise Edition Lab](https://gitlab.com/opentracing-workshop/ee-lab-notes/tree/master/lab-01#welcome-to-lab-01-docker-ee-setup)
- [Lab 02 - Setting up Gitlab and our Microservice Application Repository Kubernetes Integration](https://gitlab.com/opentracing-workshop/ee-lab-notes/tree/master/lab-02#welcome-to-lab-02-gitlab-and-repository-setup)
- [Lab 03 - Deploying our Microservice Application and Adding Observability](https://gitlab.com/opentracing-workshop/ee-lab-notes/tree/master/lab-03#welcome-to-lab-03-deploy-all-the-things)
- [Lab 04 - Monitoring Application Metrics with Grafana / Prometheus](https://gitlab.com/opentracing-workshop/ee-lab-notes/tree/master/lab-04#welcome-to-lab-04-monitor-it)
- [Lab 05 - Observing with Jaeger and Breaking Things](https://gitlab.com/opentracing-workshop/ee-lab-notes/tree/master/lab-05#welcome-to-lab-05-observe-it)
- [Lab 06 - Advanced Analytics and Use Cases with Automated Distributed Tracing](https://gitlab.com/opentracing-workshop/ee-lab-notes/tree/master/lab-06#welcome-to-lab-06-advanced-use-cases)

## Workshop Summary

The goal of this workshop is to familiarize application and site reliability engineers with the benefits that modern observability tools provide to the builders and curators of cloud native applications. 

In this workshop attendees will configure a RBAC-enabled vanilla K8S cluster on a Docker EE cluster, deploy Prometheus and Jaeger in support of observing and monitoring a distributed microservice application, instrument that application by introducing libraries and tooling to support capturing business metrics as well. Each attendee will configure, update, and deploy a cloud native application utilizing the following technologies:

* Docker Enterprise Kubernetes - we will utilize the free Docker EE labs provided by Docker.
* Gitlab - we will implement a CI/CD pipeline with our cloud native application in order to build and deploy our application while we integrate Prometheus, Opentracing and Jaeger into our application.
* Helm - we will utilize Helm to deploy policies, monitoring services, ingress rules, operators, and our demo application.
* Spring Petclinic Microservices - we will utilize a distributed Java/Springboot/Angular based cloud native application. We will start with an unmonitored version and work together to bring observability to our application.
* Grafana / Prometheus - we will demonstrate how to utilize prometheus operators to configure prometheus scrape targets. We will utilize micrometer to expose JMX metrics, install custom dashboards, and setup custom business metric dashboards and alerting rules.
* Jaeger / Opentracing - we will demonstrate how to utilize the Jaeger client and opentracing libraries to gain visibility into the communication happening between your microservices and database systems.

During this workshop we will discuss and explore some of the following topics:

* How Distributed Tracing works and why it's become a requirement when deploying cloud native applications.
* The pitfalls, challenges, and solutions to managing and deploying observability at scale.
* What's missing? Important aspects of our operations are't being monitored, let's find out what and, more importantly, how to fix it.
* The history of Spring Petclinic and the decision behind refactoring to no longer depend on Eureka/Zuul (Spring Cloud).
* Challenges of utilizing Opentracing implementation in non-java languages/frameworks along with some code samples.
* We'll uncover why service meshes, such as Istio, will reduce the technical costs of observability.
* Troubleshooting and Root Cause Analysis 101. We'll do our best to _not_ DDOS the conference wifi and put some stress on our newly deployed applications by simulating load from within Docker EE.
* Where is the innovation happening in observability? Let's discuss automation, AI, and the future.

Technical Requirements

* the internet and a web browser (this was tested in Firefox)
* git, code editor, openssh, vim, bash
  * basic understanding and recent experience using these tools will be beneficial when participating with the group during the lab sessions
* google cloud account with or without billing enabled*
* gitlab account*

> \* : Gitlab is free. You'll be copying a repository and pushing code/configuration changes to that fork.

Developer Requirements:

* Some programming experience [java, c, php, python, etc]
* Development tooling experience [make, mvn, yaml, vim]
* Unix-like tooling [bash, scripting, vim]

Q+A:

> Q: Will we walk through signing up for Gitlab during this workshop?
> A: Yes. However, I highly recommend you do this prior to the workshop. Google `new gitlab account` for answers.

> Q: Why Gitlab / Docker EE?
> A: Gitlab has a built-in CI/CD pipeline system and integration with K8S tooling. Docker Enterprise is a mature K8S offering and their lab environment is completely free to use.

> Q: Why can't we just use minikube? 
> A: This workshop was built using a 2018 XPS 13 w/ a quad core and 16gb ram using minikube. The project quickly outgrew this machine and operationally requires at minumum of 8 real cores and 20gb of ram (at load).

> Q: Wow, your laptop sucks. I have a 12-core alienware with 64gb of ram. Can I just use minikube?
> A: The project itself includes dependencies on large frameworks and libraries, requires heavy weight compilers, and multiple build tools. By operationalizing this project, we offload all of the network and compute requirements to the cloud - we only need our local machines to make code changes and commit them.
> A: To answer your question, yes, you can. But please, don't.