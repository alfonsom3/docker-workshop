Welcome to Lab 02 - Kubernetes Setup
===

In this lab we'll be setting up and configuring Google Kubernetes Engine (GKE). We'll also gather tokens and keys needed to configure and integrate with Gitlab so we can build and deploy our sample microservice application

## Tasks

- [ ] 1 :: [Setup Kubernetes Cluster](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#setup-kubernetes-cluster)
  - [ ] 1.1 :: [Enable Billing](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#11-enable-billing)
  - [ ] 1.2 :: [Create Cluster](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#12-create-cluster)
  - [ ] 1.3 :: [Connect to Cloud Shell](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#13-connect-to-cloud-shell)
  - [ ] 1.4 :: [Kubernetes Admin Credentials](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#14-kubernetes-admin-credentials)
  - [ ] 1.5 :: [Modify Kube Config](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#15-modify-kube-config)
- [ ] 2 :: [Configure Kubernetes Cluster](https://gitlab.com/opentracing-workshop/lab-notes/tree/master/lab-02#configure-kubernetes-cluster)
  - [ ] 2.1 :: [Setup Cluster]()
  - [ ] 2.2 :: [Export Gitlab Credentials]()

Setup Kubernetes Cluster
---

### 1.1 Enable Billing

> Click on the Kubernetes Engine link, under the Compute section, on the left nav bar. You'll be prompted to enable billing, this is required in order to utilize the $300 credit you just recieved from Google.

![Enable Billing](lab-02/images/img01.png)

### 1.2 Create Cluster 

> This will take a few minutes, once the API is ready, you can click the `Create Cluster` button, leave all values default except the following and fill with these values:
> 
> * Number of Nodes: 4
> * Machine type: 2 vCPUs (7.5GB Memory)

![Create Cluster](lab-02/images/img02.png)

### 1.3 Connect to Cloud Shell 

> Once your cluster is finished being created, you will need to click the `Connect` button, then click `Run in Cloud Shell`, confirm that action by clicking `Start Cloud Shell`
> 
> It will take a few moments for the shell to be provisioned, once finished there will be a command line prompt waiting for you with `gcloud container clusters get-credentials ...` pre-populated for you. Press the Enter key on your keyboard to execute this command.
> 
> **IMPORTANT We will modify `~/.kube/config` after you run this command initially. If you run this command again it will overwrite the modifications you'll be performing in [Task 1-5]().**

![Connect to Cluster](lab-02/images/img03.gif)

![Get Credentials](lab-02/images/img04.png)

### 1.4 Kubernetes Admin Credentials

> To continue, we need to get the admin password from the console
> 
> We are performing this step because the docker container we'll be using to configure the cluster is not compatible with the authentication provider which comes prebaked into Cloud Shell. In order to access the Kubernetes API we must modify the configuration to use the administrator username and password instead of the auth agent.

![Click on Cluster](lab-02/images/img05.png)

> Next, click on `Show Credentials`

![Show Credentials](lab-02/images/img05a.png)

> In the last step, copy this password into your clipboard and store it somewhere safe, like a password manager or notepad.

![Save Password](lab-02/images/img05b.png)

### 1.5 Modify Kube Config

> This is one of the more difficult tasks, because we must modify the `~/.kube/config` file to use this password rather than the default (unpriveleged) credentials supplied by default. We will use the new cloud editor to modify this file, if you're comfortable with `vim` feel free to use that instead. Click on the small editor icon on the upper right corner of the shell terminal.

![Open Cloud Editor](lab-02/images/img06.png)

> This will open a new window which allows you to open and modify the `~/.kube/config` file easily.
> * Click `File`
> * Click `Open`
> * Navigate to `<homeDir>/.kube`
> * Double-Click `config`

![File Browser](lab-02/images/img06a.png)

> Next we will modify `config` to use the admin password you saved in [Task 1.4]()
> **IMPORTANT** You will need to modify the credential name, and add a new entry with our admin user/password.
>
> For the first modification, you must empty the value of the highlighted `user` value, and provide a reference to a new entry we'll be adding to the `users` array below.
>
> In this change, we'll be replace our clusters unique value with `cluster-admin`:

![Replace User Value](lab-02/images/img06b.png)

> Next, we'll add an entirely new entry to the array sequence, it should look something like this (be sure to replace the password with the one you copied earlier in step 5):

```
- name: cluster-admin
  user:
    username: admin
    password: <stickyNotePassword>
```

![Insert Entry to Array](lab-02/images/img06c.png)

> Once we've made those changes, they're automatically saved to the file. We can test our changes by executing `kubectl version` in the terminal window. We should see something like this as the response:

```
Client Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.7", GitCommit:"0c38c362511b20a098d7cd855f1314dad92c2780", GitTreeState:"clean", BuildDate:"2018-08-20T10:09:03Z"
, GoVersion:"go1.9.3", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"11+", GitVersion:"v1.11.6-gke.2", GitCommit:"04ad69a117f331df6272a343b5d8f9e2aee5ab0c", GitTreeState:"clean", BuildDate:"2019-01-04T16:
19:46Z", GoVersion:"go1.10.3b4", Compiler:"gc", Platform:"linux/amd64"}
```

> If you see `error: You must be logged in to the server (the server has asked for the client to provide credentials)` or you don't see a client and a server version, then check the syntax of the modified yaml file, it's possible a formatting error was made.
> This sanity check must pass before moving onto the next lab. Raise your hand if you're unsure whether or not your instance is ready. The example below shows both good and bad responses.

![Good and Bad](lab-02/images/img06d.png)

Configure Kubernetes Cluster
---

### 2.1 Setup Cluster

> Once you've confirmed that the modifications to `~/.kube/config` are valid, you can run the [cluster setup script](https://gitlab.com/opentracing-workshop/build-tools/blob/master/bin/setup-cluster). Run the following command in the Cloud Shell window: `docker run --rm -it -v "$HOME/.kube:/root/.kube" registry.gitlab.com/opentracing-workshop/build-tools:latest setup-cluster`. This script will take 2-3 minutes to complete, do not close the window or interrupt the script during the execution. If you encounter an error, stop here and raise your hand.
>
> When finished executing, you should see a screen like this:

![Cluster Setup Success](lab-02/images/img07.png)

### 2.2 Export Gitlab Credentials

> For the final task in this lab, we'll be exporting keys and tokens that Gitlab will use to configure and ship our microservice applications. The next command you'll run is: `docker run --rm -it -v "$HOME/.kube:/root/.kube" registry.gitlab.com/opentracing-workshop/build-tools:latest get-gitlab-settings deploy --namespace=spc > gitlab-creds.txt`
> 
> When this command has executed, you'll need to open the file browser again to view this file.
>
> **IMPORTANT You cannot use Vim or an in-shell editor for this step, as the copy/paste command wrecks the token format.**
>
> Copy the contents of `gitlab-creds.txt` to a text file, beware of OSX Notes or anything which messes with formatting, I suggest using vscode, sublime text, etc.

![Gitlab Creds](lab-02/images/img08.png)

> ##### That's it for this lab, in Lab 3 we'll be setting up Gitlab and deploying our first microservice application.

@todo - Add link to next lab




