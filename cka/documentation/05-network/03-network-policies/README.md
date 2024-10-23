# Network Policies

This section contains policy definitions that are useful for passing the exam.
Below is a brief description of the definitions found in the directory.

## Deny all traffic to an application

The policy applies to all pods labeled as `app: web`.
All incoming traffic is blocked.
This is because the policy type is solely **Ingress**, and no fields are defined within the `ingress` structure.
This means that no filtering rules are present, and thus, by default, all incoming traffic is blocked.

## Deny all traffic to all pods

The policy is applied to all pods because the `podSelector` is initialized with the structure `{}`.
This means that the rule applies to all pods.

## Allow all traffic to an application

The policy is applied to all pods labeled as `app: web`. 
The policy type is **Ingress**, which refers to incoming traffic.
Since the `ingress` field is set with the object `{}`, this specifies that all incoming traffic is allowed.

## Limit traffic to an application

The policy is defined within the `my-namespace` namespace. 
It is an **Ingress** policy that applies to all pods.
Traffic is allowed for all pods since the `podSelector` field is initialized with an empty object.
Since the `namespaceSelector` field is not specified, the pod selector only refers to pods within the namespace where the policy is defined.
Therefore, this allows pods within the namespace to accept traffic only from other pods in the same namespace.

## Allow all traffic to an application from all namespaces

The rule applies to all pods labeled `app: web`.
Keep in mind that this applies only to the pods deployed within the same namespace as the policy.
Traffic is allowed from all pods in all namespaces because the `namespaceSelector` field is initialized with an empty object, which indicates **all**.

## Allow all traffic from namespace

Traffic to the pods labeled `app: web` is allowed only from namespaces labeled `purpose: production`.

## Limit traffic to a port

This policy restricts incoming traffic to pods labeled `app: apiserver` from all pods within the same namespace to only port `5000`.
All other incoming traffic is blocked.

## Deny external egress traffic

This policy applies to pods with the label `app: foo`.
It allows all outbound traffic on ports 53/udp and 53/tcp to the `kube-dns` pods for DNS resolution.
The policy specifies a `namespaceSelector` that matches `kubernetes.io/metadata.name: kube-system` and a `podSelector` that matches `k8s-app: kube-dns`.
This will select only the `kube-dns` pods in the `kube-system` namespace, allowing outbound traffic to those pods.
Since they are not listed, traffic to IP addresses outside the cluster is denied.
