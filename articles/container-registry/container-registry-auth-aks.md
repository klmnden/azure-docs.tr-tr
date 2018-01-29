---
title: "Azure kapsayıcı kayıt defterinden Azure kapsayıcı hizmeti ile kimlik doğrulaması"
description: "Özel kapsayıcı kaydınız görüntülere Azure kapsayıcı hizmetinden bir Azure Active Directory hizmet asıl kullanarak erişilmesine öğrenin."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/12/2018
ms.author: nepeters
ms.openlocfilehash: 86a160d8f2dbfb0e385d9dbed7cf6d789f5a8df6
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-service"></a>Azure kapsayıcı kayıt defterinden Azure kapsayıcı hizmeti ile kimlik doğrulaması

Azure kapsayıcı hizmeti (AKS) ile Azure kapsayıcı kayıt defteri (ACR) kullanırken, bir kimlik doğrulama mekanizması kurulması gerekir. Bu belgede bu iki Azure hizmetleri arasında kimlik doğrulaması için önerilen yapılandırmaları ayrıntılarını verir.

## <a name="grant-aks-access-to-acr"></a>ACR GRANT AKS erişimi

AKS küme oluşturulduğunda, bir hizmet sorumlusu küme çalışabilirlik Azure kaynakları ile yönetmek için de oluşturulur. Bu hizmet sorumlusu ACR kayıt defteri kimlik doğrulaması için de kullanılabilir. Bunu yapmak için bir rol ataması ACR kaynak için hizmet asıl okuma yetkisi vermek için oluşturulması gerekir. 

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

## <a name="access-with-kubernetes-secret"></a>Kubernetes gizliliği ile erişim

Bazı durumlarda, hizmet sorumlusu AKS tarafından kullanılan ACR kayıt defterine kapsama alınamaz. Bu durumlarda, benzersiz bir hizmet sorumlusu oluşturmak ve yalnızca ACR kayıt defterine kapsam.

Aşağıdaki komut dosyası, hizmet sorumlusu oluşturmak için kullanılabilir. 

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

Hizmet asıl kimlik bilgilerini bir Kubernetes şimdi depolanabilir [görüntü çekme gizli] [ image-pull-secret] ve kapsayıcıları AKS kümede çalışırken başvurulabilir. 

Aşağıdaki komut Kubernetes gizli oluşturur. ACR oturum açma sunucusu ile sunucu adı, kullanıcı adı Hizmet asıl kimliği ve parolası hizmet asıl parolası ile değiştirin.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

Kubernetes gizli pod kullanarak dağıtımı kullanılabilir `ImagePullSecrets` parametresi. 

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
