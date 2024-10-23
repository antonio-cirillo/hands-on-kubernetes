# Replication Controller, ReplicaSet & Deployment

This section presents the most useful commands for answering certain questions during the exam.

## Rolling update

The following command allows you to perform a rolling update on a Deployment, modifying the image of a running container. 
This command will update the image using a rolling update strategy.
Remember that the pod must be configured to use the RollingUpdate mechanism.
You can verify this setting using the `describe` command.

```console
$ kubectl set image deployment/<deployment name> <container <name>=<image name> --record
```

## Scale replica

The following command allows you to modify the number of instances managed by a `Deployment`, `ReplicaSet`, or `ReplicationController`.
It is important to note that the command below is intended for use with a `Deployment` object.

```console
$ kubectl scale deployment <deployment name> --replicas=<number of replicas>
```
