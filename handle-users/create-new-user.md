# Creazione di un nuovo utente Kubernetes

Gli utenti creati tramite certificati, come in questo caso, esistono globalmente nel cluster, non sono legati a un namespace specifico.
Questo perché Kubernetes non ha un'entità interna che gestisce o registra utenti, ma si affida a meccanismi di autenticazione esterni (come il certificato client).
Quindi, l'utente esiste per tutto il cluster. Può interagire con qualsiasi namespace, a patto che abbia i permessi corretti.


## Procedimento 

Genera una chiave RSA privata di 2048 bits.

```bash
openssl genrsa -out ./myuser.key 2048
```

Creazione di una CSR (Certificate Signing Request), che è una richiesta di firma del certificato

```bash
openssl req -new -key myuser.key -out myuser.csr -subj "/CN=myuser/O=myns"
```

Il parametro `-subj "/CN=myuser/O=myns"` serve a specificare direttamente il **distinguished name (DN)** del soggetto del certificato. Invece di inserire manualmente le informazioni tramite un'interfaccia interattiva, il DN viene passato direttamente tramite la stringa fornita. Nella stringa viene definito:
- **Common name (CN)**: nome della persona o del servizio a cui appartiene il certificato. In questo caso è il nome dell'utente che stiamo creando.
- **Organization (O)**: nome dell'organizzazione o del namespace a cui appartiene il soggetto.
Altri campi possibile in un DN includono **country (C)**, **state (ST)**, **locality (L)**, **organizational unit (OU)**.

Anche se è stato specificato il nome di un namespace nel campo **O**, questo non ha un impatto diretto sul contesto del namespace nel quale l'utente può operare.
Kubernetes non associa automaticamente il namespace specificato nel campo **O** a un namespace nel cluster.

Il campo **O** può avere un impatto solo quando vengono utilizzati i gruppi nel contesto di Kubernetes RBAC. Ad esempio, se configurato un **ClusterRoleBinding** o un **RoleBinding** per assegnare ruoli a un gruppo, Kubernetes può associare l'utente al gruppo indicato nel campo **O** del certificato. Questo significa che è possibile definire regole che danno permessi ai gruppi e Kubernetes controllerà il campo **O** nel certificato per vedere se l'utente appartiene a quel gruppo.

Il passo successivo è quello di firmare la richiesta di certificato con la **certiication authority (CA)** del cluster kubernetes.

```bash
openssl x509 -req -in myuser.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out myuser.crt -days 365
```

Il parametro `CAcreateserial` indica che, se non esiste già un file seriale per la CA, ne verrà creato uno. Il file seriale tiene traccia del numero di serie univoco assegnato ai certificati firmati da quella CA.
Il parametro `day` indica la validità del certificato in giorni.

Con il comando seguente viene creato il context chiamato `myuser@hands-on` che definisce l'utente `myuser` e il namespace `myns`.

```bash
kubectl config set-context myuser@hands-on --cluster=kind-hands-on --user=myuser --namespace=myns
```

Il sguente comando configura l'utente nel file di configurazione di Kubernetes, associandolo al certificato e alla chiave privata.
Queste credenziali saranno utilizzate per autenticare l'utente quando sarà usato in un contesto.

```bash
kubectl config set-credentials myusers --client-certificate=./myuser.crt --client_key=./myuser.key 
```

Il comando seguente permette di modificare il contesto attuale con quello dell'utente appena creato.
Questa operazione ci permetterà di effettuare l'accesso al cluster con il nuovo utente.

```bash
kubectl config use-context myuser@hands-on
```