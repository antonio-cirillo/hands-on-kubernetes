# Cluster upgrade to a specific version

This question asks to upgrade the Kubernetes cluster from one version to another.
It is important to remember that the cluster can only be upgraded one minor version at a time.

The complete guide on how to upgrade a Kubernetes cluster using Kubeadm can be found in the official documentation. 
In the documentation, search for the keyword "**upgrade**".

Important: if, during the search for the Kubernetes version to upgrade to, the desired version is not found, it may be necessary to update the apt repository. 
The command to update the apt repository can be found in the official documentation, where the installation of kubeadm is explained. T
he keyword to search for is "**Installing kubeadm**".

Alternatively, the documentation can be found at the following path:

```console
Getting started > Production environment > Installing Kubernetes with deployment tools > Bootstrapping clusters with kubeadm > Installing kubeadm
```
