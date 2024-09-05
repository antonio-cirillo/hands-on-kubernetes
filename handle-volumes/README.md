# Create volumes in Kubernetes

In Kubernetes, a **PV**, or **Persistent Volume**, is a resource that represents physical or virtual storage at the cluster level.
In other words, it is a piece of storage space that is independent of the lifecycle of the pods and containers using it.
This allows data to persist even when pods are created or destroyed.

When a **PV** is created, it is associated with a physical storage resource, such as a disk on a cloud provider or a partition on a local server.

**PV** can be configured to meet various storage requirements, such as size, access mode (read/write shared, read/write exclusive, etc.), and type of storage (SSD, HDD, file system, block, etc.).

Pods use **PVs** through **Persistent Volume Claims (PVCs)**.
A **PVC** is a request for storage from a user.
When a **PVC** is created, Kubernetes looks for a PV that meets the PVC's requirements and binds it to the PVC.

## Procedure

The YAML file used to create the test PV is as follows.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv001
spec:
  storageClassName: localpv
  accessModes:
  - ReadWriteOnce
  capacity:
    storage: 5Gi
  hostPath:
    path: /data/
```

This definition configures a PV that uses a local directory on the host node as storage, with a capacity of 5 GB and access mode allowing read and write operations from only one node at a time.

The YAML file used to create the test PVC is as follows.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
  namespace: volumes-test
spec:
  storageClassName: localpv
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

When this Kubernetes object is created, it will be assigned the previously created PV.
After the binding, the PVC will not be used until it is mounted by a pod.
The volume will be "reserved" for the PVC, but no read/write operations will occur until a pod mounts that volume.

When the pod is created, the path related to the PV is generated and assigned to the PVC being used by the pod.

```console
$ root@kind-cluster-control-plane df /data/
Filesystem     1K-blocks    Used Available Use% Mounted on
overlay         61202244 6330432  51730488  11% /
```

The remaining space on a PV cannot be shared with other PVCs. Even if the PVC only requires a portion of the PV, the entire volume is reserved for that PVC.

From the control plane node, we create an HTML page in the `/data/` path as follows.

```console
$ root@kind-cluster-control-plane echo 'THIS COMES FROM MY VOLUME' > /data/index.html
```

At this point, we perform port forwarding to test the result using a curl command.

```console
$ kubectl port-forward nginx-759f7f894c-2crwm -n volumes-test 8080:80
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
```

```console
$ curl localhost:8080
THIS COMES FROM MY VOLUME
```
