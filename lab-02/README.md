Welcome to Lab 02
---

In this lab we'll be setting up and configuring Google Kubernetes Engine (GKE). We'll also gather tokens and keys needed to configure and integrate with Gitlab so we can build and deploy our sample microservice application

1. Click on the Kubernetes Engine link, under the Compute section, on the left nav bar. You'll be prompted to enable billing, this is required in order to utilize the $300 credit you just recieved from Google.

![Enable Billing](lab-02/images/img01.png)

2. This will take a few minutes, once the API is ready, you can click the `Create Cluster` button, leave all values default except the following and fill with these values:

* Number of Nodes: 4
* Machine type: 2 vCPUs (7.5GB Memory)

![Setup Cluster](lab-02/images/img02.png)

3. Once your cluster is finished being created, you will need to click the `Connect` button, then click `Run in Cloud Shell`, confirm that action by clicking `Start Cloud Shell`

![Connect to Cluster](lab-02/images/img03.gif)

4. Once the shell has been provisioned, there will be a command line prompt waiting for you with `gcloud container clusters get-credentials ...` pre-populated for you. Press the Enter key on your keyboard to execute this command. 

![Get Credentials](lab-02/images/img04.png)

!!!IMPORTANT!!! We will modify `~/.kube/config` after you run this command. If you run this command again it will overwrite the modifications we've made. In the future, when opening cloud shell, click `OK` instead.

5. We need to get the admin password from the console, as this will allow us to do administrative actions in the console later.

![Click on Cluster](lab-02/images/img05.png)

Next, click on `Show Credentials`

![Show Credentials](lab-02/images/img05a.png)

In the last step, copy this password into your clipboard and store it somewhere safe, like a password manager or notepad.

![Save Password](lab-02/images/img05b.png)

6. This is one of the more difficult tasks, because we must modify the `~/.kube/config` file to use this password rather than the default (unpriveleged) credentials supplied by default. We will use the new cloud editor to modify this file, if you're comfortable with `vim` feel free to use that instead. Click on the small editor icon on the upper right corner of the shell terminal.

![Open Cloud Editor](lab-02/images/img06.png)

This will open a new window which allows you to open and modify the `~/.kube/config` file easily.

* Click `File`
* Click `Open`
* Navigate to `<homeDir>/.kube`
* Double-Click `config`

![File Browser](lab-02/images/img06a.png)

We will modify `config` to use the admin password you copied in step 5. You will need to modify the credential name, and add a new entry with our admin user/password.

For the first modification, you must empty the value of the highlighted `user` value, and provide a reference to a new entry we'll be adding to the `users` array below.

In this change, we'll be replace our clusters unique value with `cluster-admin`:

![Replace User Value](lab-02/images/img06b.png)

In this next change, we'll add an entirely new entry to the array sequence, it should look something like this (be sure to replace the password with the one you copied earlier in step 5):

```
- name: cluster-admin
  user:
    username: admin
    password: <stickyNotePassword>
```

![Insert Entry to Array](lab-02/images/img06c.png)

Once we've made those changes, they're automatically saved to the file. We can test our changes by executing `kubectl version` in the terminal window. We should see something like this as the response:

```
Client Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.7", GitCommit:"0c38c362511b20a098d7cd855f1314dad92c2780", GitTreeState:"clean", BuildDate:"2018-08-20T10:09:03Z"
, GoVersion:"go1.9.3", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"11+", GitVersion:"v1.11.6-gke.2", GitCommit:"04ad69a117f331df6272a343b5d8f9e2aee5ab0c", GitTreeState:"clean", BuildDate:"2019-01-04T16:
19:46Z", GoVersion:"go1.10.3b4", Compiler:"gc", Platform:"linux/amd64"}
```

If we see `error: You must be logged in to the server (the server has asked for the client to provide credentials)` or we don't see a client and a server version, then check the syntax of the modified yaml file, it's possible a formatting error was made.

This sanity check must pass before moving onto the next lab. Raise your hand if you're unsure whether or not your instance is ready.