---
title: Azure ön kapısı hizmeti - Ölçümler ve günlüğe kaydetme | Microsoft Docs
description: Bu makalede Azure ön kapısı hizmetinin desteklediği günlüklerine erişme ve farklı ölçümlerin anlamanıza yardımcı olur
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: sharadag
ms.openlocfilehash: 3097f4a1716718df5d67769e234562a234623cfe
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407037"
---
# <a name="monitoring-metrics-and-logs-for-front-door"></a>Ölçüm ve günlükleri için ön kapı izleme

Azure ön kapısı hizmetini kullanarak, aşağıdaki yollarla kaynaklarını izleyebilir:

* [Ölçümleri](#metrics): Application Gateway şu anda performans sayaçlarını görüntülemek için yedi ölçümleri sahiptir.
* [Günlükleri](#diagnostic-logging): Günlükleri, performans, erişim ve kaydedilen ya da bir kaynaktan izleme amacıyla kullanılan diğer verileri için izin verin.

## <a name="metrics"></a>Ölçümler

Ölçümler, performans sayaçları portalda görüntüleyebileceğiniz belirli Azure kaynakları için bir özelliğidir. Aşağıdaki ölçümler için ön kapı, kullanılabilir:

| Ölçüm | Ölçüm görünen adı | Birim | Boyutlar | Açıklama |
| --- | --- | --- | --- | --- |
| RequestCount | İstek Sayısı | Sayı | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Ön kapısı tarafından sunulan istemci isteklerinin sayısı.  |
| RequestSize | İstek boyutu | Bayt | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | İstekleri olarak ön kapısı istemcilerden gönderilen bayt sayısı. |
| ResponseSize | Yanıt boyutu | Bayt | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | İstemcilere ön kapısı yanıt gönderilen bayt sayısı. |
| TotalLatency | Toplam gecikme süresi | Milisaniye | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | İstemci ön kapısı son yanıt bayttan onaylanır kadar ön kapısı tarafından istemci isteği alındı, hesaplanan saat. |
| BackendRequestCount | Arka uç istek sayısı | Sayı | Httpstatus'a</br>HttpStatusGroup</br>Arka uç | Arka uçları için ön kapı gönderilen isteklerin sayısı. |
| BackendRequestLatency | Arka uç istek gecikme süresi | Milisaniye | Arka uç | Zaman zaman ön kapısı son yanıt bayt arka uçtan alınan kadar isteği arka uca ön kapısı tarafından gönderildiği hesaplanır. |
| BackendHealthPercentage | Arka uç sistem durumu yüzdesi | Yüzde | Arka uç</br>BackendPool | Başarılı sistem durumu yüzdesi arka uçları için ön kapı araştırmaları. |
| WebApplicationFirewallRequestCount | Web uygulaması güvenlik duvarı istek sayısı | Sayı | PolicyName</br>RuleName</br>Eylem | Ön kapısı uygulama katman güvenlik tarafından işlenen istemci isteklerinin sayısı. |

## <a name="activity-log"></a>Etkinlik günlükleri

Etkinlik günlükleri, ön kapısı üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlüklerini kullanarak, belirleyebilirsiniz "ne, kim ve ne zaman" işlemlerini (PUT, POST, DELETE), ön kapısı üzerinde gerçekleştirilen herhangi bir yazma için.

> [!NOTE]
> Etkinlik günlükleri, okuma (GET) işlemlerini ya da Azure portalında gerçekleştirilen veya özgün Yönetim API’leri kullanan işlemleri içermez.

Tüm Azure kaynaklarınızın Azure İzleyici günlüklerine erişmek veya etkinlik günlükleri, ön kapısı erişin. 

Etkinlik günlüklerini görüntülemek için:

1. Ön kapısı örneğinizi seçin.
2. **Etkinlik günlüğü**’ne tıklayın.

    ![etkinlik günlüğü](./media/front-door-diagnostics/activity-log.png)

3. İstenen filtre kapsamını seçin ve **Uygula**’ya tıklayın.

## <a name="diagnostic-logging"></a>Tanılama günlükleri
Tanılama günlükleri, denetim ve sorun giderme konularında önemli işlem ve hatalar hakkında zengin bilgiler sağlar. Tanılama günlükleri, etkinlik günlüklerinden farklıdır. Etkinlik günlükleri, Azure kaynaklarınız üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Tanılama günlükleri, kaynağınızın kendisi tarafından gerçekleştirilen işlemler hakkında bilgi sağlar. Daha fazla bilgi edinin [Azure İzleyici tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-overview.md). 

Tanılama günlükleri, ön kapısı yapılandırmak için:

1. APIM hizmet örneğinizi seçin.
2. **Tanılama ayarları**'na tıklayın.

    ![tanılama günlükleri](./media/front-door-diagnostics/diagnostic-log.png)

3. **Tanılamayı aç**’a tıklayın. Bir depolama hesabına ölçümlerle birlikte tanılama günlüklerini arşivleme, bunları bir olay Hub'ına akış veya bunları Azure İzleyici günlüklerine gönderin. 

Azure ön kapısı hizmeti şu anda sağlar tanılama günlükleri (saatlik olarak toplanır) tek tek API hakkında daha fazla istek her bir girdi aşağıdaki şemayı içerecek:

| Özellik  | Açıklama |
| ------------- | ------------- |
| Clientıp | İsteği gerçekleştiren istemcinin IP adresi. |
| clientPort | İsteği gerçekleştiren istemcinin IP bağlantı noktası. |
| HttpMethod | İstek tarafından kullanılan HTTP yöntemi. |
| HttpStatusCode | Proxy sunucudan döndürülen HTTP durum kodu. |
| HttpStatusDetails | İstekte tek bir durumu. Bu dize değeri anlamını bir durum başvuru tablosunda bulunabilir. |
| httpVersion | İstek veya bağlantı türü. |
| RequestBytes | HTTP istek iletisi istek üst bilgilerini ve istek gövdesi dahil olmak üzere bayt cinsinden boyutu. |
| requestUri | Alınan istek URI'si. |
| ResponseBytes | Yanıt olarak arka uç sunucu tarafından gönderilen bayt sayısı.  |
| RoutingRuleName | Eşleşen istek yönlendirme kuralı adı. |
| SecurityProtocol | Şifreleme isteği veya null tarafından kullanılan TLS/SSL protokolü sürümü. |
| timeTaken | Eylem, geçen milisaniye cinsinden süre uzunluğu. |
| UserAgent | İstemcinin kullandığı tarayıcı türü |
| TrackingReference | Ön kapısı tarafından sunulan bir isteği tanımlayan benzersiz bir başvuru dizesi istemciye ayrıca X Azure Ref başlık olarak gönderilir. Belirli bir istek için erişim günlükleri ayrıntıları aranıyor için gereklidir. |

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
