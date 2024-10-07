# Scheduling

This section contains the typical exercises related to scheduling.

# Schedule a pod with node name

It is possible to schedule a pod on a specific node.

To do this, simply add the `nodeName` field with the name of the node where you want to schedule the pod.

```yaml
nodeName: <node name>
```

## Schedule a pod with node selector

It is possible to schedule a pod on a subset of specific nodes based on the labels of the nodes.

Using the `nodeSelector` field, you can specify a list of key-value pairs that must be assigned to a node in order for it to be selected for scheduling the pod.

```yaml
nodeSelector:
    key: value
```

## Mark node as unschedulable

To mark a node as unschedulable, we have two possible commands:

- If you want to mark the node as unschedulable without affecting the pods already running on the node, use the following command:
    ```console
    $ kubectl cordon <node name>
    ```
- If you want to mark the node as unschedulable and migrate all running pods on that node to other available nodes in the cluster, use the following command:
    ```console
    $ kubectl drain <node name>
    ```

To remove the unschedulable status from the node in either case, use the following command:

```console
$ kubectl uncordon <node name>
```
