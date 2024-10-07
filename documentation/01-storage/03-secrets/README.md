# Secrets

This section contains the typical exercises related to Secrets.

## Create secret with imperitive mode

The following command allows you to create a generic secret via the command line.
The command specifies a list of key-value pairs, with each pair passed using the `from-literal` field.

```console
$ kubectl create secret generic <secret name> --from-literal=<key>=<value> [--from-literal=<key>=<value>]
```

## Mount secret to the pod location

As shown in the `02-mount-secret-pod.yaml` file, it is possible to mount a Secret object as a volume in a pod. 
The result of this operation is that, within the path where the volume is mounted, a file is created for each key contained in the Secret, and the contents of that file will be the value of the Secret.

## Mount secret to the pod as environment variable

As shown in the`"03-secret-as-env-variable.yaml` file, it is possible to define environment variables for a pod that reference key-value pairs from a Secret.

To verify the correct mapping of the environment variables, you can run the `printenv` command to list all environment variables within the pod.
