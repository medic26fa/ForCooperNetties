---
title: "Module 1 - Create a Kubernetes Cluster"
weight: 1
--- 

## Objectives
- Learn what a Kubernetes cluster is.
- Use minikube to create Kubernetes cluster on your computer.
- Use basic kubectl commands to inspect your cluster.


A Kubernetes cluster is a set of machines that run your containerized applications. In this tutorial, you’ll create a local cluster for development using _minikube_, Kubernetes' lightweight solution for creating a VM on your local machine and for deploying a simple cluster containing only one node. In addition, you'll use kubectl - the command-line tool for interacting with your cluster.

### Before You Begin

If you haven't already, first [install minikube]({{< ref "/docs/start" >}}).  

{{< note >}} You only need _Step 1: installation_, the rest is covered here. To check if minikube is properly installed, run the *minikube version* command:

```shell
minikube version
```

## Step 1 - Start the Cluster


Once minikube is installed, create a cluster by running the *minikube start* command:

```shell
minikube start
```

The _minikube start_ command:

1. **Chooses and launches a driver** — minikube picks a backend to host your cluster, such as Docker or hyperkit.

  {{< note >}} If minikube couldn't auto-detect a supported driver when running the command, ensure an appropriate driver is installed and try again.   You can also specify a driver in the command (for example: `minikube start --driver=docker`).

2. **Downloads Kubernetes components** — minikube downloads either the version of Kubernetes you requested or the latest stable version by default.

3. **Creates a single-node cluster** — This node acts as both the [control plane and the worker](https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/).

4. **Configures kubectl** — So you can run commands and manage your cluster from the terminal.

You should now have a running Kubernetes cluster in your terminal! minikube started a virtual environment for you, and a Kubernetes cluster is now running in that environment. 

### Optional: Try the minikube Dashboard!

Prefer a GUI? You can launch a visual interface for your cluster using:

```bash
minikube dashboard
```

## Step 2 - Inspect Your Cluster

Let's use kubectl to find out some details about the cluster you just created! Run the *kubectl version* command:

```shell
kubectl version
```
If kubectl is configured, you should see both the version of the client and the server. The client version is the _kubectl version_; the server version is the _Kubernetes version_ installed on the control plane. You should also see details about the build.


Next, let's view the cluster details. Run the *kubectl cluster-info* command:

```shell
kubectl cluster-info
```

The `kubectl cluster-info` command gives you a quick summary of your Kubernetes control plane components. It will tell you if your cluster is up and  where it's running. 


Finally, to view the nodes in the cluster, run the *kubectl get nodes* command:

```shell
kubectl get nodes
```

The `kubectl get nodes` command shows you all the nodes in your Kubernetes cluster. Recall that a node is a machine (virtual or physical) that is part of your Kubernetes cluster. In a minikube cluster, you'll usually see just one node. This command is helpful for checking if nodes are ready to run workloads and when debugging issues.



🎉 **You Did It!**

You just launched a real Kubernetes cluster on your machine, connected it to your command line, and explored it with kubectl.

## What's Next

Congratulations! In this tutorial, you:

- Created a single-node Kubernetes cluster on your local machine. <br/>
- Installed the Kubernetes control plane and worker (in this case as the single node). <br/>
- Configured kubectl to communicate with the newly created cluster. <br/>
- Set up a fully functioning local environment where you can deploy apps, experiment, and manage everything with Kubernetes commands.

In the next tutorial, you'll be able to deploy your first pod - the basic unit of work in Kubernetes.

{{% button link="/docs/tutorials/kubernetes_101/module2" %}}Next: Deploy Your First Pod{{% /button %}}
