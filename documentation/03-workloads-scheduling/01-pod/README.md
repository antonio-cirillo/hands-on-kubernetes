# Pod

This section contains the typical exercises related to Pods.

## Create a static pod

To create a static pod, you first need to locate the path on the node where you intend to deploy the pod, where you will place the YAML file defining the pod.
To do this, run the following command on the relevant node.

```console
$ ps -aux | grep kubelet
```

Using this command, you can view the parameters with which the kubelet process is executed.
The parameter of interest here is `config`.
This parameter contains the path to the kubelet's configuration file.

Open the configuration file and locate the `staticPodPath` field.

At this point, all you need to do is create the YAML file defining the pod within this directory.
Once completed, the pod will be automatically deployed. 

Remember that to delete a static pod, you must manually remove its definition file from the folder.

## Run as

The file `04-run-as-yaml` contains a definition of a pod where parameters related to the user running the container, the group executing the container, and other details related to the `securityContext` field are modified.

In the configuration file, the `runAsUser` field specifies that for any Containers in the Pod, all processes run with user ID `1000`. 

The `runAsGroup` field specifies the primary group ID of `3000` for all processes within any containers of the Pod. 
If this field is omitted, the primary group ID of the containers will be `root(0)`. 

Any files created will also be owned by user `1000` and group `3000` when `runAsGroup` is specified.

Since `fsGroup` field is specified, all processes of the container are also part of the supplementary group ID `2000`. The owner for volume `/data/demo` and any files created in that volume will be Group ID `2000`.

Additionally, when the `supplementalGroups` field is specified, all processes of the container are also part of the specified groups.
If this field is omitted, it means empty.

## Capabilities

The `05-capabilities.yaml` file provides an example of how to modify the capabilities of the user running a container.
You can add or remove permissions for the user.

To make it easier to complete these exercises, it is recommended to search for the keyword `securityContext` in the documentation during the exam.

## Expose through port

With the following command, you can create a service of type `ClusterIP` or `NodePort` that exposes a port of a pod.
It is important to remember that the `port` parameter defines the service port. 
If the `targetPort` is not specified, it defaults to the port.
If you need to specify a particular `nodePort`, you will have to edit the output manually.

```console
$ kubectl expose pod <pod name> --name=<service name> --port=<service port> --type=<ClusterIP | NodePort>
```

## Display resource usage of pods.

The `top pod` command allows you to see the resource consumption of pods.

```console
$ kubectl top pods [-l label=value] [--sort-by=[cpu | memory]]
```
