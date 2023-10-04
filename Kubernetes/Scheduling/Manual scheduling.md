For deleting scheduler in kind and test manual scheduling, run:
```bash
# Enter on controlplane node
docker exec -it cka-cluster-control-plane sh

# Delete the scheduler manifest
rm /etc/kubernetes/manifests/kube-scheduler.yaml
```

### Schedule with nodeName
Set nodeName for manual scheduling:
```yaml
# Pod Example
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 8080
  nodeName: node02 # Set this for manual scheduling
```

```bash
# Recreates forcing
kubectl replace --force -f nginx.yaml
```

### Schedule with Binding
If pod already exists, create a binding object:
```yaml
# Binding YAML
apiVersion: v1
kind: Binding
metadata:
  name: nginx
target:
  apiVersion: v1
  kind: Node
  name: cka-cluster-worker
```

```bash
# Sample request
curl --header "Content-Type:application/json" --request POST --data $JsonData http://{server}/api/v1/namespaces/{namespace}/pods/{name}/binding

curl --header "Content-Type:application/json" --request POST --data '{"apiVersion":"v1","kind":"Binding","metadata":{"name":"nginx"},"target":{"apiVersion":"v1","kind":"Node","name":"cka-cluster-worker"}}' http://localhost:8080/api/v1/namespaces/default/pods/nginx/binding/

# Post example
curl --header "Content-Type:application/json" --request POST --data '{"apiVersion":"v1","kind":"Binding","metadata":{"name":"nginx"},"target":{"apiVersion":"v1","kind":"Node","name":"node01"}}' http://localhost:8080/api/v1/namespaces/default/pods/nginx/binding/
```

#### Notes
-   Kubernetes will not allow modify nodeName property
-   It is not possible to get bindings
-   It is not possible to create a binding with kubectl create. To create a binding, use the api instead.
