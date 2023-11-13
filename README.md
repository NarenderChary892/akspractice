1. Create [Azure Kubernetes Service(AKS)](https://learn.microsoft.com/en-us/azure/aks/learn/quick-kubernetes-deploy-portal?tabs=azure-cli)

2. Give below RBAC access to AKS to yourself you create AKS auth type as "Azure AD authentication with Azure RBAC"<br/>
     RBAC name: Azure Kubernetes Service RBAC Cluster Admin

3. Connect to AKS from cmd<br/>
    az account set --subscription SUBSCRIPTION-ID<br/>
    az aks get-credentials --resource-group RESOURCE-GROUP-NAME --name AKS-NAME<br/>

4. Deploy AKS [dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)<br/>
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

5. Create [dashboard-admin.yaml](https://github.com/khandelwal-arpit/kubernetes-starterkit/blob/master/dashboard-admin.yaml)

6. Create admin-user token to access dashboard<br/>
    kubectl -n kube-system create token admin-user   

7. Access dashboard using admin-user token<br/>
    Command: kubectl proxy<br/>
    Dashboard URL: http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/<br/>

8. Create a sample web application and dockerize it<br/>
    Reference: https://dev.to/github/publishing-a-docker-image-to-githubs-container-repository-4n50

9. Create [Deployment.yaml](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#creating-a-deployment)<br/>
    Note: Make sure add the ghcr token to deployment.yaml if you're using private ghcr
    
10. Create the GHCR Token<br/>
    kubectl create secret docker-registry github-container-registry  --docker-server=ghcr.io --docker-username=YOUR-GITHUB-ID --docker-password=YOUR-GITHUB-ACCESS-TOKEN<br/>
    Note: Create a classic token with write and delete package permissions

11. Create the Azure Credentials<br/>
    az ad sp create-for-rbac --name "narender-aks-sp-sdk-auth" --role contributor --scopes /subscriptions/SUBSCRIPTION-ID/resourceGroups/RESOURCE-GROUP-NAME --sdk-auth  

12.  Create Azure Credentials [secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository) in GitHub actions from above step

13.  Grant "Kubernetes Service RBAC Cluster Admin" RBAC access to "narender-aks-sp-sdk-auth" SP to AKS

14.  Deploy a [service](https://kubernetes.io/docs/concepts/services-networking/service/#defining-a-service) and serve the outside
