# Certified Kubernetes Administrator (CKA) Practice Exam: Part 6

## Drain Worker Node 1

Drain the `acgk8s-worker1` node.

```console
$ kubectl drain acgk8s-worker1 --ignore-daemonsets --delete-emptydir-data --force
```

## Create a Pod That Will Only Be Scheduled on Nodes with a Specific Label

Add the `disk=fast` label to the `acgk8s-worker2` Node.

```console
$ kubectl label nodes acgk8s-worker2 disk=fast
```

Create a pod called `fast-nginx` in the `dev` namespace that will only run on nodes with this label. 
Use the `nginx` image for this pod.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: fast-nginx
  namespace: dev
  labels:
    app: fast-nginx
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    disk: fast
```

```console 
$ kubectl create -f pod.yml
```