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

4. Once the shell has been provisioned, there will be a command line prompt waiting for you with `gcloud container clusters get-credentials ...` pre-populated for your. Press the Enter key on your keyboard to execute this command.

![Get Credentials](lab-02/images/img04.png)
