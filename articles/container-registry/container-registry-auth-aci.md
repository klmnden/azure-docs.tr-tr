---
title: Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri ile kimlik doğrulaması
description: Özel kapsayıcı kaydınız görüntülere Azure kapsayıcı örneklerden bir Azure Active Directory hizmet asıl kullanarak erişilmesine öğrenin.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 04/23/2018
ms.author: marsma
ms.openlocfilehash: daa9c098de0c410bd4033cc62ee911631eb3b634
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33768235"
---
# <a name="authenticate-with-azure-container-registry-from-azure-container-instances"></a>Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri ile kimlik doğrulaması

Azure kapsayıcı kayıt defterinde, özel kapsayıcı kayıt defterleri erişim sağlamak için bir Azure Active Directory (Azure AD) hizmet asıl kullanabilirsiniz.

Bu makalede, oluşturmak ve bir Azure AD hizmet sorumlusu yapılandırmak bilgi *çekme* kayıt izinleri. Ardından, bir kapsayıcı içinde Azure kapsayıcı örnekleri (özel, kayıt defterinden görüntüsünü çeken kimlik doğrulaması için hizmet sorumlusunu kullanarak ACI) başlatın.

## <a name="when-to-use-a-service-principal"></a>Bir hizmet sorumlusu kullanma zamanı

İçinde ACI kimlik için bir hizmet sorumlusu kullanmalısınız **gözetimsiz senaryoları**, örn. uygulama veya hizmetlere kapsayıcı örnekleri otomatik olarak veya başka bir katılımsız şekilde oluşturun.

Örneğin, her gece çalışır ve oluşturan bir otomatik komut dosyası varsa, bir [görev tabanlı kapsayıcı örneği](../container-instances/container-instances-restart-policy.md) bazı verileri işlemek için bunu bir hizmet sorumlusu çoğaltılmadığı (Okuyucu) izinleriyle kayıt defterine kimliğini doğrulamak için kullanabilirsiniz. Ardından, hizmet sorumlusuna ilişkin kimlik bilgileri döndürün veya diğer hizmetler ve uygulamalar etkilemeden tamamen kendi erişimi iptal.

Hizmet asıl adı olmalıdır kullanılabilir kayıt defteri [yönetici kullanıcı](container-registry-authentication.md#admin-account) devre dışı bırakılır.

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="authenticate-using-the-service-principal"></a>Hizmet sorumlusunu kullanarak kimlik doğrulaması

Bir hizmet sorumlusu kullanarak Azure kapsayıcı örnekleri kapsayıcısında başlatmak için kendi Kimliğini belirtin `--registry-username`ve kendi parolasını `--registry-password`.

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

Yukarıdaki örnek betikler iyi sürümleri için Azure PowerShell olarak Azure CLI için github'da bulabilirsiniz:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler hizmet asıl adı ve ACR ile çalışma hakkında ek ayrıntıları içerir:

* [Hizmet asıl adı ile Azure kapsayıcı kayıt defteri kimlik doğrulaması](container-registry-auth-service-principal.md)
* [Azure kapsayıcı kayıt defterinden Azure Kubernetes hizmeti (AKS) ile kimlik doğrulaması](container-registry-auth-aks.md)

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
