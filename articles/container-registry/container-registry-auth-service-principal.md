---
title: Bir hizmet sorumlusu ile Azure Container Registry kimlik doğrulaması
description: Bir Azure Active Directory Hizmet sorumlusu kullanarak özel kapsayıcı kayıt defterinizde görüntülerine erişim sağlar.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 12/13/2018
ms.author: danlep
ms.openlocfilehash: 5d8904b5906adbdab68989b3a5cf9c3975c23533
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61347092"
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Azure Container Registry kimlik doğrulama ile hizmet sorumluları

Kapsayıcı görüntüsünü sağlamak için bir Azure Active Directory (Azure AD) hizmet sorumlusu kullanabilirsiniz `docker push` ve `pull` , kapsayıcı kayıt defterine erişim. Bir hizmet sorumlusu kullanarak "gözetimsiz" Hizmetler ve uygulamalar için erişim sağlayabilir.

## <a name="what-is-a-service-principal"></a>Hizmet sorumlusu nedir?

Azure AD *hizmet sorumluları* , aboneliğiniz kapsamındaki Azure kaynaklarına erişim sağlama. Bir hizmeti "hizmet" herhangi bir uygulama, hizmet veya kaynaklara erişmesi platformu olduğu asıl kullanıcı kimliği için bir hizmet olarak düşünebilirsiniz. Yalnızca belirttiğiniz kaynakları için kapsamlı erişim haklarına sahip bir hizmet sorumlusu yapılandırabilirsiniz. Ardından, uygulamanızın veya hizmetinizin bu kaynakları erişmek için hizmet sorumlusunun kimlik bilgilerini kullanmak için yapılandırın.

Azure Container Registry bağlamında, bir Azure AD hizmet sorumlusu çekme, gönderme ve çekme veya diğer izinler ile azure'da özel kayıt defterinize oluşturabilirsiniz. Tam bir listesi için bkz. [Azure Container Registry rolleri ve izinleri](container-registry-roles.md).

## <a name="why-use-a-service-principal"></a>Bir hizmet sorumlusu neden kullanmalısınız?

Bir Azure AD hizmet sorumlusu kullanarak, özel kapsayıcı kayıt defterinizde kapsamlı erişim sağlayabilir. Uygulamaların veya hizmetlerin her kayıt için özel olarak uyarlanmış erişim hakları her biri için farklı hizmet sorumluları oluşturabilirsiniz. Hizmetler ve uygulamalar arasında kimlik bilgilerinin paylaşımı önlemek için kimlik bilgilerini döndürme veya yalnızca hizmet sorumlusu (ve bu nedenle uygulama) için seçtiğiniz erişimi iptal ve.

Örneğin, web uygulamanız görüntüsüyle sağlayan bir hizmet sorumlusu kullanabilir `pull` yapı sisteminizi her ikisi de sağlayan bir hizmet sorumlusu kullanabilirsiniz, ancak yalnızca erişim `push` ve `pull` erişim. Uygulamanızın geliştirilmesi uygulamalı değişirse, derleme sistemi etkilemeden, hizmet asıl kimlik bilgileri döndürebilirsiniz.

## <a name="when-to-use-a-service-principal"></a>Bir hizmet sorumlusu kullanma zamanı

Kayıt defteri erişim sağlamak için bir hizmet sorumlusu kullanmalısınız **gözetimsiz senaryoları**. Diğer bir deyişle, herhangi bir uygulama, hizmet veya anında iletme veya kapsayıcı çekin gerekir betiği otomatik olarak veya başka bir katılımsız şekilde görüntüler.

Tek tek el ile bir kapsayıcı görüntüsü geliştirme istasyonunuza çekerken gibi bir kayıt defterine erişim için bunun yerine kendi kullanmalısınız [Azure AD kimlik](container-registry-authentication.md#individual-login-with-azure-ad) kayıt defteri erişimi için (örneğin, [az acr oturum açma][az-acr-login]).

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="sample-scripts"></a>Örnek komut dosyaları

Azure PowerShell için iyi bir sürüm olarak, github'da Azure CLI için önceki örnek betikler bulabilirsiniz:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcı kayıt defterinizde erişim verdiyseniz bir hizmet sorumlusu oluşturduktan sonra uygulama ve hizmetlerinizin gözetimsiz kayıt defteri etkileşimi için kendi kimlik bilgilerini kullanabilirsiniz. Azure container registry ile hizmet sorumlusu kimlik bilgilerini doğrulanabilir herhangi bir Azure hizmeti kullanabilirsiniz. Örneklere şunlar dahildir:

* [Azure Container Registry'den Azure Kubernetes Service'i (AKS) ile kimlik doğrulaması](container-registry-auth-aks.md)
* [Azure Container Registry'den Azure Container Instances (ACI) ile kimlik doğrulaması](container-registry-auth-aci.md)

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login
