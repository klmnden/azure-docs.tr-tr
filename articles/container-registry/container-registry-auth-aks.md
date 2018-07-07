---
title: Azure Container Registry'den Azure Kubernetes hizmeti ile kimlik doğrulaması
description: Özel kapsayıcı kayıt defterinizde görüntüleri için Azure Kubernetes hizmetindeki bir Azure Active Directory Hizmet sorumlusu kullanarak erişmeyi öğrenin.
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 02/24/2018
ms.author: marsma
ms.openlocfilehash: 36b66fdcd157e4c44fcd451c8cc07ba68242b583
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888771"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Azure Container Registry'den Azure Kubernetes hizmeti ile kimlik doğrulaması

Azure Kubernetes Service (AKS) ile Azure Container Registry (ACR) kullanırken, bir kimlik doğrulama mekanizması kurulması gerekir. Bu belge, bu iki Azure Hizmetleri arasındaki kimlik doğrulaması için önerilen yapılandırmaları açıklanmaktadır.

## <a name="grant-aks-access-to-acr"></a>ACR verme AKS erişimi

Bir AKS kümesi oluşturulurken küme çalışabilirlik ile Azure kaynaklarını yönetmek için de bir hizmet sorumlusu oluşturulur. Bu hizmet sorumlusunu bir ACR kayıt defteri ile kimlik doğrulaması için de kullanılabilir. Bunu yapmak için bir rol ataması ACR kaynak hizmet sorumlusu okuma erişimi vermek için oluşturulması gerekir.

Aşağıdaki örnek, bu işlemi tamamlamak için kullanılabilir.

```bash
#!/bin/bash

AKS_RESOURCE_GROUP=myAKSResourceGroup
AKS_CLUSTER_NAME=myAKSCluster
ACR_RESOURCE_GROUP=myACRResourceGroup
ACR_NAME=myACRRegistry

# Get the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER_NAME --query "servicePrincipalProfile.clientId" --output tsv)

# Get the ACR registry resource id
ACR_ID=$(az acr show --name $ACR_NAME --resource-group $ACR_RESOURCE_GROUP --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role Reader --scope $ACR_ID
```

## <a name="access-with-kubernetes-secret"></a>Kubernetes gizli ile erişim

Bazı durumlarda, hizmet sorumlusu AKS tarafından kullanılan ACR kayıt defterine kapsama alınamaz. Bu durumlarda, benzersiz bir hizmet sorumlusu oluşturma ve yalnızca ACR kayıt defterine kapsam.

Aşağıdaki komut, hizmet sorumlusu oluşturmak için kullanılabilir.

```bash
#!/bin/bash

ACR_NAME=myacrinstance
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate the ACR login server and resource id.
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create a contributor role assignment with a scope of the ACR resource.
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --role Reader --scopes $ACR_REGISTRY_ID --query password --output tsv)

# Get the service principle client id.
CLIENT_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output used when creating Kubernetes secret.
echo "Service principal ID: $CLIENT_ID"
echo "Service principal password: $SP_PASSWD"
```

Hizmet sorumlusu kimlik bilgileri artık bir Kubernetes depolanabilir [görüntü çekme gizli] [ image-pull-secret] ve bir AKS kümesinde çalışan kapsayıcılar, başvurulan.

Aşağıdaki komut, Kubernetes gizli dizi oluşturur. Sunucu adını ACR oturum açma sunucusuyla, kullanıcı adı, hizmet sorumlusu kimliği ve parola ile hizmet sorumlusu parolası ile değiştirin.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

Kubernetes gizliliği pod kullanarak dağıtım kullanılabilir `ImagePullSecrets` parametresi.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: acr-auth-example
spec:
  template:
    metadata:
      labels:
        app: acr-auth-example
    spec:
      containers:
      - name: acr-auth-example
        image: myacrregistry.azurecr.io/acr-auth-example
      imagePullSecrets:
      - name: acr-auth
```

<!-- LINKS - external -->
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[image-pull-secret]: https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets
