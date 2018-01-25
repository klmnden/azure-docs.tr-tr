---
title: "Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri ile kimlik doğrulaması"
description: "Özel kapsayıcı kaydınız görüntülere Azure kapsayıcı örneklerden bir Azure Active Directory hizmet asıl kullanarak erişilmesine öğrenin."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/24/2018
ms.author: marsma
ms.openlocfilehash: 00d9632a5d0c42eceee1b412f8963bbadbea651f
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
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

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler hizmet asıl adı ve ACR ile çalışma hakkında ek ayrıntıları içerir:

* [Hizmet asıl adı ile Azure kapsayıcı kayıt defteri kimlik doğrulaması](container-registry-auth-service-principal.md)
* [Azure kapsayıcı kayıt defterinden Azure kapsayıcı hizmeti (AKS) ile kimlik doğrulaması](container-registry-auth-aks.md)

<!-- IMAGES -->

<!-- LINKS - External -->

<!-- LINKS - Internal -->
