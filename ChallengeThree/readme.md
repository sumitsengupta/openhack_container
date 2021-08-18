## Yaml for Pod Deployment

* the *poipoddeployment.yaml* file is an example of creating a pod inside our AKS cluster

1. Each pod needs to have a service deployed along side to handle http traffic. 
2. Each service needs to have a portforward enabled to allow access from a local dev machine. Pods are able to communicate with each other from within the cluster itself.

##Key Notes

### yaml features to deploy a pod properly
1. kind: Deployment .

2. The following is intended for a friendly name. Used for dev interaction.
```
metadata:
  name: pod-poi-img
  ```
3. The following is how a pod is reference from other attributes inside the cluster. This name will be what is reference when you deploy a service with the pod.
```
    selector:
        matchLabels:
            app: poi
```
4. The following refernces the image name within our ACR. You need to supply any environment variables neccesary for the application to function. This will be best solved utilizing Config Maps or Config Secrets, ultimately sitting within a key vault
```
spec:
      containers:
      - name: poi-container
        image: registrybwj5358.azurecr.io/poi:1.0
        env:
        - name: SQL_USER
          value: "sqladminbWj5358"
        - name: SQL_PASSWORD
          value: "bN1uq3Fc9"
        - name: SQL_SERVER
          value: "sqlserverbwj5358.database.windows.net"
        - name: SQL_DBNAME
          value: "mydrivingDB"
```
### yaml features to deploy a service properly
1. The following is intended for a friendly name. Used for dev interaction.
```
metadata:
  name: poi-service
  ```
2. The following is how a pod is reference from other attributes inside the cluster. This name needs to match the name of the pod which the service is connected to.
```
  spec:
  selector:
    app: poi
```

## Commands to deploy and check resources within the cluster


- kubectl apply -f poipoddeployment.yml
    - This will deploy the contents to AKS. For us we have built both the Pod and Servce in one.
- kubectl port-forward service/poi-service 8080:80
    - Port forwards the traffice to the service associated with the pod
- kubectl exec -it pod-poi-img-pnxl7 -- sh
    - get the shell to a container running in a pod
- kubectl get pods
    - will list all the pods within the cluster
- kubectl delete pod pod-poi-img-pnxl7
    - will delete the specified pod

## General kubectl commands to hunt down issues

Here is a cheat sheet on some common kubectl commands:
kubectl [command] [type] [name]
| command | decsript |
|--|--|
| kubectl get |	used to view and find resources,can output JSON, YAML, or be directly formatted |
| kubectl describe | retrieve extra information about a resources, needs a resource type and (optionally) a resource name |
| kubectl logs | view container logs for debugging |
