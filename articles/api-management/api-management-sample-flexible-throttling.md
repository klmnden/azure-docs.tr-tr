---
title: Gelişmiş istek azaltma ile Azure API Yönetimi
description: Oluşturma ve esnek kota ve ilkeleri Azure API Management ile hız sınırı uygulama hakkında bilgi edinin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2018
ms.author: apimpm
ms.openlocfilehash: 22c3987121e2ab3479274c89c359c679f5f1135e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61087150"
---
# <a name="advanced-request-throttling-with-azure-api-management"></a>Gelişmiş istek azaltma ile Azure API Yönetimi
Gelen istek kısıtlama için Azure API Management'ın önemli bir rol var. Hem istekler veya aktarılan toplam istekler/veri oranı denetleyerek, Azure API yönetimi, API'leri kötüye korumak ve farklı API ürün katmanları için değer oluşturmak API sağlayıcıları sağlar.

## <a name="product-based-throttling"></a>Ürün tabanlı azaltma
Bugüne kadar hızı azaltma özellikleri Azure Portalı'nda tanımlanan belirli bir ürün aboneliği için kapsamı belirlenmiş için sınırlı olmuştur. Bu API'leri kullanmak için kaydolan geliştiriciler sınırları uygulamak API sağlayıcısı için yararlıdır, ancak bu, örneğin, tek tek son kullanıcılar API'si azaltma kullanışlı değildir. Bu, tüm kota kullanmak ve ardından diğer müşterilerin Geliştirici uygulama kullanmanız mümkün olmasını engellemek için kullanıcı uygulamasının geliştiricinin tek için mümkündür. Ayrıca, yüksek hacimli istekleriniz üretebilir çeşitli müşteriler dönemsel kullanıcılar için erişimi sınırlayabilir.

## <a name="custom-key-based-throttling"></a>Özel anahtar tabanlı azaltma
Yeni [anahtarı tarafından oran sınırı](/azure/api-management/api-management-access-restriction-policies#LimitCallRateByKey) ve [quota-by-key](/azure/api-management/api-management-access-restriction-policies#SetUsageQuotaByKey) ilkeleri, trafiği denetlemek için daha esnek bir çözüm sağlar. Bu yeni ilkeleri trafik kullanımı izlemek için kullanılan anahtarları tanımlamak için ifadeleri tanımlamanızı sağlar. Bu çalışma biçimini kolay içeren bir örnek gösterilmiştir. 

## <a name="ip-address-throttling"></a>IP adresi azaltma
Aşağıdaki ilkeleri tek istemci IP adresi yalnızca 10 çağrıları toplam 1.000.000 çağrıları ve bant genişliği 10.000 kilobayt dakikada kısıtlayın. 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

Internet üzerindeki tüm istemcilere benzersiz bir IP adresi kullandıysanız, bu kullanıcı tarafından kullanımını sınırlamak için etkili bir yol olabilir. Ancak, birden çok kullanıcı tek bir genel IP adresi bunları bir NAT cihazı üzerinden İnternet'e erişim nedeniyle paylaşıyor olasıdır. Bu, kimliği doğrulanmamış erişime izin API'leri için rağmen `IpAddress` en iyi seçenek olabilir.

## <a name="user-identity-throttling"></a>Kullanıcı kimlik azaltma
Son kullanıcı kimlik doğrulaması gerekiyorsa, azaltma anahtar kullanıcının benzersiz olarak tanımlayan bilgilere göre oluşturulabilir.

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

Bu örnekte, yetkilendirme üst Ayıkla, onu şöyle dönüştürün gösterilmektedir `JWT` nesne ve kullanıcıyı tanımlamak ve anahtar sınırlama oranı kullanan için belirteç konusunu kullanın. Kullanıcı kimliği içinde depolanıyorsa `JWT` diğer taleplerinden biri daha sonra bu değeri onun yerine kullanılabilir.

## <a name="combined-policies"></a>Birleşik ilkeleri
Yeni kısıtlama ilkeleri mevcut kısıtlama ilkeleri daha fazla denetim sağlasa da yine de her iki özelliklerini birleştirmektir değer yoktur. Ürün abonelik anahtarı tarafından azaltma ([abonelik tarafından çağrı hızını sınırla](/azure/api-management/api-management-access-restriction-policies#LimitCallRate) ve [abonelik tarafından kullanım kotası ayarla](/azure/api-management/api-management-access-restriction-policies#SetUsageQuota)) bir API kullanım düzeylerine göre kurumuna markalaştırabilir etkinleştirmek için harika bir yoludur. Kullanıcı tarafından kullanımını azaltmak için daha ayrıntılı denetim tamamlayıcı ve başka bir deneyim düşürmesini gelen bir kullanıcının davranışını önler. 

## <a name="client-driven-throttling"></a>İstemci tabanlı azaltma
Kısıtlama anahtarını kullanarak tanımlandığında bir [ilke ifadesi](/azure/api-management/api-management-policy-expressions), azaltma nasıl kapsamlı seçme API sağlayıcısı kaldırılır. Ancak, bir geliştirici nasıl bunlar kendi hız sınırı denetlemek isteyebilirsiniz müşteriler. Bu API sağlayıcı tarafından bir özel üst bilgi sunarak geliştiricinin istemci uygulaması API anahtarına iletişim kurmasına izin vermek için etkinleştirilemedi.

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

Bu, nasıl bunlar anahtar sınırlama oranı oluşturmak istediğinizi seçmek geliştiricinin istemci uygulaması sağlar. İstemci geliştiriciler, kullanıcılarına anahtarları kümesi ayırma ve anahtar kullanımı döndürme kendi oranı katmanları oluşturabilir.

## <a name="summary"></a>Özet
Azure API Management, hızı ve hem koruma hem de API hizmetinizi değeri eklemek için azaltma teklif sağlar. Yeni kısıtlama ilkeleri özel ölçüm kuralları ile müşterilerinize daha iyi uygulamalar oluşturmak etkinleştirmek için bu ilkeleri üzerinde daha ayrıntılı denetim sağlar. Bu makaledeki örneklerde, istemci IP adresleri, kullanıcı kimliğini ve istemci oluşturulan değerleri anahtarlarıyla sınırlama üretim hızı tarafından bu yeni ilkeleri kullanımını gösterir. Ancak, diğer birçok kullanılabilecek kullanıcı aracısı, URL yolu parçaları, ileti boyutu gibi ileti bölümü vardır.

## <a name="next-steps"></a>Sonraki adımlar
Lütfen yönelik bu konuda Disqus iş parçacığında, geri bildirimde bulunun. Senaryolarınız mantıksal bir seçenek olan diğer olası anahtar değerleri öğrenmek harika olacaktır.

