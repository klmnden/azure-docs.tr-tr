---
title: "Gelişmiş istek azaltma ile Azure API Yönetimi"
description: "Oluşturma ve esnek kota ve ilkeleri Azure API Management ile hız sınırı uygulama öğrenin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2018
ms.author: apimpm
ms.openlocfilehash: 427660be92d3caf4c381cec65f49adce9808e50a
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="advanced-request-throttling-with-azure-api-management"></a>Gelişmiş istek azaltma ile Azure API Yönetimi
Gelen istek kısıtlama erişebildiklerinden, Azure API Management anahtar bir roldür. Da istekleri ya da aktarılan toplam istekleri/veri oranını denetleyerek, API Management kendi API kötüye korumak ve farklı API ürün katmanları için değer oluşturmak API sağlayıcıları sağlar.

## <a name="product-based-throttling"></a>Ürün tabanlı azaltma
Bugüne kadar hızı azaltma özellikleri olmaya sınırlı silinmiş belirli ürün abonelik (aslında bir anahtarı) için kapsamlı, Azure portalında tanımlı. Kendi API kullanmak için açmış geliştiriciler sınırları uygulamak API sağlayıcısı için faydalıdır, ancak bu, örneğin, tek tek son kullanıcılar API azaltma korumaz. Bu, tüm kota kullanabilir ve ardından diğer müşteriler geliştiricinin uygulamayı kullanabilmek için engellemek için kullanıcı Geliştirici uygulamasının tek için mümkün olur. Ayrıca, bir istek hacmi yüksek oluşturabilir çeşitli müşteriler zaman zaman kullanıcılara erişimi sınırlayabilir.

## <a name="custom-key-based-throttling"></a>Özel anahtar tabanlı azaltma
Yeni [anahtar tarafından hızı sınırı](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) ve [kota anahtarı](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) ilkeleri, akış denetimi önemli ölçüde daha esnek bir çözüme sağlar. Bu yeni ilkeleri, trafiği kullanımı izlemek için kullanılan anahtarları tanımlamak için ifadeleri tanımlamanıza olanak sağlar. Bu çalışma biçimini kolay bir örnek gösterilmiştir. 

## <a name="ip-address-throttling"></a>IP adresi azaltma
Aşağıdaki ilkeleri tek istemci IP adresi yalnızca 10 çağrıları 1.000.000 çağrıları toplam ve bant genişliği aylık 10.000 kilobayt dakikada kısıtlayın. 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

Internet üzerindeki tüm istemcilere benzersiz bir IP adresi kullandıysanız, bu kullanıcı tarafından kullanımı sınırlama, etkili bir yol olabilir. Ancak, birden çok kullanıcı bunları Internet'e bir NAT cihazı üzerinden erişim nedeniyle tek bir ortak IP adresi paylaşımı olacak oldukça olasıdır. Buna karşın, kimliği doğrulanmamış erişime izin API'leri için `IpAddress` en iyi seçenek olabilir.

## <a name="user-identity-throttling"></a>Kullanıcı Kimliği azaltma
Son kullanıcının kimliği doğrulandıktan sonra azaltma anahtarı tanıtan bir, bilgilere dayalı olarak oluşturulabilen kullanıcı.

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

Bu örnekte biz Authorization Üstbilgisi ayıklamak, onu şöyle dönüştürün `JWT` nesne ve kullanıcıyı tanımlamak ve anahtar sınırlama oranı kullanmak için belirteç konusunu kullanın. Kullanıcı Kimliği depolanıyorsa `JWT` diğer birini sonra değer onun yerine kullanılabilir talep gibi.

## <a name="combined-policies"></a>Birleşik ilkeleri
Yeni kısıtlama ilkeleri, mevcut kısıtlama ilkeleri daha fazla denetim sağlar ancak da hala her iki özelliği birleştirme değer yoktur. Ürün abonelik anahtarı tarafından azaltma ([abonelik tarafından çağrı hızını sınırla](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) ve [abonelik tarafından kullanım kotası ayarla](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) bir API kullanım düzeylerine göre şarj monetizing etkinleştirmek için harika bir yoludur. Kullanıcı tarafından kısıtlama yapamamasına daha hassas denetim tamamlayıcı ve bir kullanıcının davranışına başka bir deneyim düşürmesini engeller. 

## <a name="client-driven-throttling"></a>Azaltma yönetilen istemci
Azaltma anahtarı kullanarak tanımlandığında bir [ilke ifadesi](https://msdn.microsoft.com/library/azure/dn910913.aspx), azaltma nasıl kapsamlıdır seçme API sağlayıcı ise. Ancak, bir geliştirici nasıl bunlar kendi hız sınırı denetlemek isteyebilirsiniz müşteriler. Bu API sağlayıcı tarafından bir özel üst bilgi sunarak Geliştirici istemci uygulamasının API anahtarı iletişim kurmasına izin vermek için etkinleştirilmiş.

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

Bu, nasıl bunlar anahtar sınırlama oranı oluşturmak istediğinizi seçmek Geliştirici istemci uygulaması sağlar. Biraz resimleri ile bir istemci Geliştirici kullanıcılara anahtarları kümelerini ayırma ve anahtar kullanımı döndürme kendi oranı katmanları oluşturabilirsiniz.

## <a name="summary"></a>Özet
Azure API Management oranı ve hem korumak ve API hizmetinize değeri eklemek için azaltma teklif sağlar. Özel ölçüm kuralları ile yeni kısıtlama ilkeleri, müşterilerin daha da iyi uygulamalar oluşturmanıza olanak tanımak için bu ilkeleri üzerinde daha hassas denetim sağlar. Bu makaledeki örnekler, istemci IP adresleri, kullanıcı kimliği ve istemci oluşturulan değerleri anahtarlarla sınırlama üretim hızına göre bu yeni ilkeleri kullanımını göstermektedir. Ancak, diğer birçok kullanılabilecek kullanıcı aracısı, URL yolu parçaları, ileti boyutu gibi iletinin bölümleri vardır.

## <a name="next-steps"></a>Sonraki adımlar
Lütfen bize Geri bildiriminizi Disqus iş parçacığı için bu konunun verin. Senaryolarınız mantıksal bir seçenek olan diğer olası anahtar değerleri hakkında dinlemek harika olacaktır.

## <a name="watch-a-video-overview-of-these-policies"></a>Bu ilkelerin video genel izleyin
Daha fazla bilgi için [anahtar tarafından hızı sınırı](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) ve [kota anahtarı](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) bu makalede ele alınan ilkeleri Lütfen aşağıdaki videoyu izleyin.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 

