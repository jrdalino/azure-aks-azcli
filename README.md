# Azure Kubernetes Service (AKS) using AZ CLI

### Prerequisites
- homebrew
- docker
- git

### Clone the sample repository
```
$ cd ~/environment
$ git clone https://github.com//Azure-Samples/azure-voting-app-redis
```

### Run Locally and test locally at http://localhost:8080
```
$ docker-compose up -d
$ docker images
$ docker ps
# docker-compose down
```

### Install Azure CLI for Mac using Homebrew
```
$ brew update && brew install azure-cli

$ az login
```

### Create a new Resource Group
```
$ az group create \
--name rg-jrdalino-aksdemo \
--location southeastasia
```

### Create Azure Container Registry
```
$ az acr create \
--resource-group rg-jrdalino-aksdemo \
--name jrdalinoaksdemoacr \
--sku basic
```

### Login to ACR
```
$ az acr login \
--name jrdalinoaksdemoacr
```

### Tag Docker Image with ACR Login Server
```
$ az acr list
$ docker tag azure-vote-front jrdalinoaksdemoacr.azurecr.io/azure-vote-front:v1
$ docker images
```

### Push Images to Registry and verify
```
$ docker push jrdalinoaksdemoacr.azurecr.io/azure-vote-front:v1
$ az acr repository list \
--name jrdalinoaksdemoacr
$ az acr repository show-tags \
--name jrdalinoaksdemoacr \
--repository azure-vote-front \
--output table
```

### Create Service Principal to allow AKS Cluster to interact with other Azure Resources
```
$ az ad sp create-for-rbac \
--skip-assignment
```
```
{
  "appId": "0a41692f-f9f2-47ed-98d6-d997aEXAMPLE",
  "displayName": "azure-cli-2020-02-29-04-53-00",
  "name": "http://azure-cli-2020-02-29-04-53-00",
  "password": "7d57b9ab-270e-47bc-98bc-f9cd3EXAMPLE",
  "tenant": "df7fa6cf-2f69-491d-bacd-8caa9EXAMPLE"
}
```

### Configure Azure Container Registry for Authentication
```
$ az acr show \
--resource-group rg-jrdalino-aksdemo \
--name jrdalinoaksdemoacr
```
```
{
  "adminUserEnabled": false,
  "creationDate": "2020-02-29T04:34:22.567553+00:00",
  "id": "/subscriptions/9ff4bd6d-47af-4022-a8ca-b8b92a368e0a/resourceGroups/rg-jrdalino-aksdemo/providers/Microsoft.ContainerRegistry/registries/jrdalinoaksdemoacr",
  "location": "southeastasia",
  "loginServer": "jrdalinoaksdemoacr.azurecr.io",
  "name": "jrdalinoaksdemoacr",
  "networkRuleSet": null,
  "policies": {
    "quarantinePolicy": {
      "status": "disabled"
    },
    "retentionPolicy": {
      "days": 7,
      "lastUpdatedTime": "2020-02-29T04:34:24.536341+00:00",
      "status": "disabled"
    },
    "trustPolicy": {
      "status": "disabled",
      "type": "Notary"
    }
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "rg-jrdalino-aksdemo",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### Grant Correct access for the AKS Cluster to use the images stored on Azure Container Registry. Assign permissions to the service Principal

### Create AKS Cluster
```
$ az aks create \
--resource-group rg-jrdalino-aksdemo \
--name jrdalinoaksdemo\
--node-count 1 \
--service-principal ENTER_HERE \
--client-secret ENTER_HERE \
--generate-ssh-keys
```

### Install Kubetcl and update System Path Environment
```
$ az aks install-cli
```

### Get Credentials and Test kubectl access
```
$ az aks get-credentials \
--resource-group rg-jrdalino-aksdemo \
--name jrdalinoaksdemo
$ kubectl get nodes
```

### Run and Scale App
```
$
```
