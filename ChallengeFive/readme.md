## Yaml for Pod Deployment with namespaces
- create namespace
```
kubectl create namespace [name]
```
- To deploy pod to cluster under a namepsace
```
kubectl apply -f [pod.yaml] --namespace=[name]
```