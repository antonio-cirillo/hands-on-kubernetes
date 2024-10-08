# Application failure

Use the following commands to analyze the situation and gather as much information as possible:

```console
$ kubectl describe <object type> <name>
```

```console
$ kubectl logs <pod name>
```

```console
$ systemctl status <service name>
```

## PV, PVC and StorageClass

During pod debugging, using the `describe` command can help identify if the issue is related to volume binding.

In such a case, the problem is likely due to one of the following errors:

- The name of the claim used in the pod differs from the name of the PVC object.
- The PVC object is requesting a volume that does not match the requirements. This could be due to an incorrect `storageClassName`, a capacity larger than what is available, or an incompatible access mode.

## Services

During service debugging, it's important to use `curl` to determine where the issue lies.

Analyze the various services and pods to verify the following:

- The services correctly select the pods to expose.
- The services expose the correct port.
- The services redirect traffic to the correct port.
- Verify the names of services and pods.
- If environment variables are used, carefully check their values.