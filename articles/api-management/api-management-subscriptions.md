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
ms.openlocfilehash: 6f577530c42952c6340a15110bcd37383a5fca57
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59526597"
---
# <a name="subscriptions-in-azure-api-management"></a>Azure API Management abonelikler

Abonelik, Azure API Management'ta önemli bir kavramdır. Bunlar, en yaygın yolu bir API Management örneğinizden yayımlanan API'leri erişmek API Tüketiciler için hedeflenmiştir. Bu makalede, kavram genel bir bakış sağlar.

## <a name="what-are-subscriptions"></a>Abonelik nedir?

API Management aracılığıyla API'leri yayımladığınızda, bu Abonelik anahtarları kolay ve bu API'lere güvenli erişim için ortak kullanmaktır. Yayımlanan API'leri kullanmak isteyen geliştiriciler bu API'lere giden çağrıların zaman HTTP isteklerini bir geçerli abonelik anahtarı eklemeniz gerekir. Aksi takdirde, çağrılar hemen API Management ağ geçidi tarafından reddedilir. Bunlar arka uç hizmetlerine iletilen değildir.

API erişimi için bir abonelik anahtarı almak için bir abonelik gereklidir. Abonelik anahtarları çifti için temelde adlı bir kapsayıcı bir aboneliktir. Yayımlanan API'leri kullanmak isteyen geliştiricilere abonelikleri alabilirsiniz. Ve API yayımcılarının onayını ihtiyacınız yoktur. API yayımcıları, abonelikleri doğrudan API tüketicilerini de oluşturabilirsiniz.

> [!TIP]
> API Management API'leri, aşağıdaki örnekler de dahil olmak üzere erişim güvenliğini sağlamak için başka mekanizmalar da destekler:
> - [OAuth2.0](api-management-howto-protect-backend-with-aad.md)
> - [İstemci sertifikaları](api-management-howto-mutual-certificates-for-clients.md)
> - [IP beyaz listesi](https://docs.microsoft.com/azure/api-management/api-management-access-restriction-policies#RestrictCallerIPs)

## <a name="scope-of-subscriptions"></a>Abonelik kapsamı

Abonelikleri çeşitli kapsamlar ile ilişkilendirilebilir: Ürün, tüm API veya tek tek bir API.

### <a name="subscriptions-for-a-product"></a>Bir ürün için abonelik

Geleneksel olarak, API Management abonelikler her zaman tek bir ilişkili [bir API ürününe](api-management-terminology.md) kapsam. Geliştiriciler Geliştirici portalında ürünlerin listesini bulundu. Ardından ürünler için abonelik isteklerini kullanmak istedikleri göndermeniz. Otomatik olarak veya API yayımcılarının bir abonelik isteği onaylandıktan sonra Geliştirici anahtarları içinde üründeki tüm API'ler erişmek için kullanabilirsiniz.

![Ürün abonelikler](./media/api-management-subscriptions/product-subscription.png)

> [!TIP]
> Bazı senaryolarda, API yayımcılarının abonelikleri gereksinimi olmadan genel API ürünü yayımlamak isteyebilirsiniz. Seçimini kaldırabilirsiniz **aboneliği gerektiren** seçeneğini **ayarları** Azure portalında ürün sayfası. Sonuç olarak, tüm API'leri ürün altında bir API anahtarı erişilebilir.

### <a name="subscriptions-for-all-apis-or-an-individual-api"></a>Tüm API'ler veya tek tek bir API için abonelikler

Ne zaman kullanıma sunduk [tüketim](https://aka.ms/apimconsumptionblog) katman API yönetimi, anahtar yönetimini kolaylaştırmak için bazı değişiklikler yaptık:
- İlk olarak, iki daha fazla abonelik kapsamları ekledik: tüm API'leri ve tek bir API. Abonelik kapsamı artık bir API ürününe sınırlıdır. Artık, bir API ya da tüm API'leri, API Management örneği içinde Ürün oluşturma ve API'leri ilk eklemek zorunda kalmadan erişim anahtarları oluşturmak mümkündür. Ayrıca, her API Management örneği, artık bir sabit, tüm API aboneliğiyle içermektedir. Bu abonelik daha kolay ve daha basit bir test ve API'leri test konsolda hata ayıklamayı kolaylaştırır.

- İkinci olarak, API Management'ı şimdi sağlar **tek başına** abonelikler. Abonelikleri, geliştirici hesabıyla ilişkilendirilmesi için artık gerekli değildir. Bu özellik, ne zaman bir abonelik birkaç geliştiriciler veya ekipler paylaşma gibi senaryolarda yararlıdır.

- Son olarak, API yayımcılarının artık [abonelikleri oluşturma](api-management-howto-create-subscriptions.md) doğrudan Azure Portalı'nda:

    ![Esnek abonelikler](./media/api-management-subscriptions/flexible-subscription.png)

## <a name="next-steps"></a>Sonraki adımlar
API yönetimi hakkında daha fazla bilgi alın:

+ Diğer bilgi [kavramları](api-management-terminology.md) API Management.
+ İzleyin bizim [öğreticiler](import-and-publish.md) API yönetimi hakkında daha fazla bilgi edinmek için.
+ Denetleme bizim [SSS sayfasını](api-management-faq.md) sık sorulan sorular için.