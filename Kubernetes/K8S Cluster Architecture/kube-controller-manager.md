A Controller is a loop that monitor the actual state of a resource and tries to keep the desired state. The kube controller manager manages various controllers on the cluster. Some examples of controllers:

-   Node controller
-   Replication Controller
-   Deployment Controller
-   Namespace Controller
-   PV Binder Controller
-   PV Protection Controller
-   Endpoint Controller

```bash
# Install
wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-controller-manager

cat /etc/systemd/system/kube-controller-manager.service
```
