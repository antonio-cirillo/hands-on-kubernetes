# Certified Kubernetes Administrator (CKA) Practice Exam: Part 7

## Create a PersistentVolume

Create a PersistentVolume called `host-storage-pv` with a storage capacity of `1Gi` in the `acgk8s` context. 
Configure this PersistentVolume so that volumes that use it can be expanded in the future.

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: my-storage-class
provisioner: kubernetes.io/no-provisioner
allowVolumeExpansion: true
```

Configure the PersistentVolume to use a hostPath for storage located at `/etc/data`.
Configure the PersistentVolume so that it can be automatically reused if all claims are deleted.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: host-storage-pv
spec:
  storageClassName: my-storage-class
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /etc/data
  persistentVolumeReclaimPolicy: Recycle
```

## Create a Pod That Uses the PersistentVolume for Storage

Create a Pod called `pv-pod` in the `auth` Namespace.
Configure this Pod so that it uses the `host-storage-pv` PersistentVolume for storage.

Name the PersistentVolumeClaim `host-storage-pvc`.
Give your PersistentVolumeClaim a size of `100Mi`.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: host-storage-pvc
  namespace: auth
spec:
  resources:
    requests:
      storage: 100Mi
  storageClassName: my-storage-class
  accessModes:
    - ReadWriteOnce
```

Use the `busybox` image for this Pod, and configure it to run the command

```sh
sh -c while true; do echo success > /output/output.log; sleep 5; done
```

Mount the volume that uses the PersistentVolume for storage, such that the `/output` directory ultimately writes to the PersistentVolume.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pv-pod
  namespace: auth
spec:
  containers:
    - name: busybox-container
      image: busybox
      command: ["sh", "-c", "while true; do echo success > /output/output.log; sleep 5: done"]
      volumeMounts:
        - name: my-volume
          mountPath: /output
  volumes:
     - name: my-volume
       persistentVolumeClaim:
         claimName: host-storage-pvc
```

## Expand the PersistentVolumeClaim
Expand the PersistentVolumeClaim so that the requested storage size is `200Mi.`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: host-storage-pvc
  namespace: auth
spec:
  resources:
    requests:
      storage: 200Mi
  storageClassName: my-storage-class
  accessModes:
    - ReadWriteOnce
```

```console
kubectl apply -f pvc.yml
```
