---
title: Azure Container Registry'den Azure Container Instances ile kimlik doğrulaması
description: Özel kapsayıcı kayıt defterinizde bulunan görüntü Azure Container Instances ' bir Azure Active Directory Hizmet sorumlusu kullanarak erişmeyi öğrenin.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 04/23/2018
ms.author: danlep
ms.openlocfilehash: 8a2d19a09233e510055e147fa1cf95dd4471768b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61333585"
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Azure Container Registry'den Azure Container Instances ile kimlik doğrulaması

Azure Container Registry, özel kapsayıcı kayıt defterleri için erişim sağlamak için bir Azure Active Directory (Azure AD) hizmet sorumlusu kullanabilirsiniz.

Bu makalede, oluşturmak ve bir Azure AD hizmet sorumlusu ile yapılandırmak bilgi *çekme* kayıt izinleri. Sonra Azure Container Instances'a (özel kayıt defterinizdeki, kendi görüntü çeken hizmet sorumlusunu kullanarak kimlik doğrulaması için ACI) bir kapsayıcı başlatın.

## <a name="when-to-use-a-service-principal"></a>Bir hizmet sorumlusu kullanma zamanı

ACI kimlik doğrulaması için bir hizmet sorumlusu kullanmalısınız **gözetimsiz senaryoları**kapsayıcı örnekleri otomatik olarak veya başka bir katılımsız şekilde oluşturmak, uygulamaları veya hizmetleri olduğu gibi.

Örneğin, gecelik çalıştırır ve oluşturan otomatik bir betik varsa bir [görev tabanlı bir kapsayıcı örneği](../container-instances/container-instances-restart-policy.md) bazı verileri işlemek için bunu bir hizmet sorumlusu çoğaltılmadığı izinlerle kayıt defterinde kimlik doğrulaması için kullanabilirsiniz. Ardından, hizmet sorumlusunun kimlik bilgilerini döndürme veya diğer hizmetler ve uygulamaları etkilemeden, erişimi tamamen iptal.

Hizmet sorumluları olmalıdır ne zaman kullanılan kayıt defteri [yönetici kullanıcı](container-registry-authentication.md#admin-account) devre dışı bırakıldı.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Hizmet sorumlusunu kullanarak kimlik doğrulaması

Bir kapsayıcıda bir hizmet sorumlusu kullanarak Azure Container Instances'ı başlatmak için kendi kimliği belirtin. `--registry-username`ve bunun parolasını `--registry-password`.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image mycontainerregistry.azurecr.io/myimage:v1 \
    --registry-login-server mycontainerregistry.azurecr.io \
    --registry-username <service-principal-ID> \
    --registry-password <service-principal-password>
```

## <a name="sample-scripts"></a>Örnek komut dosyaları

Azure PowerShell için iyi bir sürüm olarak, github'da Azure CLI için önceki örnek betikler bulabilirsiniz:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Sonraki adımlar

ACR ile hizmet sorumluları ile çalışma hakkında ek ayrıntılar aşağıdaki makaleleri içerir:

* [Azure Container Registry kimlik doğrulama ile hizmet sorumluları](container-registry-auth-service-principal.md)
* [Azure Container Registry'den Azure Kubernetes Service'i (AKS) ile kimlik doğrulaması](container-registry-auth-aks.md)

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
