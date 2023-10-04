Primary component in Kubernetes. It is responsible for authenticating, validating requests, retrieving and updating data in ETCD.

```bash
# Install
wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-apiserver

# Access the API
kubectl proxy --port=8080 &
curl http://localhost:8080/api/
```

##### Pod configuration
```yaml
spec:
  containers:
  - command:
    - kube-apiserver
    - --advertise-address=172.18.0.4
    - --allow-privileged=true
    - --authorization-mode=Node,RBAC
    - --client-ca-file=/etc/kubernetes/pki/ca.crt
    - --enable-admission-plugins=NodeRestriction
    - --enable-bootstrap-token-auth=true
    - --etcd-cafile=/etc/kubernetes/pki/etcd/ca.crt
    - --etcd-certfile=/etc/kubernetes/pki/apiserver-etcd-client.crt
    - --etcd-keyfile=/etc/kubernetes/pki/apiserver-etcd-client.key
    - --etcd-servers=https://127.0.0.1:2379
    - --kubelet-client-certificate=/etc/kubernetes/pki/apiserver-kubelet-client.crt
    - --kubelet-client-key=/etc/kubernetes/pki/apiserver-kubelet-client.key
    - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
    - --proxy-client-cert-file=/etc/kubernetes/pki/front-proxy-client.crt
    - --proxy-client-key-file=/etc/kubernetes/pki/front-proxy-client.key
    - --requestheader-allowed-names=front-proxy-client
    - --requestheader-client-ca-file=/etc/kubernetes/pki/front-proxy-ca.crt
    - --requestheader-extra-headers-prefix=X-Remote-Extra-
    - --requestheader-group-headers=X-Remote-Group
    - --requestheader-username-headers=X-Remote-User
    - --runtime-config=
    - --secure-port=6443
    - --service-account-issuer=https://kubernetes.default.svc.cluster.local
    - --service-account-key-file=/etc/kubernetes/pki/sa.pub
    - --service-account-signing-key-file=/etc/kubernetes/pki/sa.key
    - --service-cluster-ip-range=10.96.0.0/16
    - --tls-cert-file=/etc/kubernetes/pki/apiserver.crt
    - --tls-private-key-file=/etc/kubernetes/pki/apiserver.key
```

# Security
kube-apiserver is the component responsible for security in Kubernetes, managing authentication, authorization, TLS certificates and more.

### Authentication
There are four ways of configuring authentication:
- Static password file
- Static token file
- Certificates
- Idendity services - LDAP, Kerberos

##### Static password file
> [!INFO] Configuring it requires a restart from the server
```csv
password123,user1,u0001,group1
password123,user2,u0002,group1
password123,user3,u0003
password123,user4,u0004
```
Configure passing the parameter `--basic-auth-file=user-details.csv` to kube-apiserver. Example of authentication:
```bash
curl -v -k https://master-node-ip:6443/api/v1/pods -u "user1:password123"
```

##### Static token file
Configure passing the parameter `--token-auth-file=user-details.csv` to kube-apiserver. Example of authentication:
```bash
curl -v -k https://master-node-ip:6443/api/v1/pods --header "Authorization: Bearer <token-here>"
```