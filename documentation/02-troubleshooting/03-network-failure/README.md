# Troubleshoot network failure

In case of connectivity issues, check the following:

- Network Plugin Installation: Verify if the network plugin (CNI) is correctly installed and running. 
- Check the status of the `kube-proxy` component.
  For a kubeadm cluster, look for the kube-proxy pod in the `kube-system` namespace.
  
If `kube-proxy` is not running or is encountering issues, check its logs for more information.
