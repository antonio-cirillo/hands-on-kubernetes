# Mock Exam - 1

## Exercise 1 

Deploy a pod named nginx-pod using the nginx:alpine image.

```console
$ k run nginx-pod --image=nginx:alpine
```

## Exercise 2

Deploy a messaging pod using the redis:alpine image with the labels set to tier=msg.

```console
$ k run messaging --image redis:alpine -l=tier=msg
```

## Exercise 3

Create a namespace named apx-x9984574.

```console
$ k create ns apx-x9984574
```

## Exercise 4

Get the list of nodes in JSON format and store it in a file at /opt/outputs/nodes-z3444kd9.json.

```console
k get node -o json > /opt/outputs/nodes-z3444kd9.json
```

## Exercise 5

Create a service messaging-service to expose the messaging application within the cluster on port 6379.

```yaml
spec:
  containers:
  - image: redis:alpine
    name: messaging
    ports:
      - containerPort: 6379
```

```console
$ k expose pod messaging --port=6379 --name=messaging-service
```

## Exercise 6

Create a deployment named hr-web-app using the image kodekloud/webapp-color with 2 replicas.

```console
$ k create deploy hr-web-app  --image kodekloud/webapp-color --replicas=2
```

## Exercise 7

Create a static pod named static-busybox on the controlplane node that uses the busybox image and the command sleep 1000.

```console
k run static-busybox --image=busybox --dry-run='client' -o yaml --command -- /bin/sh -c "sleep 1000" > /etc/kubernetes/manifests/static-busybox.yaml
```

## Exercise 8

Create a POD in the finance namespace named temp-bus with the image redis:alpine.

```console
$ k run temp-bus -n finance --image redis:alpine
```

## Exercise 9

A new application orange is deployed. There is something wrong with it. Identify and fix the issue.

```yaml
initContainers:
  - command:
    - sh
    - -c
    - sleeeep 2;
```

```yaml
initContainers:
  - command:
    - sh
    - -c
    - sleep 2;
```

## Exercise 10

Expose the hr-web-app as service hr-web-app-service application on port 30082 on the nodes on the cluster.

The web application listens on port 8080.

```console
k expose deploy hr-web-app --name hr-web-app-service --port 8080 --type=NodePort -o yaml --dry-run=client > svc.yaml
```

```yaml
spec:
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 8080
    nodePort: 30082
```

## Exercise 11

Use JSON PATH query to retrieve the osImages of all the nodes and store it in a file /opt/outputs/nodes_os_x43kj56.txt.

The osImages are under the nodeInfo section under status of each node.

```console
k get nodes -o=jsonpath='{.items[*].status.nodeInfo.osImage}' > /opt/outputs/nodes_os_x43kj56.txt
```

## Exercise 12

Create a Persistent Volume with the given specification: -

Volume name: pv-analytics

Storage: 100Mi

Access mode: ReadWriteMany

Host path: /pv/data-analytics

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-analytics
spec:
  accessModes:
    - ReadWriteMany
  hostPath:
    path: "/pv/data-analytics"
  capacity:
    storage: 100Mi
```