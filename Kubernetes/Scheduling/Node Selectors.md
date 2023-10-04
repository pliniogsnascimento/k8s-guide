NodeSelectors are selectors that ensure a pod run on nodes with the specified labels.

```bash
# Label node for using it with a selector
kubectl label no <node> <key>=<value>

# Example node labeling
kubectl label no cka-cluster-worker size=Large
```

```yaml
# Example nodeSelector
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  nodeSelector:
    size: Large
```

##### Limitations
Node selector only work with single label and selector, and only matching. Cannot use:
-   OR operator: Large OR Medium
-   NOT operator: NOT Small

For this, use nodeAffinity and AntiAffinity instead.