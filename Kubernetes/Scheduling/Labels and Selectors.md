### Labels and Selectors
Labels and selectors are used to group things together. Labels are used to identify resources, selectors can be used to manage some resources based on labels.

```yaml
# Labels examples in pod

apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
    tier: frontend
spec:
  containers:
  - name: nginx
    image: nginx
```

```bash
# Get pods by label
kubectl get pods -l tier=frontend
kubectl get pods --selector tier=frontend
```

```yaml
# Example of selector. ReplicaSet uses selector to manage pods with the labels specified
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx
  labels:
    app: nginx
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
```

### Annotations
Use annotations for informative purposes.

```yaml
# Example of annotations
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
  annotations:
    buildVersion: "1.34"
    deployedBy: Plinio
spec:
  containers:
  - name: nginx
    image: nginx
```