# Certified Kubernetes Administrator (CKA) Practice Exam: Part 10

## Determine What Is Wrong with the Broken Node

Something is wrong with one of the nodes in the cluster.
Explore the cluster and determine which Node is broken, then save the name of that node to the file `/k8s/0004/broken-node.txt`.

```console
$ kubectl get nodes  --no-headers | grep -w NotReady | awk '{print $1}' > /k8s/0004/broken-node.txt
```

## Fix the Problem

Explore the broken Node and determine what is wrong with it.

Fix the problem with the broken node so that it becomes `READY` again.

```console
$ ssh acgk8s-worker2
```

```console
$ sudo systemctl enable kubelet
```

```console
$ sudo systemctl start kubelet
```