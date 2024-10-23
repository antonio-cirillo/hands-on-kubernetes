# Certified Kubernetes Administrator (CKA) Practice Exam: Part 9

## Create a Multi-Container Pod

Create a pod called `multi` in the `baz` namespace. 
Run two containers in this pod, with the following images (in order):
- `nginx`
- `redis`

# Create a Pod Which Uses a Sidecar to Expose the Main Container's Log File to `stdout`

Create a pod called `logging-sidecar` in the `baz` namespace.

Run a container in this pod with the `busybox` image and the following command: 

```sh
sh -c while true; do echo Logging data > /output/output.log; sleep 5; done
```

Add a second container called sidecar to the pod which also uses the busybox image, and reads the output from the first container, as well as prints it to the container log (console).

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: logging-sidecar
  namespace: baz
spec:
  containers:
  - name: container
    image: busybox
    command: ["sh", "-c", "while true; do echo Logging data > /output/output.log; sleep 5; done"]
    volumeMounts:
      - name: shared-vol
        mountPath: /output/
  - name: sidecar
    image: busybox
    command: ["sh", "-c", "tail -f /output-2/output.log"]
    volumeMounts:
    - name: shared-vol
      mountPath: /output-2/
  volumes:
    - name: shared-vol
      emptyDir: {}
```