---
title: Helm Chart
---

# Deploying Permify with Helm Charts

## Introduction to Helm

Helm is a package manager for Kubernetes applications that simplifies the deployment and management of applications in a Kubernetes cluster. Using Helm, you can package and release your applications as charts, which are pre-configured Kubernetes resources.

You can learn more about helm [here](https://helm.sh/docs/)

## Helm Charts for Permify

Permify provides Helm Charts to facilitate the deployment and management of Permify in Kubernetes environments. Helm Charts encapsulate all the necessary Kubernetes resources and configurations required to run Permify, making it easy to deploy and maintain.([helm-permify-github](https://github.com/Permify/helm-charts))

## Helm installation

### Prerequisite

Installing the Helm Chart pretty easy but there is a pre-requisite of setting up Kubernetes Cluster.

If you do not have a Kubernetes cluster you can choose any of the four below options.

**1. [EKS-Amazon Elastic k8s service](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)**

**2. [GKE-Google k8s engine](https://cloud.google.com/kubernetes-engine)**

**3. [AKS-Azure kubernetes Service](https://azure.microsoft.com/en-in/products/kubernetes-service)**

**4. [microk8s](https://microk8s.io/#install-microk8s)**

### 1.1: Install Helm Chart Using Script

If you like doing everything from scratch then I would suggest you to install the Helm Chart Using script.

Run the following scripts -

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
```

```bash
chmod 700 get_helm.sh
```

```bash
./get_helm.sh
```

You can verify the installation by running the command

```bash
helm version
```

If helm is installed the terminal will provide this as output

```bash
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/vagrant/.kube/config
version.BuildInfo{Version:"v3.4.0", GitCommit:"7090a89efc8a18f3d8178bf47d2462450349a004", GitTreeState:"clean", GoVersion:"go1.14.10"}
```

### 1.2: Install Helm Chart with package Manager

If you like package manager then you use the following install command based on your preference -

**Homebrew**

```bash
brew install helm
```

**Chocolatey**

```bash
choco install kubernetes-helm
```

**Scoop**

```
scoop install helm
```

**Snap**

```bash
sudo snap install helm --classic
```

If helm is installed the terminal will provide this as output

```bash
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/vagrant/.kube/config
version.BuildInfo{Version:"v3.4.0", GitCommit:"7090a89efc8a18f3d8178bf47d2462450349a004", GitTreeState:"clean", GoVersion:"go1.14.10"}
```

## Adding the Permify Helm Charts Repository

To use Permify Helm Charts, you need to add the Permify Helm Charts repository to Helm. Follow these steps:

**1.** Open your terminal.

**2.** Run the following command to add the Permify Helm Charts repository:

```bash
$ helm repo add permify https://permify.github.io/helm-charts
```

**3.** After adding the Permify Helm Charts repository, you can search for available charts using the following command:

```bash
$ helm search repo permify
```

**Installing Permify using Helm Charts**

Once you've added the Permify Helm Charts repository, you can install Permify using Helm

```bash
helm install permify permify/permify
```