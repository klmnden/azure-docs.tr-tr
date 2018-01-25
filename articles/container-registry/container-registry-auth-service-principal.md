---
title: "Hizmet asıl adı ile Azure kapsayıcı kayıt defteri kimlik doğrulaması"
description: "Bir Azure Active Directory hizmet asıl kullanarak özel kapsayıcı kaydınız görüntülerinde erişim sağlamak öğrenin."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/24/2018
ms.author: marsma
ms.openlocfilehash: 97036ecabceb12b87b76c6ecb7e521157cbef827
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/25/2018
---
# <a name="azure-container-registry-authentication-with-service-principals"></a>Hizmet asıl adı ile Azure kapsayıcı kayıt defteri kimlik doğrulaması

Bir Azure Active Directory (Azure AD) hizmet asıl kapsayıcı görüntü sağlamak için kullanabileceğiniz `docker push` ve `pull` kapsayıcı kayıt defterine erişim. Bir hizmet sorumlusu kullanarak "gözetimsiz" Hizmetler ve uygulamalar için erişim sağlayabilir.

## <a name="what-is-a-service-principal"></a>Bir hizmet sorumlusu nedir?

Azure AD *hizmet sorumluları* aboneliğinizi içinde Azure kaynaklarına erişimi sağlar. Bir hizmeti "hizmet" herhangi bir uygulama, hizmet veya kaynaklarına ihtiyacı platform olduğu asıl bir hizmet için bir kullanıcı kimliği olarak düşünebilirsiniz. Yalnızca belirttiğiniz kaynakları için kapsamlı erişim haklarına sahip bir hizmet sorumlusu yapılandırabilirsiniz. Ardından, uygulama veya hizmet bu kaynaklara erişmek için hizmet sorumlusuna ilişkin kimlik bilgilerini kullanmak için yapılandırabilirsiniz.

Azure kapsayıcı kayıt defteri bağlamında, Azure AD hizmet sorumlusu çekme, anında iletme ve çekme veya sahibinin izinleriyle Azure özel, Docker kayıt defterine oluşturabilirsiniz.

## <a name="why-use-a-service-principal"></a>Bir hizmet sorumlusu neden kullanılır?

Bir Azure AD hizmet sorumlusu kullanarak özel kapsayıcı kaydınız kapsamlı erişim sağlayabilir. Her uygulamalar veya hizmetler, kayıt defterine özel erişim haklarına sahip her biri için farklı hizmet asıl adı oluşturabilirsiniz. Ve hizmetler ve uygulamalar arasında kimlik bilgilerinin paylaşımı önlemek için kimlik bilgileri döndürün veya yalnızca hizmet sorumlusu (ve dolayısıyla uygulama) için seçtiğiniz erişimi iptal.

Örneğin, web uygulamanızı görüntüsüyle sağlayan bir hizmet sorumlusu kullanabilirsiniz `pull` derleme sisteminiz ikisi ile sağlayan bir hizmet sorumlusu kullanabilirsiniz, ancak yalnızca, erişimi `push` ve `pull` erişim. Uygulamanızın geliştirilmesi ellerini değişirse, yapı sistem etkilemeden, hizmet ilkesi kimlik bilgileri döndürebilirsiniz.

## <a name="when-to-use-a-service-principal"></a>Bir hizmet sorumlusu kullanma zamanı

Bir hizmet sorumlusu kayıt defteri erişim sağlamak için kullanmalısınız **gözetimsiz senaryoları**. Diğer bir deyişle, herhangi bir uygulama, hizmet veya İtme veya kapsayıcı çekme gerekir komut dosyası otomatik olarak veya başka bir katılımsız şekilde görüntüler.

Tek tek erişim ne zaman, el ile bir kapsayıcı görüntüsü geliştirme istasyonunuza çekme gibi bir kayıt defteri için bunun yerine kendi kullanmanız [Azure AD kimlik](container-registry-authentication.md#individual-login-with-azure-ad) kayıt defteri erişimi için (örneğin, [az acr oturum açma][az-acr-login]).

[!INCLUDE [container-registry-service-principal](../../includes/container-registry-service-principal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Kapsayıcı kayıt defterine erişim izni hizmet sorumlusuna sahip olduktan sonra uygulamaları ve Hizmetleri kayıt defteri etkileşim için kimlik bilgilerini kullanabilirsiniz.

Hizmet asıl kimlik bilgilerini kullanmak üzere ayrı ayrı uygulamaları yapılandırma bu makalenin kapsamı dışında olsa da, bazı belirli hizmetlere ve burada platformlar için yönergeler bulabilirsiniz:

* [Azure kapsayıcı kayıt defterinden Azure kapsayıcı hizmeti (AKS) ile kimlik doğrulaması](container-registry-auth-aks.md)
* [Azure kapsayıcı kayıt defterinden Azure kapsayıcı örnekleri (ACI) ile kimlik doğrulaması](container-registry-auth-aci.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-acr-login]: /cli/azure/acr#az_acr_login