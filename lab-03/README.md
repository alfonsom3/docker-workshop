Welcome to Lab 03
---

In this lab we'll be setting up Gitlab, copying and configuring our repository, also we'll setup the CI/CD process to deploy to our newly configured K8S cluster in GKE.

1. To get started, you'll need to either sign into Gitlab with your account, or create a new account. We're going to create a new account for those here who may be new to Gitlab. You may need to help train Google's AI - especially if the entire room starts creating accounts. Be sure to use a valid email address because you'll need to confirm it in order to use your account. You may go to the next step if you already have an account.

![Sign Up for Gitlab Account](lab-03/images/img01.png)

2. Login to your account and then create a new project.

![Create New Project](lab-03/images/img02.png)

3. Click the `Import Project` tab, and click `Repo by URL`. You will be cloning the `spring-petclinic-kubernetes` repository next.

![Import Project](lab-03/images/img03.png)

4. Fill out the following values:

* Git Repository URL: `https://gitlab.com/opentracing-workshop/spring-petclinic-kubernetes`
* Project Name: `spring-petclinic-kubernetes`
* Set the Project to `Public`
* Click `Create Project`

![Create Project](lab-03/images/img04.png)

5. Configure CI/CD settings and parameters

First we must populate the Ingress IP as a build time variable - this will configure our applications to respond to requests from the outside and route accordingly. You will find this IP address in the `gitlab-creds` file you exported and saved earlier. The `Ingress IP:` field is near the bottom of this document. If there is no IP address there, you can find the IP address in the google console by clicking on `Services`. An example can be found below:

![Service Ingress IP](lab-03/images/img05a.png)

In the above example the Ingress IP address is surrounded by a green box. *DO NOT USE THIS IP ADDRESS* your application will not work if you do.

In the example below, you may have to scroll down to find `Settings`:

* Click `Settings`
* Click `CI/CD`

![CI/CD Settings](lab-03/images/img05b.png)

* Click `Expand` next to `Environment Variables`
* Fill in `INGRESS_IP` and the value you've collected from either `gitlab-creds` or from the Service Ingress IP in the Google Console.
* Click `Save variables`

![Input Ingress IP](lab-03/images/img05c.png)

6. In the next step you will be configuring the Kubernetes Integration in Gitlab by providing the other values which are exposed in the `gitlab-creds` document

* Click `Operations` and the `Kubernetes` Sub-menu item on the left nav-bar.

![Kubernetes Ops](lab-03/images/img06a.png)

* Click `Add Kubernetes Cluster`

![Add Kubernetes](lab-03/images/img06b.png)

Fill in the following values, taking data from `gitlab-creds` which was created earlier in Lab 02:

* Kubernetes Cluster Name: `my-cluster`
* Environment Scope: `*`
* API URL: <Copy URL from `gitlab-creds`>
* CA Certificate: <Copy CA CERT from `gitlab-creds`>
* Token: <Copy Token from bottom of `gitlab-creds`>
* Project Namespace: `spc`
* RBAC-enabled cluster: `checked`

Once you've populated these values, click `Add Kubernetes Cluster`, you'll validate these entries in the next step during our first build.

![For real - Add Kubernetes](lab-03/images/img06c.png)

7. Click `CI/CD` in the left-nav menu, then click on `Run Pipeline`

![Run Pipeline](lab-03/images/img07a.png)

Once the pipelines has begun executing, click the `Play` button next to `validate-k8s`

![Run Validate Job](lab-03/images/img07b.png)

To see this job run in real-time, click the `validate-k8s` button

![See Validate Job Run](lab-03/images/img07c.png)

This job should complete and be successful. If you do not see the `INGRESS_IP` value, and both the server and client versions for `kubectl` **STOP HERE** and raise your hand before moving on to the next lab