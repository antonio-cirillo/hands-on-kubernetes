# Certified Kubernetes Administrator (CKA) Practice Exam: Part 1

## Count the Number of Nodes That Are Ready to Run Normal Workloads

Determine how many nodes in the cluster are ready to run normal workloads (i.e., workloads that do not have any special tolerations).

Output this number to the file `/k8s/0001/count.txt`.

```console
$ kubectl get node -o custom-columns=NAME:.metadata.name,TAINS:.spec.taints | grep -w "<none>" | wc -l > /k8s/0001/count.txt
```

## Retrieve Error Messages from a Container Log

In the `backend` namespace, check the log for the proc container in the `data-handler` Pod.

Save the lines which contain the text ERROR to the file `/k8s/0002/errors.txt`.

```console
$ kubectl logs data-handler -n backend -c proc | grep -w ERROR > /k8s/0002/errors.txt
```

# Find the Pod with a Label of app=auth in the Web Namespace That Is Utilizing the Most CPU

Before doing this step, please wait a minute or two to give our backend script time to generate CPU load.
Determine which Pod in the `web` namespace with the label `app=auth` is using the most CPU.
Save the name of this Pod to the file `/k8s/0003/cpu-pod.txt`.

```console
$ kubectl top pod -n web --sort-by cpu -l app=auth --no-headers | awk 'NR==1 {print $1; exit}' > /k8s/0003/cpu-pod.txt
```