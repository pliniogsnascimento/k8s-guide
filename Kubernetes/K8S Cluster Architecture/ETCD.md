ETCD is a distributed reliable key-value store that is simple, secure and fast. Stores all configuration from Kubernetes resources(Pods, nodes, Replicates, Deployments).
[etcd documentation](https://etcd.io/)

#### Key value store example
| key | value |
|-----|------|
| Name | Plinio |
| Birthdate | 09/17/1998 |
| Genre | Men |

### Install using docker
```shell
# Run as interactive terminal and attach to it in another shell
docker run -it --rm --name Etcd -e ALLOW_NONE_AUTHENTICATION=yes -e ETCDCTL_API=2 bitnami/etcd

# Run as interactive terminal and attach to it in another shell
docker run -d --rm --name Etcd -e ALLOW_NONE_AUTHENTICATION=yes bitnami/etcd

# Attach to container
docker exec -it Etcd sh

# Attach to pod in k8s
k exec -it -n kube-system etcd-cka-cluster-control-plane -- sh
```
Check the [image documentation](https://hub.docker.com/r/bitnami/etcd) for more info.

#### Important info
-   Default port: 2379
-   Has 2 api-versions: 2 and 3. 
	- When not set, assume 2 as default.
	- v2.0 came on Feb-2015
	- v3.1 came January-2017
	- CNCF incubation on Nov-2018

### etcdctl

API Version v2:
```bash
# Get api version
etcdctl --version

etcdctl backup
etcdctl cluster-health
etcdctl mk
etcdctl mkdir

# Put key
etcdctl set key1 val1

# List keys
etcdctl get / --prefix --keys-only

```

API Version v3:
```bash
etcdctl version
# Put and get key on api 3
etcdctl put greeting "Hello, etcd"
etcdctl get greeting
```

### Pod Configuration

For running ETCD on kubernetes, it is advised running multiple ControlPlane nodes and setup ETCD in cluster mode.

```yaml
spec:
  containers:
  - command:
    - etcd
    - --advertise-client-urls=https://172.18.0.3:2379
    - --cert-file=/etc/kubernetes/pki/etcd/server.crt # cert
    - --client-cert-auth=true
    - --data-dir=/var/lib/etcd
    - --initial-advertise-peer-urls=https://172.18.0.3:2380
    - --initial-cluster=cka-cluster-control-plane=https://172.18.0.3:2380 # Specify different instances of ETCD for HA Setup
    - --key-file=/etc/kubernetes/pki/etcd/server.key # key
    - --listen-client-urls=https://127.0.0.1:2379,https://172.18.0.3:2379
    - --listen-metrics-urls=http://127.0.0.1:2381
    - --listen-peer-urls=https://172.18.0.3:2380
    - --name=cka-cluster-control-plane
    - --peer-cert-file=/etc/kubernetes/pki/etcd/peer.crt
    - --peer-client-cert-auth=true
    - --peer-key-file=/etc/kubernetes/pki/etcd/peer.key
    - --peer-trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
    - --snapshot-count=10000
    - --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt # cacert
```

### ETCD Backup

ETCD backups are a way of backing up kubernetes clusters, since all data are stored on it. Backing up consists of creating snapshots of its data for a future restore, if needed.

##### Creating a snapshot
```bash
# Mandatory use of API v3
export ETCDCTL_API=3
etcdctl snapshot save snapshot.db
etcdctl snapshot save snapshot.db \
	--endpoints=https://127.0.0.1:2379 \
	--cacert=/etc/etcd/ca.crt \
	--cert=/etc/etcd/etcd-server.crt \
	--key=/etc/etcd/etcd-server.key 

# Checking the snapshot
etcdctl snapshot status snapshot.db
etcdctl snapshot restore snapshot.sb --data-dir /var/lib/etcd-from-backup
```

##### Restoring from a snapshot
```bash
# Checking the snapshot
etcdctl snapshot status snapshot.db
etcdctl snapshot restore snapshot.sb --data-dir /var/lib/etcd-from-backup
```

After a restore, modify ETCD parameters and restart it.

[?(@.type==)]