Use taints to “repel” a pod in a node. Use tolerations to tolerate taints present in a node. Taints are created by passing a key-value pair and adding an effect to it.

##### Effects
Effects tell what Kubernetes should do when a pod is created and the node have a toleration. Types of effect:
-   NoSchedule
-   PreferNoSchedule
-   NoExecute

##### Taints
```bash
# Get taints in a node
kubectl get no <node> -ojsonpath-as-json={.items..spec.taints}

# Add a taint to a node
kubectl taint node <node> <key>=<value>:<effect>

# Example taint for consul application
kubectl taint node cka-cluster-worker app=consul:NoSchedule
```

##### Tolerations
```bash
# A simple pod. Won't schedule because does not tolerate the taints
kubectl run weak-nginx --image=nginx
```

```yaml
# A strong pod. It tolerates the taints.
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: strong-nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  tolerations:
  - key: app
    operator: "Equal"
    value: "nginx"
    effect: "NoSchedule"
```

##### Operators
-   Exists: If the key is specified
-   Equal: If the value is equal

##### Notes
-   Taint will only work for unscheduled pods. If you scheduled it manually, it won’t work.