# OSBA-Azure-AKS
Sample project for demoing OSBA (Open Service Broker API) for Azure and Kubernetes/AKS.


# Requirements:
1) Kubernetes cluster
2) kubectl CLI
3) svcat CLI


helm init --upgrade


helm repo add svc-cat https://svc-catalog-charts.storage.googleapis.com


helm install svc-cat/catalog --name catalog --namespace catalog --set rbacEnable=false --set apiserver.storage.etcd.persistence.enabled=true --set apiserver.healthcheck.enabled=false --set controllerManager.healthcheck.enabled=false --set apiserver.verbosity=2 --set controllerManager.verbosity=2


kubectl get apiservice


helm repo add azure https://kubernetescharts.blob.core.windows.net/azure


az ad sp create-for-rbac


helm install azure/open-service-broker-azure --name osba --namespace osba \
    --set azure.subscriptionId=$AZURE_SUBSCRIPTION_ID \
    --set azure.tenantId=$AZURE_TENANT_ID \
    --set azure.clientId=$AZURE_CLIENT_ID \
    --set azure.clientSecret=$AZURE_CLIENT_SECRET


svcat get brokers
svcat get classes
svcat get plans

# Create the SQL instance and Database
kubectl create -f sql-instance.yaml

# check the created resources in Azure Portal
# check the created resources in CLI
kubectl get ServiceInstances
svcat get instances

# Create the Service Binding
kubectl create -f sql-binding.yaml

# The Service Binding will create a Kubernetes Secret with username, password, database name, server host, port number...
kubectl get secrets
kubectl describe secret <secret-name>

Check the created Binding using kubectl and svcat CLI:
kubectl get ServiceBindings
svcat get bindings


src: https://docs.microsoft.com/en-us/azure/aks/integrate-azure