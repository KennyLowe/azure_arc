# Overview

The following README will guide you on how to connect an existing Kubernetes cluster to Azure Arc using a simple shell script.

# Prerequisites

* Make sure your *kubeconfig* file is configured properly and you are working against your [k8s cluster context](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/). 

* (Optional) To simplify work against multiple k8s contexts, consider using [kubectx](https://github.com/ahmetb/kubectx).

* Create Azure Service Principal (SP)   

    To connect the K3s cluster installed on the VM to Azure Arc, Azure Service Principal assigned with the "Contributor" role is required. To create it, login to your Azure account run the following command:

    ```az login```

    ```az ad sp create-for-rbac -n "http://AzureArcK8s" --role contributor```

    Output should look like this:
    ```
    {
    "appId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    "displayName": "AzureArcK8s",
    "name": "http://AzureArcK8s",
    "password": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    "tenant": "XXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    }
    ```
    **Note**: It is optional but highly recommended to scope the SP to a specific [Azure subscription and Resource Group](https://docs.microsoft.com/en-us/cli/azure/ad/sp?view=azure-cli-latest)

* Create a new Azure Resource Group where you want your cluster(s) to show up. 

* Download the [az_connect_k8s](../scripts/az_connect_k8s.sh) shell script.

![](../img/onboard_k8s/01.png)

* Change the environment variables according to your environment. 

![](../img/onboard_k8s/02.png)

# Deployment

Run the script using the ```. ./az_connect_k8s.sh``` command. 

**Note**: The extra dot is due to the script has an *export* function and needs to have the vars exported in the same shell session as the rest of the commands. 

Upon completion, you will have your Kubernetes cluster, connected as a new Azure Arc cluster inside your Resource Group. 

![](../img/onboard_k8s/03.png)

![](../img/onboard_k8s/04.png)

![](../img/onboard_k8s/05.png)

# Delete the deployment

The most straightforward way is to delete the cluster is via the Azure Portal, just select cluster and delete it. 

![](../img/onboard_k8s/06.png)

If you want to nuke the entire environment, just delete the Azure Resource Group.

![](../img/onboard_k8s/07.png)