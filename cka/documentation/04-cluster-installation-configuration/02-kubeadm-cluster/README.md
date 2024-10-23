# Use Kubeadm to install a basic cluster

## Join a node to the cluster

This question requires adding a new node to the cluster.
The keyword for the documentation is: "**Kubeadm token**".

To do this, simply generate a token from the control-plane node. 
The command to achieve this is:

```console
$ kubeadm token create [token] --print-join-command
```

The output of this command also provides the command to run on the node that wishes to join the cluster.
You only need to execute that command on the new node, and the process will be complete.

If the node has not successfully joined the cluster yet, check whether the Kubelet service is running correctly on the node. If there are issues, restart the service.
