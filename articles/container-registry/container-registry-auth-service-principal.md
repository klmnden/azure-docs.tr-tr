---
title: Azure Container Registry kimlik doğrulama ile hizmet sorumluları
description: Özel kapsayıcı kayıt defterinizde görüntüleri bir Azure Active Directory Hizmet sorumlusu kullanarak erişmeyi öğrenin.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 04/23/2018
ms.author: danlep
ms.openlocfilehash: 30f0eb04b4b7d07785854e3079bc6656889edec6
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48854499"
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Azure Container Registry kimlik doğrulama ile hizmet sorumluları

Kapsayıcı görüntüsünü sağlamak için bir Azure Active Directory (Azure AD) hizmet sorumlusu kullanabilirsiniz `docker push` ve `pull` , kapsayıcı kayıt defterine erişim. Bir hizmet sorumlusu kullanarak "gözetimsiz" Hizmetler ve uygulamalar için erişim sağlayabilir.

## <a name="what-is-a-service-principal"></a>Bir hizmet sorumlusu nedir?

Azure AD *hizmet sorumluları* , aboneliğiniz kapsamındaki Azure kaynaklarına erişim sağlama. Bir hizmeti "hizmet" herhangi bir uygulama, hizmet veya kaynaklara erişmesi platformu olduğu asıl kullanıcı kimliği için bir hizmet olarak düşünebilirsiniz. Yalnızca belirttiğiniz kaynakları için kapsamlı erişim haklarına sahip bir hizmet sorumlusu yapılandırabilirsiniz. Ardından, uygulamanızın veya hizmetinizin bu kaynakları erişmek için hizmet sorumlusunun kimlik bilgilerini kullanmak için yapılandırabilirsiniz.

Azure Container Registry bağlamında, bir Azure AD hizmet sorumlusu çekme, gönderme ve çekme veya sahip izinleri ile azure'da özel Docker kayıt oluşturabilir.

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

Kapsayıcı kayıt defterinizde erişim verdiyseniz bir hizmet sorumlusu oluşturduktan sonra uygulama ve hizmetlerinizin kayıt defteri etkileşimi için kendi kimlik bilgilerini kullanabilirsiniz.

Hizmet sorumlusu kimlik bilgileri kullanmak üzere bireysel uygulamaları yapılandırma bu makalenin kapsamı dışında olsa da, bazı belirli hizmetlere ve burada platformları için yönergeler bulabilirsiniz:

* [Azure Container Registry'den Azure Kubernetes Service'i (AKS) ile kimlik doğrulaması](container-registry-auth-aks.md)
* [Azure Container Registry'den Azure Container Instances (ACI) ile kimlik doğrulaması](container-registry-auth-aci.md)

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az-acr-login