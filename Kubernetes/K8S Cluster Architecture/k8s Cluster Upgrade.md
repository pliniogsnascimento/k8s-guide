```bash
kubectl version

kubeadm upgrade plan

kubectl cordon controlplane
kubectl drain controlplane --ignore-daemonsets

apt-get upgrade kubeadm kubelet
apt-get install kubeadm=1.26.0-00 kubelet=1.26.0-00

kubeadm upgrade apply v1.26.0
systemctl restart kubelet

kubectl uncordon controlplane

apt-get update
apt-get upgrade kubelet kubeadm -y --allow-change-held-packages
kubeadm upgrade node phase kubelet-config
```