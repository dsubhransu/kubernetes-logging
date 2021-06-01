# kubernetes-logging
    Promtail is an agent that needs to be installed on each node running your applications or services. It detects targets (such as local log files), attaches labels to log streams from the pods, and ships them to Loki.
    Loki is the heart of the PLG Stack. It is responsible to store the log data.
    Grafana is an open-source visualization platform that processes time-series data from Loki and makes the logs accessible in a web UI.

Getting Started with the PLG Stack in Kubernetes

Let's get started with Loki with some hands-on action. In this example, we're going to use the Loki stack to visualize the logs of a Kubernetes API server in Grafana.

Before you start, make sure you have a Kubernetes cluster up and running, and Helm installed. When you're all set, we can install Loki:
Install the PLG Stack with Helm

Create a Kubernetes namespace to deploy the PLG Stack to:

$ kubectl create namespace loki

Add Loki’s Helm Chart repository:

$ helm repo add loki https://grafana.github.io/loki/charts

Run the following command to update the repository:

$ helm repo update

Deploy the Loki stack:

$ helm upgrade --install loki loki/loki-stack --namespace=loki --set grafana.enabled=true

This will install Loki, Grafana and Promtail into your Kubernetes cluster.

Retrieve the password to log into Grafana:

$ kubectl get secret loki-grafana --namespace=loki -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

The generated admin password will look like this one -> jvjqUy2nhsHplVwrX8V05UgSDYEDz6pSiBZOCPHf

Finally, execute the command below to access the Grafana UI.

$ kubectl port-forward --namespace loki service/loki-grafana 3000:80

Now open your browser, and go to http://localhost:3000.

Log in with the user name "admin" and the password you retrieved previously.
Loki in Grafana

The Grafana we installed comes with the Loki data source preconfigured. So we can start right away exploring our Kubernetes logs:
Grafana dashboard showing Loki

Next, click on the Explore tab on the left side. Select Loki from the data source dropdown.

Click on the Log labels dropdown > container > kube-apiserver
Grafana dashboard showing Loki menu with kube-apiserver

Now you should get data in the Logs window!
Grafana dashboard showing Loki Logs window

Scroll down and you will find the details on the kube-apiserver logs.
Grafana dashboard showing Loki container Logs
