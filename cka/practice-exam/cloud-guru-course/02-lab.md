# Certified Kubernetes Administrator (CKA) Practice Exam: Part 2

## Edit the Web Frontend Deployment to Expose the HTTP Port

In the `web` namespace, there is a deployment called `web-frontend`.

Edit this deployment so that the containers within its Pods expose port 80.

```console
$ kubectl get deployment web-frontend -n web -o yaml > deployment.yml
```

```yaml
containers:
      - image: nginx:1.14.2
        imagePullPolicy: IfNotPresent
        name: nginx
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        ports:
        - containerPort: 80
```

```console
$ kubectl apply -f deployment.yml
```

## Create a Service to Expose the Web Frontend Deployment's Pods Externally

Create a service called `web-frontend-svc` in the `web` namespace. 
This service should make the Pods from the `web-frontend` deployment in the `web` namespace reachable from outside the cluster.

External entities should be able to reach the service by contacting any node in the cluster on port 30080.

```console
$ kubectl create svc nodeport web-frontend-svc --tcp=80:80 --node-port=30080 -o yaml --dry-run=client > svc.yml
```

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: web-frontend-svc
  name: web-frontend-svc
spec:
  ports:
  - name: 80-80
    nodePort: 30080
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: web-frontend
  type: NodePort
status:
  loadBalancer: {}
```

```console
$ kubectl create -f svc.yml -n web
```

## Scale Up the Web Frontend Deployment

Scale the `web-frontend` deployment in the `web` namespace up to `5` replicas.

```console
$ kubectl scale deployment web-frontend -n web --replicas=5
```

## Create an Ingress That Maps to the New Service

Create an Ingress called `web-frontend-ingress` in the `web` namespace that maps to the `web-frontend-svc` service in the `web` namespace. 
The Ingress should map all requests on the `/` path.

```console
$ kubectl create ingress web-frontend-ingress -n web --class=default --rule="/*=web-frontend-svc:80" --dry-run=client -o yaml > ingress.yml
```

```console
$ kubectl create -f ingress.yml
```