## Yaml for Pod Deployment with namespaces
***
#### Summary
- This folder contains the pod deployment files from challenge three with a slight modification.
Challenge five requires namespaces and seperating the deploys. The existing deployment files have been modified to utilize the new namespaces.

- For This challenge the two new namespaces are *Web* and *API*.
# Service NEEDS to be in the SAME namespace as the related POD
***
### Resources
- Keyvault = team11aksvault 
    - access given to team11 AAD
    - currenlty has 4 secretes
- https://docs.microsoft.com/en-us/azure/aks/manage-azure-rbac

-docs
    https://docs.microsoft.com/en-us/azure/aks/csi-secrets-store-driver
#### Commands
- create namespace
```
kubectl create namespace [name]
```
- To deploy pod to cluster under a namepsace
```
kubectl apply -f [pod.yaml] --namespace=[name]
```


az aks enable-addons --addons azure-keyvault-secrets-provider --name ch4aks --resource-group teamResources

  "addonProfiles": {
    "azureKeyvaultSecretsProvider": {
      "config": {
        "enableSecretRotation": "false"
      },
      "enabled": true,
      "identity": {
        "clientId": "a3f79df4-940a-4c80-a5fe-3e07916fab2d",
        "objectId": "a6776d31-7a5e-404e-bd89-53beb04e34da",
        "resourceId": "/subscriptions/31cce40a-5535-4e30-8458-246e62c3a763/resourcegroups/MC_teamResources_ch4aks_westus/providers/Microsoft.ManagedIdentity/userAssignedIdentities/azurekeyvaultsecretsprovider-ch4aks"
      }



--connection permission to keyvault from cluster user-assigned managed identity (clientID)
az keyvault set-policy -n team11aksvault --secret-permissions backup delete get list purge recover restore set --spn a3f79df4-940a-4c80-a5fe-3e07916fab2d
az keyvault set-policy -n team11aksvault --secret-permissions backup delete get list purge recover restore set --spn 09b50902-a23f-46a2-ad56-0bad74b87485



-redeploy modified pods which now utilize the csi

- command used to verify pods are using csi
      kubectl get pods -n api -l 'app in (secrets-store-csi-driver, secrets-store-provider-azure)'