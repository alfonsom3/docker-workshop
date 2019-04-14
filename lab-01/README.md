Welcome to Lab 01 - Docker EE setup
===

In this lab, we'll get started with the Docker EE labs and setup our cluster so we can begin deploying our microservice application along with our observability tooling.

## Tasks:

- [ ] 1 :: [Launch EE labs]()
  - [ ] 1.1 :: [Fill in Details]()
- [ ] 2 :: [Configure the Cluster]()
  - [ ] 2.1 :: [Download K8S credentials]()
  - [ ] 2.1 :: [Setup Namespaces and Helm]()
  - [ ] 2.2 :: [Export Gitlab Credentials]()


Launch EE labs
---

### 1.1 Fill in Details

> Visit https://ee-labs.play-with-docker.com

![Fill in Details](lab-01/images/img01.png)

> Click `Submit` once you've completed the form

Configure the Cluster
---

### 2.1 Configure Docker EE to run K8S workloads

We need to tell Docker EE that it's permissable to schedule Kubernetes workloads on all the nodes, by default the K8S scheduler is turned off on most worker nodes because it induces additional overhead that non-k8s workloads shouldn't have to endure. However, because this is solely a K8S workshops, we need to update the nodes.

> Click on manager1 in the left panel, press enter to see the command prompt, then paste the following command:

```
curl -Ls https://bit.ly/2DcQhRz | bash
```

![Configure Nodes](lab-02/images/img02a.png)

### 2.2 Setup Namespaces and Helm

We will need to export a variable which will be used for multiple deployments during this exercise,this variable will allow us to contact the UCP (Universal Control Plane) to obtain a K8S cluster, and it'll also be used later in the exercise to configure the nginx ingress control so we can access the various services we'll be deploying.

> Underneath the terminal in your browser we see Session Information, we need to copy the value in _UCP Hostname_ into the terminal and set the `UCP_HOSTNAME` variable using the following command:

```
export UCP_HOSTNAME=<paste hostname here>
```

![Export Hostname](lab-02/images/img02b.png)

The next script will setup up our application namespace, called `spc`, configure RBAC for that namespace, so when we provide credentials to gitlab we aren't giving access to the entire cluster. This could be considered a best practice for security. Going over all the different RBAC controls we are configuring is outside of the scope of this workshop - but you can always go back later and investigate the script we are running.

We are also configuring our root namespace so we can install an nginx ingress controller and monitor tooling. The nginx ingress controller allows us to setup a proxy that will route requests for each of the services we deploy later. 

> While still on the manager1 node paste the following command:

```
docker run --rm -it \
  -e "UCP_HOSTNAME=$UCP_HOSTNAME" \
  registry.gitlab.com/opentracing-workshop/ee-build-tools:latest \
  setup-cluster
```

![Setup Cluster](lab-01/images/img02c.png)

> Next, we need to fetch the configuration settings that we are going to put into gitlab in the next lab exercise. This will allow gitlab to deploy **ONLY** to the `spc` namespace. It is recommended you copy and paste the entire output into a code editor on your desktop. (Note: Do not use OSX notes as it might autocorrect/format your text)

```
docker run --rm -it \
  -e "UCP_HOSTNAME=$UCP_HOSTNAME" \
  registry.gitlab.com/opentracing-workshop/ee-build-tools:latest \
  get-gitlab-settings deploy
```

![Gitlab Settings](lab-01/images/img02d.png)

> ##### That's it for this lab, in [Lab 2]() we'll be setting up Gitlab and deploying our first microservice application.