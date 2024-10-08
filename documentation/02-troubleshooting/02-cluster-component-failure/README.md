# Troubleshoot cluster component failure

Based on the distribution of the components, you can use the following commands to analyze the logs.

The following command is used to check the status of the cluster components when they are distributed as pods within the cluster.

```console
$ kubectl describe <object type> <name>
```

If the components are distributed as services, you can use the following commands to check their status.

```console
$ service kube-apiserver status
$ service kube-controller-manager status
$ service kube-scheduler status
$ service kubelet status
$ service kube-proxy status
```

Based on how the components have been distributed (either as pods or services), you can use the following commands to analyze their logs:

```console
$ kubectl logs <pod name>
```

```console
$ sudo journalctl -u <component> -f
```

## Control Plane

- Verify that all components are running correctly.
  In a kubeadm-managed cluster, check that all pods in the `kube-system` namespace are in the `Running` state.
- To check if the directories that the pods need to access are correctly mounted as volumes, you can use the `kubectl describe` command to inspect the pod and ensure that the volumes are properly defined and mounted.

## Worker Node


If a node is in the NodeReady state and if you encounter issues related to deploying or running a pod, check the status of the Kubelet service on the affected node.

``` console
$ systemctl status kubelet
```

If the service is stopped or not running, try starting it:

```
$ systemctl start kubelet
```

To obtain the logs related to the kubelet service, you can use the following command:

```console
$ journalctl -u kubelet
```

This command will display the logs for the kubelet service, allowing you to identify any errors or issues that may be affecting its operation. 
You can scroll through the logs to find specific error messages that can help you troubleshoot and resolve the problem.
