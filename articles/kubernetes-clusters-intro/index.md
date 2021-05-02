### Getting started with Kubernetes Clusters

A **Kubernetes Cluster** is a dynamic system that places and manages containers, grouped in pods, running on nodes, along with all the interconnections and communication channels working to perform a certain operation. In this article, we will first look at how to install Kubernetes on a Cloud Provider and locally in our machines.
After successfully building an application container, we usually get the motivation to deploy it into a reliable and scalable distributed system. To do this, we need a working Kubernetes cluster.

There are several cloud-based service providers that make it easy to create Kubernetes clusters using few command-based instructions. This option is highly recommended if one is getting started with Kubernetes because  it's better to quickly get started with
Kubernetes, learn about it, and then, later on, learn how to install it in our physical machines. As much as using cloud-based solutions requires us to pay and have an active network connection to the cloud,  the **minikube** tool only creates a *single-node cluster*, which doesn’t demonstrate all aspects of a complete Kubernetes cluster.

#### Installing Kubernetes Public Cloud Providers

We will look at how to install Kubernetes on the:

- Google Cloud Platform.
- Microsoft Azure

1. Google Cloud Platform

It offers a hosted *Kubernetes-as-a-Service* called
**Google Container Engine (GKE)**. To use it, you need a [Google Cloud Platform account](https://console.cloud.google.com/freetrial?_ga=2.256403528.294839319.1619953021-1551188299.1619953021) with billing enabled and the [gcloud tool](https://cloud.google.com/sdk/docs/install) installed.
Once you have gcloud installed, set a default zone:

```bash
     $ gcloud config set compute/zone <your-timezone>
```     

Then create a cluster:

```bash
     $ gcloud container clusters create <your-cluster>
```

> It may take a few minutes

When the cluster is ready you can get its credentials
using this command:

```bash
     $ gcloud auth application-default login
```

If you experience any trouble or you want the complete installation instructions, then read there [documentation](https://cloud.google.com/Kubernetes-engine/docs/how-to/).

2. Microsoft Azure

As part of the Azure Container Service, it also offers Kubernetes-as-a-Service. To get started with it, we use the built-in Shell in the Azure portal. You can activate the
shell by clicking the shell icon in the toolbar:

![Shell icon](shell.png)

The shell has the **az CLI** tool already
configured to work with your Azure environment. You can also install the az command-line interface (CLI) in your local
machine using the instructions contained [here](https://docs.microsoft.com/cli/azure/install-azure-cli).

Once the shell is up and running, we can create a resource group using this command:

```bash
     $ az group create --name=<your-group-name> --location=<your-location>
```
You should get a json output as in the format below:

```json
{
  "id": "<resource-path>",
  "location": "<your-location>",
  "managedBy": null,
  "name": "<your-group-name>",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

We then create a cluster using:

```bash
     $ az acs create --orchestrator-type=Kubernetes \
        --resource-group=<your-group-name> --name=<your-cluster-name>
```

After the cluster is created, we can get credentials
for the cluster using this command:

```bash
     $ az acs Kubernetes get-credentials         --resource-group=<your-group-name> --name=<your-cluster-name>
```

Further instructions can be found in the [Azure documentation](https://docs.microsoft.com/en-us/azure/aks/Kubernetes-walkthrough).

3. Installing Kubernetes Locally

> Too use minikube, you need to have [hypervisor](https://www.vmware.com/topics/glossary/content/hypervisor#:~:text=A%20hypervisor%2C%20also%20known%20as,such%20as%20memory%20and%20processing.) installed on your machine.

The minikube tool can be found on GitHub in this [repository](https://github.com/Kubernetes/minikube) where links to binaries for Linux, macOS, and Windows can be found. Once the tool is installed, we can create a local cluster using this command:

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

`kubectl` is a command-line tool used to interact with the Kubernetes API and is used to manage Kubernetes
objects such as pods and also monitor the overall health of the cluster.

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
- `scheduler` - places different
pods onto different nodes in the cluster.
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

It is responsible for routing network traffic to load-balanced services in the Kubernetes Cluster and must be present on every node in the cluster to do its job effectively. Kubernetes has an API object named `DaemonSet` that is used in clusters to accomplish this. If your cluster runs the Kubernetes proxy with a `DaemonSet`, you can see the proxies by running this command:

```bash
$ kubectl get daemonSets --namespace=kube-system kube-proxy
```

Read more on Kubernetes Proxy [here](https://Kubernetes.io/docs/concepts/cluster-administration/proxies/)

2. Kubernetes DNS

Kubernetes runs a DNS server that provides naming and discovery for services defined in the cluster. It can also run as a
replicated service on the cluster. To view the servers, use this command:

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