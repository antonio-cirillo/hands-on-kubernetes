# Certified Kubernetes Administrator (CKA) Practice Exam: Part 4

## Back Up the etcd Data

From the exam server, log in to the `etcd1` server and back up the etcd data to a file located at `/home/cloud_user/etcd_backup.db`.

```console
$ ssh etcd1
```

```console
$ ETCDCTL_API=3 etcdctl snapshot save /home/cloud_user/etcd_backup.db \
--endpoints=https://etcd1:2379 \
--cacert=/home/cloud_user/etcd-certs/etcd-ca.pem \
--cert=/home/cloud_user/etcd-certs/etcd-server.crt \
--key=/home/cloud_user/etcd-certs/etcd-server.key
```

## Restore the etcd Data from the Backup

On the `etcd1` server, restore your etcd data from the backup file at `/home/cloud_user/etcd_backup.db`.

```console
sudo systemctl stop etcd
```

```console
sudo rm -rf /var/lib/etcd
```

```console
$ sudo ETCDCTL_API=3 etcdctl snapshot restore /home/cloud_user/etcd_backup.db \
--initial-cluster etcd-restore=https://etcd1:2380 \
--initial-advertise-peer-urls https://etcd1:2380 \
--name etcd-restore \
--data-dir /var/lib/etcd
```

```console
$ sudo chown -R etcd:etcd /var/lib/etcd
```

```console
$ sudo systemctl start etcd
```