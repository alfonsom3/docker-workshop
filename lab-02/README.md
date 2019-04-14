Welcome to Lab 02 - Gitlab and Repository Setup
===

In this lab we'll be setting up Gitlab, copying and configuring our repository, also we'll setup the CI/CD process to deploy to our newly configured K8S cluster in GKE.

## Tasks

- [ ] 1 :: [Setup Application Repository](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#setup-application-repository)
  - [ ] 1.1 :: [Sign Up / Login to Gitlab](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#11-sign-up-login-to-gitlab)
  - [ ] 1.2 :: [Create New Project](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#12-create-new-project)
- [ ] 2 :: [Configure CI/CD](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#configure-cicd)
  - [ ] 2.1 :: [Setup Environment Variables](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#21-setup-environment-variables)
  - [ ] 2.2 :: [Configure Kubernetes Integration](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#22-configure-kubernetes-integration)
  - [ ] 2.3 :: [Run Pipeline Validation](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#23-run-pipeline-validation)

Setup Application Repository
---

### 1.1 Sign Up / Login to Gitlab

> To get started, you'll need to either sign into Gitlab with your account, or create a new account. We're going to create a new account for those here who may be new to Gitlab. You may need to help train Google's AI - especially if the entire room starts creating accounts. Be sure to use a valid email address because you'll need to confirm it in order to use your account. You may go to the next step if you already have an account.

![Sign Up for Gitlab Account](lab-02/images/img01.png)

### 1.2 Create New Project

![Create New Project](lab-02/images/img02.png)

> Click the `Import Project` tab, and click `Repo by URL`. You will be cloning the `spring-petclinic-kubernetes` repository next.

![Import Project](lab-02/images/img03.png)

> Fill out the following values:
>
> * Git Repository URL: `https://gitlab.com/opentracing-workshop/spring-petclinic-kubernetes`
> * Project Name: `spring-petclinic-kubernetes`
> * Set the Project to `Public`
> * Click `Create Project`

![Create Project](lab-02/images/img04.png)

Configure CI/CD
---

### 2.1 Setup Environment Variables

> First we must populate the UCP_HOSTNAME as a build time variable - this will configure our applications to respond to requests from the outside and route accordingly. You will find this UCP Hostname in the `gitlab-creds` data you exported and saved earlier. The `UCP Hostname` field is near the bottom of this document.

> In Gitlab, you may need to scroll down to find `Settings` in the left nav bar:
> 
> * Click `Settings`
> * Click `CI/CD`

![CI/CD Settings](lab-02/images/img05b.png)

> * Click `Expand` next to `Environment Variables`
> * Fill in `UCP_HOSTNAME` and the value you've collected from `gitlab-creds`.
> * There is a bug in Gitlab currently which may cause a red warning to appear. If this happens, click the slider to uncheck `Mask`
> * Click `Save variables`

![Input UCP Hostname]()

## 2.2 Configure Kubernetes Integration

> In the next step you will be configuring the Kubernetes Integration in Gitlab by providing the other values which are exposed in the `gitlab-creds` output
> 
> * Click `Operations` and the `Kubernetes` Sub-menu item on the left nav-bar.

![Kubernetes Ops](lab-02/images/img06a.png)

> * Click `Add Kubernetes Cluster`

![Add Kubernetes](lab-02/images/img06b.png)

**IMPORTANT** The default page shown is Create new Cluster on GKE - which we will not be doing.

> Click on `Add existing Cluster`

> Fill in the following values, taking data from `gitlab-creds` which was created earlier in the first lab:
> 
> * Kubernetes Cluster Name: `ee-cluster`
> * Environment Scope: `*`
> * API URL: `<Copy URL from gitlab-creds>`
> * CA Certificate: `<Copy CA CERT from gitlab-creds>`
> * Token: `<Copy Token from bottom of gitlab-creds>`
> * Project Namespace: `spc`
> * RBAC-enabled cluster: `checked`
> 
> Once you've populated these values, click `Add Kubernetes Cluster`, you'll validate these entries in the next step during > our first build.

![For real - Add Kubernetes](lab-02/images/img06c.png)

### 2.3 Run Pipeline Validation

> Click `CI/CD` in the left-nav menu, then click on `Run Pipeline`, then click `Run Pipeline` again.

![Run Pipeline](lab-02/images/img07a.png)

> Once the pipelines has begun executing, click the `Play` button next to `validate-k8s`

![Run Validate Job](lab-02/images/img07b.png)

> To see this job run in real-time, click the `validate-k8s` button

![See Validate Job Run](lab-02/images/img07c.png)

> This job should complete and be successful. If you do not see the `INGRESS_IP` value, and a list of resources from `kubectl` **STOP HERE** and raise your hand before moving on to [Lab 3]().