# Certified Kubernetes Administrator (CKA) Practice Exam: Part 8

## Create a NetworkPolicy That Denies All Access to the Maintenance Pod

There is a pod called `maintenance` in the `foo` namespace.
Create a `NetworkPolicy` that blocks all traffic to and from this pod.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-policy
  namespace: foo
spec:
  podSelector:
    matchLabels:
      app: maintenance
  policyTypes:
    - Ingress
    - Egress
```

## Create a NetworkPolicy That Allows All Pods in the Users-Backend Namespace

Create a NetworkPolicy That Allows All Pods in the Users-Backend Namespace to Communicate with Each Other Only on a Specific Port

There are some pods in the `users-backend` namespace.
Create a `NetworkPolicy` that blocks all traffic to pods in this namespace, except for traffic from pods in the same namespace on port 80.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-policy
  namespace: users-backend
spec:
  podSelector: {}
  policyTypes:
    - Ingress
  ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            kubernetes.io/metadata.name: users-backend
      ports:
        - protocol: TCP
          port: 80
```
