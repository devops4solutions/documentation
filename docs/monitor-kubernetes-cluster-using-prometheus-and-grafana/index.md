---
title: "Monitor Kubernetes Cluster using Prometheus and Grafana"
date: "2020-12-07"
categories: 
  - "kubernetes"
  - "monitoring"
---

In this blog, we will learn how to monitor metrics for performance analysis, and also how to monitor and manage the real-time cost of Kubernetes resources using Prometheus and Grafana. We will deploy Prometheus and Grafana on our Kubernetes cluster.

Checkout my [YouTube](https://youtu.be/-i2K6KBwK3U) video for this blog

### **Prerequisite**

1. Functional [Kubernetes Cluster](https://devops4solutions.com/setup-kubernetes-cluster-in-gcp/) configured
2. Kubernetes 1.16+
3. Helm 3+
4. Kubernetes’ command-line tool, kubectl, which is the command-line interface for running commands against Kubernetes clusters.
5. Helm should be [installed](https://helm.sh/docs/intro/install/). We will install helm on windows in this article

### **Agenda**:

- Install Helm on Windows
- Deploying Prometheus using Helm charts
- Access the url using `port-forward`
- Monitoring metrics using Grafana dashboards

### **Install helm**

On windows, you need to first install [chocolate](https://chocolatey.org/install) and then using chocolate you can install the helm

Run PowerShell as an administrator and run the below command

Set-ExecutionPolicy Bypass -Scope Process -Force; \[System.Net.ServicePointManager\]::SecurityProtocol = \[System.Net.ServicePointManager\]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('[https://chocolatey.org/install.ps1'](https://chocolatey.org/install.ps1%27)))

![](https://cdn-images-1.medium.com/max/880/1*S3cV-iQ9V_1MBEtjU8nQ-w.png)

Now run the command prompt as an administrator and check if `chocolate` is installed 

choco

```
choco install kubernetes-helm
```

![](https://cdn-images-1.medium.com/max/880/1*7HnKOdmr5y0wVErVleTu-w.png)

![](https://cdn-images-1.medium.com/max/880/1*UCXSGrTx17n7wp0VMknfOQ.png)

We have successfully installed helm on our windows machine. You can skip this if helm is already installed on your machine.

### **Deploying Prometheus using Helm charts**

Now we will deploy Prometheus on a Kubernetes cluster using Helm charts:

Make sure that you have a kubernetes cluster is already running.

1. Firstly, we will add the helm repository which is required
2. Update the repository

```
helm repo add stable https://charts.helm.sh/stable
```

helm repo add prometheus-community [https://prometheus-community.github.io/helm-charts](https://prometheus-community.github.io/helm-charts)

```
helm repo update
```

![](https://cdn-images-1.medium.com/max/880/1*6i27x6PyQ3KJ2-OYYxSV1A.png)

![](https://cdn-images-1.medium.com/max/880/1*QPqFRmmaKAlb0mtwvjBHwQ.png)

You can then run `helm search repo prometheus-community` to see the charts.

helm search repo prometheus-community

![](https://cdn-images-1.medium.com/max/880/1*PPVkFZL8W4nMBkK0i7FHFQ.png)

Let’s install the Prometheus chart. You can find more details about this chart [here](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack)

helm install prometheus prometheus-community/kube-prometheus-stack

![](https://cdn-images-1.medium.com/max/880/1*8nWlUQVDjEVi4qdCyMEHsA.png)

Check the status

kubectl get pods -l "release=prometheus"

kubectl get pods

![](https://cdn-images-1.medium.com/max/880/1*Gm7u3u76fWCN1A1iyrM_PQ.png)

This chart will install additional charts as well:

- Node-exporter on all the three nodes which are in your cluster
- Grafana
- Prometheus
- Alert Manager
- Operator
- Kube State Metrics

Now you have Prometheus installed with the bundle of components required to operate it on a Kubernetes environment.

### **Access the Prometheus url on the browser**

Now lets verify our installation and try to access it on browser using `port-forward`

kubectl port-forward prometheus-prometheus-kube-prometheus-prometheus-0 8000:9090

![](https://cdn-images-1.medium.com/max/880/1*jGuI1HOOUbSqtdMOkq_39Q.png)

### MONITORING METRICS USING GRAFANA DASHBOARDS

Grafana is an open source analytics and monitoring solution. By default, Grafana is used for querying Prometheus

kubectl get svc

![](https://cdn-images-1.medium.com/max/880/1*6hK0pFdAUbBjgt28x6fuUQ.png)

#### **Port Forwarding**

Create a port forwarding to access the Grafana UI using the `kubectl port-forward`command. This command will forward the local port 8000 to port 3000 which is the default port of a Grafana pod:

Get the pod name using `kubectl get pods`

kubectl port-forward **kube-prometheus-stack-1606233825-grafana-598d4d4bd6-r7pp5** 8000:3000

1. Open it on the browser 

[http://localhost:8000/login](http://localhost:8000/login)

![](https://cdn-images-1.medium.com/max/880/1*HIykHFAk1RliVOHS_F6dcA.png)

Log in using `admin` as the username and `prom-operator`as the password:

#### **Import dashboard**

Once you are able to login to Grafana successfully you can try exploring using the default dashboard which are provided by Grafana

#### **Add Prometheus DataSource**

With this `helm chart` Prometheus data source will be added by default. You can verify as shown below:

Click on Setting ->datasources

![](https://cdn-images-1.medium.com/max/880/0*z815a-fwmUZ2Jrzd.png)

![](https://cdn-images-1.medium.com/max/880/0*zJlliXflVdWSqGu2.png)

![](https://cdn-images-1.medium.com/max/880/0*vNJwONxl8WL026ds.png)

Now we will create a dashboard which shows us all the pod details like CPU, memory, storage etc.

Grafana provides lot of dashboards which we can directly import in our Grafana instance and use it.

In this example, we will use [this](https://grafana.com/grafana/dashboards/6781) dashboard

**Import the dashboard**

Click on `+` icon -> Import and the save it

![](https://cdn-images-1.medium.com/max/880/0*1obx2uoDeeGkUh1n.png)

This is how the dashboard will look like and provide all the metrics for your pods.

![](https://cdn-images-1.medium.com/max/880/1*c9C0jVl63pzWJYetvhs1vg.png)

**Troubleshooting**

When you will uninstall this chart and install it again you may see the below error 

Failed d to install CRD crds/crd-alertmanagerconfigs.yaml: unable to recognize “”: no matches for kind “CustomResourceDefinition” in version “apiextensions.k8s.io/v1”

This is because cred’s didn’t get deleted with the uninstall step. Refer [here](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack#uninstall-chart) for more details

kubectl delete crd alertmanagerconfigs.monitoring.coreos.com  
kubectl delete crd alertmanagers.monitoring.coreos.com  
kubectl delete crd podmonitors.monitoring.coreos.com  
kubectl delete crd probes.monitoring.coreos.com  
kubectl delete crd prometheuses.monitoring.coreos.com  
kubectl delete crd prometheusrules.monitoring.coreos.com  
kubectl delete crd servicemonitors.monitoring.coreos.com  
kubectl delete crd thanosrulers.monitoring.coreos.com

### **Conclusion**:

We have successfully learnt how to monitor the Kubernetes cluster using Prometheus and visualize the dashboard using Grafana.
