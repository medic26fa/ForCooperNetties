---
title: "Module 2 - Deploy Your First Pod"
weight: 1
--- 

## Objectives
- Learn what a Kubernetes pod is.
- Create a pod manifest using YAML.
- Use the pod manifest to deploy the pod to your cluster.
- Use basic kubectl commands to explore your pod.


A pod is the smallest deployable unit in Kubernetes. Think of a pod as a wrapper around your container(s) that lets Kubernetes manage them; Kubernetes manages pods rather than managing the containers directly. 

In this tutorial, you’ll manually deploy a pod using a simple YAML script. This pod runs a container that prints a message ("Hello Kubernetes!") and stays alive for debugging. While this isn’t a full _Deployment_, this tutorial will help you understand pods as a deployable unit, test your Kubernetes setup, and practice using basic kubectl commands.

### Before You Begin

You should already have minikube and kubectl installed. If not, [install minikube]({{< ref "/docs/start" >}}).  To check if minikube and kubectl are properly installed, run the *version* commands:

```shell
kubectl version --client
minikube version
```

## Step 1 - Create the Pod Manifest

A _pod manifest_ is simply a YAML file that defines a Kubernetes pod. For your "Hello Kubernetes" pod manifest, create a file called _hello-pod.yaml_ (or whatever name you prefer), and paste in this YAML script:

```shell
apiVersion: v1
kind: pod
metadata:
  name: hello
spec:
  containers:
  - name: hello
    image: busybox
    command: ["sh", "-c", "echo Hello, Kubernetes! && sleep 3600"]
```

In this script:
- `busybox` is a Linux tool [image](https://kubernetes.io/docs/concepts/containers/images/)
- `echo` prints the message
-  `sleep` keeps the pod running for a specific amount of time (3600s)

Save the file.

## Step 2 - Start minikube

Before you can deploy the pod, you need to start minikube. As we saw in _Module 1: Create a Cluster_, minikube:

1. Boots up the local Kubernetes cluster you created
2. Starts the Kubernetes control plane and worker node (as the single node in this case)
3. Configures kubectl to connect to that local cluster

Start minikube:

```shell
minikube start
```

You should see a message similar to "🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default", but let's also check if it's working:

```shell
kubectl cluster-info
kubectl get nodes
```

If it's working, you should see a _Ready_ status.

## Step 3 - Apply the YAML File

Now that minikube is up and working, run the file:

```shell
kubectl apply -f hello-pod.yaml
```

If it worked, you'll see:

```shell
pod/hello created
```

Congratulations! Your first Kubernetes pod is live! 🎉

## Step 4 - Explore Your Pod

Now that your pod is up and running, let's use it to practice some basic kubectl commands.

### Check the Pod's Status

First, let's check the pod's status:

```shell
kubectl get pods
```

If your pod is up and running, you'll see status "Running" in the return.

### View the Pod's Log

Next, let's try viewing the pod's log to see what it did:

```shell
kubectl logs hello
```

As you can see in your YAML script, we asked echo to print a message, so the log should return: "Hello Kubernetes!".

### Get Pod Details

Now that you've viewed the log, let's get some pod details using the `kubectl describe` command:

```shell
kubectl describe pod hello
```

This command prints a detailed description of the selected resources, including related resources such as events or controllers.

### Delete and Recreate Your Pod

Finally, try deleting your pod:

```shell
kubectl delete pod hello
```

Then recreating it:

```shell
kubectl apply -f hello-pod.yaml
```

Nice work! When you run the `kubectl apply -f hello-pod.yaml` command, Kubernetes schedules the pod to a node and runs it. Remember that Kubernetes doesn’t run containers directly — it runs _pods_. This makes understanding pods as the basic unit of execution essential. They are the core building block of everything you will do in Kubernetes. Further, troubleshooting always happens at the _pod level_. If your app won’t start, for example, you might begin troubleshooting using `kubectl get pods`, `kubectl describe pod`, and `kubectl logs`.

## What's Next

In this tutorial, you:

✅ Wrote and applied a pod manifest using YAML <br/>
✅ Used kubectl to deploy a pod to your cluster <br/>
✅ Explored your pod’s status, logs, and details <br/>
✅ Practiced common pod operations like delete and recreate <br/>

Manually launching a unit of execution (a pod) is useful for learning, testing, and short-lived workloads, but in the real world you'll likely be launching a full-blown app. For that you'll need a _Deployment_. 

In the next tutorial, you'll level up by creating a Deployment, which automatically creates and manages pods for you — enabling scaling, self-healing, and easy updating.

{{% button link="/docs/tutorials/kubernetes_101/module3" %}}Next: Deploy an App with a Deployment{{% /button %}}
