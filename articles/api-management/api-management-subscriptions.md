---
title: Azure API Management abonelikleri | Microsoft Docs
description: Azure API Management'ta abonelik kavramı hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 45a65c7a94f3e6917b2882f27c9ad7d764ef97d7
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621861"
---
# <a name="subscriptions-in-azure-api-management"></a>Azure API Management abonelikler

Abonelikleri olan Azure API Management (APIM) önemli bir kavramdır. Bu API tüketicilerini API'leri bir APIM örneği yayımlanan erişim sağlamak en yaygın yoludur. Bu makalede, kavram genel bir bakış sağlar.

## <a name="what-is-subscriptions"></a>Abonelik nedir

API'ler aracılığıyla APIM yayımlarken bu API'lerine erişimi güvenli hale getirmek için kolay ve en yaygın yolu abonelik anahtarlarını kullanmaktır. Diğer bir deyişle, yayımlanan API'leri kullanmak isteyen geliştiricilere geçerli bir abonelik anahtarı HTTP isteklerini bu API'lere çağrı yaparken içermelidir. Aksi takdirde, çağrılar hemen APIM ağ geçidi tarafından reddedilir ve arka uç hizmetlerine iletilmez.

API erişimi için bir abonelik anahtarı almak için bir abonelik gereklidir. Abonelik anahtarları çifti için temelde adlı bir kapsayıcı aboneliktir. Abonelikler ile veya olmadan onay API yayımcılar tarafından yayımlanan API'leri isteyen geliştiriciler tarafından alınabilir. API yayımcıları abonelikleri doğrudan API tüketicilerini adına oluşturabilirsiniz.

> [!TIP]
> APIM da API'leri dahil olmak üzere erişim güvenliğini sağlamak için başka mekanizmalar destekler [OAuth2.0](api-management-howto-protect-backend-with-aad.md), [istemci sertifikaları](api-management-howto-mutual-certificates-for-clients.md), ve [IP beyaz listesi](https://docs.microsoft.com/azure/api-management/api-management-access-restriction-policies#RestrictCallerIPs)

## <a name="scope-of-subscriptions"></a>Abonelik kapsamı

Abonelikleri çeşitli kapsamlar ile ilişkilendirilebilir: Ürün, tüm API veya tek tek bir API.

### <a name="subscriptions-for-a-product"></a>Bir ürün için abonelik

Geleneksel olarak, APIM Aboneliklerde her zaman tek bir ilişkili [bir API ürününe](api-management-terminology.md) kapsam. Geliştiriciler Geliştirici portalında ürünlerin listesini bulmak ve kullanmak istediğiniz ürünleri için abonelik isteklerini göndermek. (Otomatik olarak veya API yayımcılar tarafından) bir abonelik isteği onaylandıktan sonra Geliştirici anahtarları üründeki tüm API'ler erişmek için kullanabilirsiniz.

![Ürün abonelikler](./media/api-management-subscriptions/product-subscription.png)

> [!TIP]
> Bazı senaryolarda, API yayımcılarının abonelikleri gereksinimi olmadan genel API ürünü yayımlamak isteyebilirsiniz. İşaretini kaldırın yapabilirler **aboneliği gerektiren** seçeneğini **ayarları** Azure portalında ürün sayfası. Sonuç olarak, tüm API'leri ürün altında bir API anahtarı erişilebilir.

### <a name="subscriptions-for-all-apis-or-an-individual-api"></a>Tüm API'ler veya tek tek bir API için abonelikler

> [!NOTE]
> Şu anda bu özellik yalnızca API Management tüketimini katmanında kullanılabilir.

Ne zaman kullanıma sunduk [tüketim](https://aka.ms/apimconsumptionblog) katmanı APIM anahtar yönetimini kolaylaştırmak için bazı değişiklikler yaptık. İlk olarak, iki daha fazla abonelik kapsamları - ekledik tüm API'leri ve tek bir API. Abonelik kapsamı artık bir API ürününe sınırlıdır. Artık, bir API (ya da tüm API'leri bir APIM örneği içinde), gerek olmadan Ürün oluşturma ve API'ler için önce eklemek için erişim anahtarları oluşturmak mümkündür. Ayrıca, her APIM örneği artık daha kolay ve daha basit bir test ve API'leri Test konsolda hata ayıklamayı kolaylaştırır. bir sabit, tüm API aboneliğiyle birlikte gelir.

İkinci olarak, APIM artık "tek başına" aboneliklere izin verir. Abonelikleri, geliştirici hesabıyla ilişkilendirilmesi için artık gerekli değildir. Bu, bir abonelik geliştiriciler veya ekipler tarafından ne zaman paylaşılan gibi senaryolarda kullanışlıdır.

Son olarak, API yayımcılarının artık [abonelikleri oluşturma](api-management-howto-create-subscriptions.md) doğrudan Azure Portalı'nda:

![Esnek abonelikler](./media/api-management-subscriptions/flexible-subscription.png)

## <a name="next-steps"></a>Sonraki adımlar
API Management hakkında daha fazla bilgi için:

+ Diğer bilgi [kavramları](api-management-terminology.md) API Management
+ İzleyin bizim [öğreticiler](import-and-publish.md) API yönetimi hakkında daha fazla bilgi edinmek için
+ Denetleme bizim [SSS sayfasını](api-management-faq.md) sık sorulan sorular için