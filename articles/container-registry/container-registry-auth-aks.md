---
title: Azure Container Registry'den Azure Kubernetes hizmeti ile kimlik doğrulaması
description: Özel kapsayıcı kayıt defterinizde görüntüleri için Azure Kubernetes hizmetindeki bir Azure Active Directory Hizmet sorumlusu kullanarak erişmeyi öğrenin.
services: container-service
author: dlepow
ms.service: container-service
ms.topic: article
ms.date: 08/08/2018
ms.author: danlep
ms.openlocfilehash: 1d7e130d619f580aeb82939e19ea5abf680ff039
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61333625"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Azure Container Registry'den Azure Kubernetes hizmeti ile kimlik doğrulaması

Azure Kubernetes Service (AKS) ile Azure Container Registry (ACR) kullanırken, bir kimlik doğrulama mekanizması kurulması gerekir. Bu makalede, bu iki Azure Hizmetleri arasındaki kimlik doğrulaması için önerilen yapılandırmaları açıklanmaktadır.

Bu makalede, bir AKS kümesi oluşturduğunuz ve kümeyle erişebilir varsayılır `kubectl` komut satırı istemcisi. 

## <a name="grant-aks-access-to-acr"></a>ACR verme AKS erişimi

Bir AKS kümesi oluşturduğunuzda, Azure da bir hizmet sorumlusu diğer Azure kaynakları ile küme çalışabilirlik desteklemek için oluşturur. Bu otomatik olarak oluşturulan hizmet sorumlusu, bir ACR kayıt defteri ile kimlik doğrulaması için kullanabilirsiniz. Bunu yapmak için Azure AD'yi oluşturmanız gerekir [rol ataması](../role-based-access-control/overview.md#role-assignments) , kapsayıcı kayıt defterine kümenin hizmet sorumlusu erişimi verir.

Azure container registry AKS tarafından oluşturulan hizmet sorumlusu çekme erişimi vermek için aşağıdaki betiği kullanın. Değiştirme `AKS_*` ve `ACR_*` betiği çalıştırmadan önce ortam değişkenleri.

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
az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

## <a name="access-with-kubernetes-secret"></a>Kubernetes gizli ile erişim

Bazı durumlarda, ACR erişim verme otomatik olarak oluşturulan AKS hizmet sorumlusuna gerekli rol atamak mümkün olmayabilir. Örneğin, kuruluşunuzun güvenlik modeli nedeniyle, yeterli izinlere AKS tarafından oluşturulan hizmet sorumlusuna bir rol atamak için Azure Active Directory kiracınızdaki olmayabilir. Bir hizmet sorumlusuna bir rol atayarak yazma izni Azure AD kiracınız için Azure AD hesabınızın gerektirir. İzni yoksa, yeni bir hizmet sorumlusu oluşturun, ardından Kubernetes görüntü çekme gizli kullanarak kapsayıcı kayıt defterine erişim izni.

(Kubernetes görüntü çekme gizli için kimlik bilgilerini kullanacaksınız) yeni bir hizmet sorumlusu oluşturmak için aşağıdaki betiği kullanın. Değiştirme `ACR_NAME` betiği çalıştırmadan önce ortamınız için değişken.

```bash
#!/bin/bash

ACR_NAME=myacrinstance
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate the ACR login server and resource id.
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create acrpull role assignment with a scope of the ACR resource.
SP_PASSWD=$(az ad sp create-for-rbac --name http://$SERVICE_PRINCIPAL_NAME --role acrpull --scopes $ACR_REGISTRY_ID --query password --output tsv)

# Get the service principal client id.
CLIENT_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output used when creating Kubernetes secret.
echo "Service principal ID: $CLIENT_ID"
echo "Service principal password: $SP_PASSWD"
```

İçinde bir Kubernetes hizmet sorumlusunun kimlik bilgileri artık depolayabilirsiniz [görüntü çekme gizli][image-pull-secret], AKS kümenizin kapsayıcı çalıştırırken başvuran.

Aşağıdaki **kubectl** Kubernetes gizli dizi oluşturmak için komutu. Değiştirin `<acr-login-server>` (biçim "acrname.azurecr.io" olduğu) bir Azure kapsayıcı kayıt defterinizin tam ada sahip. Değiştirin `<service-principal-ID>` ve `<service-principal-password>` önceki betiği çalıştırarak aldığınız değerlerle. Değiştirin `<email-address>` herhangi bir iyi biçimlendirilmiş bir e-posta adresine sahip.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

Adını belirterek Kubernetes gizli pod dağıtımlarda artık kullanabilirsiniz (Bu durumda, "acr-auth") içinde `imagePullSecrets` parametresi:

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
