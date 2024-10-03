# Implement etcd backup and restore

In this category of questions, you may be asked to perform either a backup or a restore of the ETCD server, or both. 
Pay close attention to the question, as it might ask for the restore of a backup different from the one just created.

The keyword to search in the documentation is: "**ETCD snapshot**".

To perform a backup of the ETCD server, three files are required:

- The Cluster Certificate Authority.
- The Client Private Key.
- The Client Certificate.

Restoring a backup is done using a directory called `data-dir`.
It is crucial that the ETCD server has the correct permissions for this directory.
Furthermore, if the ETCD server is running as a pod, you must modify the volume mounted on the pod, updating the path with the value specified in the data-dir parameter.