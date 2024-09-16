# Certified Kubernetes Administrator (CKA) Practice Exam: Part 3

## Create a Service Account

Create a service account in the `web` namespace called `webautomation`.

```console
$ kubectl create sa webautomation -n web
```

## Create a ClusterRole That Provides Read Access to Pods

Create a ClusterRole called `pod-reader` that has `get`, `watch`, and `list` access to all Pods.

```console
$ kubectl create clusterrole pod-reader --verb=get,watch,list --resource=pod
```

## Bind the ClusterRole to the Service Account to Only Read Pods in the web Namespace

Bind the ClusterRole to the `webautomation` service account so that it can read all Pods, but only in the `web` namespace.

```console
$ kubectl create clusterrolebinding webautomation-binding -n web --clusterrole=pod-reader --serviceaccount=web:webautomation
```