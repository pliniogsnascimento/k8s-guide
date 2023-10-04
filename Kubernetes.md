# Kubernetes

> This section aims to explain about kubernetes architecture. A practical approach of installing a cluster is noted in [[Kubernetes Hard Way(containerd)]]. All learned in [certified kubernetes administrator course](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/learn).

Kubernetes is an open source container orchestrator, initially developed by google and now with support by CNCF.
![components-of-kubernetes.svg](components-of-kubernetes.svg)
[Kubernetes Components | Kubernetes](https://kubernetes.io/docs/concepts/overview/components/)

### Common resources
-   Container runtime: Runtime engine to run containers
-   Container network interface(CNI) 

### Nodes
-   Worker nodes
-   Control plane nodes

### Control plane
Control plane is a set of applications tha aims to manage, plan, schedule and monitor kubernetes resources:

-   [[ETCD]]: KV Database to store cluster and resources configuration;
-   [[kube-scheduler]]: Identifies best nodes to place containers, based on resource requirements, worker nodes capacity, taints and tolerations, node affinity, etc;
-   Controllers: Control loops that watch the state of cluster and then make or request changes as needed to match the desired state. It tracks at least one object;
-   [[kube-controller-manager]]: Single binary that runs all controllers;
	- Controllers: Manages kubernetes resources to keep desired state. Examples: Node controller, Replication controller, etc.
-   [[Kube API Server]]: Orchestrate all operations on the cluster, via api. Fetches kubelet periodically to get nodes and pods status;
-   Manifests directory: /etc/kubernetes/manifests.

### Worker nodes
Host applications as containers
-   [[kubelet]]: Agent that runs on each nodes that listens instructions from apiserver to create, destroy and manage containers in node.
-   [[kube-proxy]]: Ensure that necessary network rules are created to make communications between nodes are possible.

# Concepts
- [[Scheduling]]

# Utils

### Links
- [CKAD Exercises](https://github.com/dgkanatsios/CKAD-exercises)

### Code Snippets
Kind cluster:
```bash
# Creates cluster with 2 worker nodes
cat <<EOF | kind create cluster --config -
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: cka-cluster
nodes:
- role: control-plane
  image: kindest/node:v1.23.6
- role: worker
  image: kindest/node:v1.23.6
- role: worker
  image: kindest/node:v1.23.6
EOF

# Deletes cluster
kind delete cluster --name cka-cluster
```
