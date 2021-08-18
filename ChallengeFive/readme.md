## Yaml for Pod Deployment with namespaces
***
#### Summary
- This folder contains the pod deployment files from challenge three with a slight modification.
Challenge five requires namespaces and seperating the deploys. The existing deployment files have been modified to utilize the new namespaces.

- For This challenge the two new namespaces are *Web* and *API*.
***
#### Commands
- create namespace
```
kubectl create namespace [name]
```
- To deploy pod to cluster under a namepsace
```
kubectl apply -f [pod.yaml] --namespace=[name]
```