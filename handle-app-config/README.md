# Mount ConfigMap and Secret to a pod

The content of this folder contains the solution to one of the labs from the CloudGuru course **Certified Kubernetes Administrator (CKA)**.

Inside the folder, there is a YAML file that creates a pod with the `nginx` image. 
This pod mounts both a ConfigMap and a Secret as volumes. The ConfigMap will contain the Nginx server configuration, while the Secret will store the registered users for the server.

## Solution

To generate a user, in this case, `user` with the password `test`, use the following command:

```console
$ htpasswd -c .htpasswd user
```

This command will generate the `.htpasswd` file in the current directory.
Before deploying the pod, we need to create the Secret from this file using the following command:

```console
$ kubectl create secret generic nginx-htpasswd --from-file=.htpasswd
```

Once the Secret is created, remove the file for security purposes:

```console
rm .htpasswd
```

Now, deploy the pod:

```console
$ kubectl create -f deployment.yaml
```

To test that everything is working correctly, retrieve the IP address of the pod running the Nginx server, and perform a curl request from another pod.

```console
kubectl get pod -o wide
NAME                     READY   STATUS    RESTARTS   AGE    IP               NODE          NOMINATED NODE   READINESS GATES
busybox                  1/1     Running   0          162m   192.168.194.65   k8s-worker1   <none>           <none>
nginx-5965889559-7ssrp   1/1     Running   0          7s     192.168.194.68   k8s-worker1   <none>           <none>
```
Using a `busybox` pod, execute a curl to the Nginx server.

```console
$ curl 192.168.194.68
<html>
<head><title>401 Authorization Required</title></head>
<body>
<center><h1>401 Authorization Required</h1></center>
<hr><center>nginx/1.19.1</center>
</body>
</html>
```

As shown by the result, the server is reachable, but authentication fails because the username and password were not provided. 
Now, let's perform the test again, this time using the credentials generated earlier.

```console
$ url -u user:test 192.168.194.68
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<html>
```

As we can see, the test was successful.