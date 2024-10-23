# Storage

This section presents the definitions of the `PersistentVolume`, `PersistentVolumeClaim`, and `StorageClass` objects.

## Persistent Volume

The keyword to search for in the documentation is "**pv**". 
There is a guide that explains how to create a volume and attach it to a pod. 
This guide includes YAML files that can be reused for exercises.

The definition of a PersistentVolume object must specify the volume size, the access mode, and either the hostPath (if the volume is defined on the node) or a configuration based on the intended service.

The file `pv-definition.yaml` is an example of a PersistentVolume object definition.

## Persistent Volume Claim

The definition of a PersistentVolumeClaim object is included in the documentation related to creating a PersistentVolume.

The file `pvc-definition.yaml` contains an example of a PVC definition.

## Storage Class

The keyword to use in the documentation is "**storage classes**".
Here, you can find several templates that can be used to define a StorageClass.

An important parameter is `allowVolumeExpansion`, which, when set to `true`, allows the volume size to be modified.

The file `storage-class-definition.yaml` contains an example of a StorageClass definition.
