# Create a service in Kubernetes

In Kubernetes, a service is a resource that allows a set of pods to be exposed as a single access point, making them available to other pods or external users.
Pods in Kubernetes are dynamic; they can be created or destroyed, and their IP addresses can change, making it challenging to maintain a direct and reliable connection between applications.
A service solves this issue by providing a fixed endpoint, even when the pods behind it change.

The main components of a service are:

- **Selector**: The service selects pods using labels. When a pod has the specified labels, it is considered part of the service.
- **ClusterIP**: Each service receives a fixed internal IP (if defined as a ClusterIP), which is used to route traffic to the pods.
- **Endpoints**: These represent the actual pods that meet the service’s selection criteria.

## Procedure

This example demonstrates how two pods within the same namespace cannot communicate with each other by name.
Creating a service will later enable communication between the two test pods.

The first pod defined is the `backend` pod. Below is its YAML definition.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend-container
        image: mariadb
        env:
        - name: MYSQL_DATABASE
          value: mydb
        - name: MYSQL_USER
          value: myuser
        - name: MYSQL_PASSWORD
          value: mypass
        - name: MYSQL_RANDOM_ROOT_PASSWORD
          value: "1"
```

The second pod involved in this example is the frontend pod. Below is its YAML definition.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 1
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: frontend-container
        image: wordpress
        env:
        - name: WORDPRESS_DB_HOST
          value: backend
        - name: WORDPRESS_DB_USER
          value: myuser
        - name: WORDPRESS_DB_PASSWORD
          value: mypass
        - name: WORDPRESS_DB_NAME
          value: mydb
```


Upon creating these two pods, we use the following command to forward the ports, allowing us to test connectivity.

```console
$ kubectl port-forward frontend 8080:80 -n ns-be
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
```

Using the `lynx` command, we can test the connectivity between the pods.

```console
$ lynx localhost:8080 -dump
Error establishing a database connection
```

As shown by the error message, the `frontend` pod is unable to connect to the `backend` pod. For this reason, it's necessary to create a `service` resource.

Service will rely on the pod label for the selector, so we need to use an existing one or creating a new one.

```console
$ kubectl label pod backend name=backend -n ns-be
pod/backend labeled
```

Then it is time to create the `service`, and this can be done by creating a service object named `backend` that expose the tcp 3306 port targeting the 3306 port and applying its selector to the label named `name` with value `backend`.

```console
$ kubectl create service clusterip backend --tcp=3306:3306 -n ns-be
service/backend created

$ kubectl set selector service backend 'name=backend' -n ns-be
service/backend selector updated
```

With the solution in place we can see the expected output.

```console
$ lynx localhost:8080 -dump
WordPress
Select a default language [English (United States)__________]

Continue
```
