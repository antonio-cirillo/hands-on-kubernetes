# Create role binding

Before proceeding, remember that the **O** attribute in the certificate used by the user to authenticate to the cluster defines the groups they belong to within the cluster. 

In this guide, we will show how to assign the **view** cluster role to the user created earlier. The role will be assigned at the group level.

## Procedure

With the following command, we can check which context we are currently using. In this case, we are logged in with the user created in the previous guide.

```console
$ kubectl config get-contexts
CURRENT   NAME                        CLUSTER    AUTHINFO           NAMESPACE
          kubernetes-admin@hands-on   hands-on   kubernetes-admin   
*         myuser@hands-on             hands-on   myuser             namespace
```

Let's try executing the command to list all the pods in the cluster. 

```console
$ kubectl get po --all-namespaces
Error from server (Forbidden): pods is forbidden: User "myuser" cannot list resource "pods" in API group "" at the cluster scope
```

Since the user was just created, they have no roles assigned. Therefore, the command returns a `forbidden` error.

Let's switch back to the admin context. At this point, proceed by assigning the `view` ClusterRole to the `myns` group, which is the group to which the user `myuser` belongs.

```console
$ kubectl config get-contexts
CURRENT   NAME            CLUSTER         AUTHINFO        NAMESPACE
*         kind-hands-on   kind-hands-on   kind-hands-on   

$ kubectl create clusterrolebinding view-to-group-myns --clusterrole=view --group=myns
clusterrolebinding.rbac.authorization.k8s.io/view-to-group-myns created
```

By switching the context again, let's try executing the command to list all the pods.

```console
$ kubectl config get-contexts
CURRENT   NAME                        CLUSTER    AUTHINFO           NAMESPACE
          kubernetes-admin@hands-on   hands-on   kubernetes-admin   
*         myuser@hands-on             hands-on   myuser             namespace

$ kubectl get pod --all-namespaces
NAMESPACE            NAME                                             READY   STATUS    RESTARTS   AGE
kube-system          coredns-6f6b679f8f-dcwz8                         1/1     Running   0          3h21m
kube-system          coredns-6f6b679f8f-v6n6l                         1/1     Running   0          3h21m
kube-system          etcd-hands-on-control-plane                      1/1     Running   0          3h21m
kube-system          kindnet-fjthb                                    1/1     Running   0          3h21m
kube-system          kube-apiserver-hands-on-control-plane            1/1     Running   0          3h21m
kube-system          kube-controller-manager-hands-on-control-plane   1/1     Running   0          3h21m
kube-system          kube-proxy-64dvs                                 1/1     Running   0          3h21m
kube-system          kube-scheduler-hands-on-control-plane            1/1     Running   0          3h21m
local-path-storage   local-path-provisioner-57c5987fd4-b6gdz          1/1     Running   0          3h21m
```

As we can see from the result, the user now has the necessary permissions to complete the listing operation.
