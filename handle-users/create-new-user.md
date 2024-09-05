# Creating a new Kubernetes user

Users created via certificates, as in this case, exist globally in the cluster and are not tied to a specific namespace. This is because Kubernetes does not have an internal entity that manages or registers users but relies on external authentication mechanisms (such as the client certificate). Therefore, the user exists for the entire cluster. They can interact with any namespace as long as they have the correct permissions.


## Procedimento 

Generate a 2048-bit RSA private key.

```bash
openssl genrsa -out ./myuser.key 2048
```

Create a **CSR (Certificate Signing Request)**, which is a certificate signing request.

```bash
openssl req -new -key myuser.key -out myuser.csr -subj "/CN=myuser/O=myns"
```

Create a CSR (Certificate Signing Request), which is a certificate signing request.

The parameter `-subj "/CN=myuser/O=myns"` is used to directly specify the subject's **distinguished name (DN)** for the certificate. Instead of manually entering information via an interactive interface, the DN is passed directly through the provided string. The string defines:

- **Common name (CN)**: nthe name of the person or service to whom the certificate belongs. In this case, it is the name of the user we are creating.
- **Organization (O)**: the name of the organization or namespace to which the subject belongs. Other possible fields in a **DN include country (C)**, **state (ST)**, **locality (L)**, **organizational unit (OU)**.

Even though a namespace name has been specified in the **O** field, this does not have a direct impact on the namespace context in which the user can operate. Kubernetes does not automatically associate the specified namespace in the **O** field with a namespace in the cluster.

The O field may have an impact only when groups are used in the context of Kubernetes RBAC. For example, if a **ClusterRoleBinding** or a **RoleBinding** is configured to assign roles to a group, Kubernetes can associate the user with the group specified in the **O** field of the certificate. This means it is possible to define rules that give permissions to groups, and Kubernetes will check the **O** field in the certificate to see if the user belongs to that group.

The next step is to sign the certificate request with the cluster's **certification authority (CA)**.

```bash
openssl x509 -req -in myuser.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out myuser.crt -days 365
```

The **CAcreateserial** parameter indicates that if no serial file for the CA already exists, one will be created. The serial file tracks the unique serial number assigned to certificates signed by that CA. The **days** parameter indicates the validity of the certificate in days.

The following command creates the context named `myuser@hands-on` defining the user `myuser` and the namespace `myns`.

```bash
kubectl config set-context myuser@hands-on --cluster=kind-hands-on --user=myuser --namespace=myns
```

The next command configures the user in the Kubernetes configuration file, associating them with the certificate and the private key. These credentials will be used to authenticate the user when used in a context.

```bash
kubectl config set-credentials myusers --client-certificate=./myuser.crt --client_key=./myuser.key 
```

The following command allows switching the current context to the newly created user's context. This operation will allow us to access the cluster with the new user.

```bash
kubectl config use-context myuser@hands-on
```