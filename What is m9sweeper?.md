## What is m9sweeper?

If you are a beginner or experienced administrator, [m9sweeper](https://m9sweeper.io/docs/latest/docs/overview/) is a one-stop-shop to secure a kubernetes environment in a matter of hours, not days or weeks, as well as to continue to assess and report on your kubernetes security posture.
<br>

## Installation

To install m9sweeper, the first thing you will need to to is add it's helm repository to your computer. To do this, run the following command:
```
helm repo add m9sweeper https://m9sweeper.github.io/m9sweeper
```
<br>

You will then receive a messsage:

```
"m9sweeper" has been added to your repositories
```
<br>

## Editing the values.yaml file

Locate the ```values.yaml``` then edit the following:
```
global:
  apiKey: "DemoKey"

dash:
  init:
    superAdminEmail: "demo@demomail.com"
    superAdminPassword: "DemoAdminPassword"
```
<br>

Next, we will install m9sweeper to the k8s cluster using helm. In this command you are instructing helm to do several things:

* Install m9sweeper from the m9sweeper helm repo you added earlier. We use the ```upgrade``` command instead of the ```install``` command as it is more repeatable later on when you upgrade m9sweeper.

* Create a new namespace called m9sweeper-system and install m9sweeper into that.

* Use the values.yaml file you edited previously to defined the values that Helm will use when installing m9sweeper.

* Wait for the installation to be completed before completing the command execution.
<br>

The commands should look like this:
```
helm repo add m9sweeper https://m9sweeper.github.io/m9sweeper && \
helm repo update && \
helm upgrade \
 m9sweeper m9sweeper/m9sweeper \
--install \
--create-namespace \
--namespace m9sweeper-system \
--values /root/values.yaml \
--wait --timeout 10m0s \
--set-string dash.init.superAdminEmail="demo@demo.com" \
--set-string dash.init.superAdminPassword="DemoPassword" \
--set 'dash.ingress.hosts[0]=m9sweeper.dash-url.fr'
```
<br>

You can monitor the installation process by using the command:
```
watch -n1 kubectl get pods -n m9sweeper-system
```
<br>

Wait for the installation to complete before attempting to move to the next step. This may take several minutes. You should get the following prompt when it is installed:
```
Release "m9sweeper" does not exist. Installing it now.
NAME: m9sweeper
LAST DEPLOYED: Mon Feb 13 03:45:34 2023
NAMESPACE: m9sweeper-system
STATUS: deployed
REVISION: 1
TEST SUITE: None
```
<br>

Now that m9sweeper has been installed, you can access the dashboard and login using the credentials you've on the ```values.yaml``` file. In order to access the dashboard, you must first port forward the dash service with the following commands:
```
ubectl -n m9sweeper-system port-forward service/m9sweeper-dash 80:3000 --address 0.0.0.0
```
<br>

Once you've login using the credentials, it will then prompt you to this dashboard:

![image](https://github.com/user-attachments/assets/e0d0fbad-1141-46bb-a5a0-4639cf9a44a6)
<br>

Open the *default-cluster* to view the cluster overview page. You can see here the total number of images used by the running pods and the count of vulnerabilities that are already detected.

![image](https://github.com/user-attachments/assets/03169c55-4f44-4429-a56c-68802e500ce5)
<br>

Using the navigation menu on the left side of your screen, open the *Images* page. This page shows all the images registered in the cluster.

![image](https://github.com/user-attachments/assets/9cbcaed5-ede6-4137-9a1a-29eb9ad372e2)
<br>


*References:*

* https://m9sweeper.io/docs/latest/docs/overview/

* https://medium.com/@ninapepite/free-security-tools-for-kubernetes-introduction-to-m9sweeper-75db21f848da









