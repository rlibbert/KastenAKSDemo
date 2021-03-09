# KastenAKSDemo
Steps to get Kasten running in your AKS cluster and a 3 node kafka instance to use as a model for backup and restore.


#Get this from Azure Active Directory or use:
    
    az account show --subscription "MySubscriptionName" --query tenantId --output tsv


#tenantID = 72f988bf**************

#Get SPClient ID:

    az aks show --name $AKS_CLUSTER_NAME --resource-group $AKS_CLUSTER_RESOURCE_GROUP --query servicePrincipalProfile.clientId -o tsv

#Service Princpal Client ID: da9e4d24****************

#Go create a client ID/Secret for the service Principal in AAD / App registrations
#----Rob's Testing Credentials----
#Secret: 3feee15f************
#CID: 968cc8ae-76a***********


#Set env variables and execute:
    helm install k10 kasten/k10 --namespace=kasten-io \
        --set secrets.azureTenantId=<tenantID> \
        --set secrets.azureClientId=<azureclient_id> \
        --set secrets.azureClientSecret=<azureclientsecret>

#    Validate the instalation:

    kubectl get pods --namespace kasten-io --watch

#Forward a local port to the K10 Ingresss Port:
    kubectl --namespace kasten-io port-forward service/gateway 8080:8000

#You can now access the GUI here:
http://127.0.0.1:8080/k10/#/
