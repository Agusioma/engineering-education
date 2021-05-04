### Getting started with Kubernetes Clusters

A **Kubernetes Cluster** is a  manages containers which consists of nodes that work together to perform a certain operation. In this article, we will first look at how to install Kubernetes on a Cloud Provider and locally in our machines.
After successfully building an application container, we usually get the motivation to deploy it into a reliable and scalable distributed system. This can be achieved by using Kubernetes cluster.

There are several cloud-based service providers that make it easy to create Kubernetes clusters using few command-based instructions. This option is highly recommended if one is getting started with Kubernetes because  it's better to quickly get started with
Kubernetes, learn about it, and then, later on, learn how to install it in our physical machines. As much as using cloud-based solutions requires us to pay and have an active network connection to the cloud,  the **minikube** tool only creates a *single-node cluster*, which doesn’t demonstrate all aspects of a complete Kubernetes cluster.

#### Installing in Public Cloud Providers

In this section, We will look at how to install Kubernetes in these Cloud Providers:

- Google Cloud Platform.
- Microsoft Azure

1. Google Cloud Platform

It offers a hosted *Kubernetes-as-a-Service* called
**Google Container Engine (GKE)**. One needs to sign up for [the platform's Platform account](https://console.cloud.google.com/freetrial?_ga=2.256403528.294839319.1619953021-1551188299.1619953021) and install the [gcloud tool](https://cloud.google.com/sdk/docs/install).

> NOTE: Billing has to be enabled for us to use the platform.

Once everything is in place, we set our default zone:

```bash
     $ gcloud config set compute/zone <timezone>
```     

Then create a cluster:

```bash
     $ gcloud container clusters create <cluster>
```

> It may take a few minutes

We can get the cluster's credentials using this command:

```bash
     $ gcloud auth application-default login
```

For complete and detailed instructions, read the documentation [here](https://cloud.google.com/Kubernetes-engine/docs/how-to/).

2. Microsoft Azure

To get started, we use the built-in Shell in the portal by clicking the shell icon in the toolbar:

![Shell icon](shell.png)

We can also install the  shell in our local machines using the instructions contained [here](https://docs.microsoft.com/cli/azure/install-azure-cli).

Once the shell is up and running, we can create a resource group using this command:

```bash
     $ az group create --name=<group-name> --location=<location>
```

We then create a cluster using:

```bash
     $ az acs create --orchestrator-type=Kubernetes \
        --resource-group=<group-name> --name=<cluster-name>
```

After the cluster is created, we can get credentials
for the cluster using this command:

```bash
     $ az acs Kubernetes get-credentials --resource-group=<group-name> --name=<cluster-name>
```

Further instructions can be found in the [Azure documentation](https://docs.microsoft.com/en-us/azure/aks/Kubernetes-walkthrough).

3. Installing Kubernetes Locally

> Too use minikube, we need to have [hypervisor](https://www.vmware.com/topics/glossary/content/hypervisor#:~:text=A%20hypervisor%2C%20also%20known%20as,such%20as%20memory%20and%20processing.) installed on our machines.

The minikube tool can be found [here](https://github.com/Kubernetes/minikube) where links to binaries for one's Operating System of choice can be found. Once the tool is installed, we can create a local cluster using this command:

```bash
     $ minikube start
```

We can pause it using:

```bash
     $ minikube pause
```

When done with it, we halt using:

```bash
     $ minikube stop
```    

To remove the cluster, run:

```bash
$ minikube delete
```

More instructions can be found [here](https://minikube.sigs.k8s.io/docs/start/)

#### The Kubernetes Client

`kubectl` is a command-line tool used to interact with the Kubernetes API. It is used to manage Kubernetes
objects and also helps us monitor the health of the cluster.

We can check a cluster version using:

```bash
$ kubectl version
```

We can monitor the health of the cluster using:

```bash
$ kubectl get componentstatuses
```

The output will appear as shown:

```bash
NAME                STATUS      MESSAGE             ERROR
scheduler           Healthy     ok                  
controller-manager  Healthy     ok
etcd-0              Healthy     {"health": "true"}

```
We have a glimpse of the components making up the Kubernetes cluster namely:

- `controller-manager` - runs various controllers that regulate
behavior in the cluster,
- `scheduler` - places pods in appropriate nodes in our cluster.
- `etcd` - serves as the storage for the cluster and all the API objects

To list all nodes in a cluster, we run this command:

```bash
$ kubectl get nodes
```

We will get all the node names, their health statuses, and age.

To get more information about a specific node, we run this:

```bash
$ kubectl describe nodes <node-name>
```

#### Common Cluster Components

We are going to look at a few components that make up a Kubernetes Cluster namely:

- Kubernetes Proxy
- Kubernetes DNS
- Kubernetes UI

1. Kubernetes Proxy

It is responsible managing network communications from different sources with our clusters. An API object named `DaemonSet` that is used to achieve this. We can see the proxies in our clusters by running this command:

```bash
$ kubectl get daemonSets --namespace=kube-system kube-proxy
```

Read more on Kubernetes Proxy [here](https://Kubernetes.io/docs/concepts/cluster-administration/proxies/)

2. Kubernetes DNS

Kubernetes runs a DNS server that provides easy identification of services in a cluster by assigning them names thereby allowing their functionality to be accessed without getting to know their IP addresses. 
To view the servers, use this command:

```bash
$ kubectl get deployments --namespace=kube-system kube-dns
```
Read more on Kubernetes DNS [here](https://Kubernetes.io/docs/concepts/services-networking/dns-pod-service/)

3. Kubernetes UI

This is a web-based graphical user interface and to see it, run this command:

```bash
$ kubectl get deployments --namespace=kube-system Kubernetes-dashboard
```

We can then use the `kubectl proxy` to access the server on http://localhost:8001/api/v1/namespaces/Kubernetes-dashboard/services/https:Kubernetes-dashboard:/proxy/:

```bash
$ kubectl proxy
```
Read more on Kubernetes UI [here](https://Kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)

#### Conclusion

We have gone through a very basic introduction to Kubernetes clusters. View content on the links attached to get more insights. Have a good one.