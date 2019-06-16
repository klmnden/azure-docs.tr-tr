---
title: Ölçüm ve günlükleri Azure ön kapısı hizmetinde izleme | Microsoft Docs
description: Bu makalede Azure ön kapısı hizmetinin desteklediği günlüklerine erişme ve farklı ölçümleri
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
ms.openlocfilehash: 16770ea0a320b3d9f081cc21a102ab050a6467f6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60736810"
---
# <a name="monitoring-metrics-and-logs-in-azure-front-door-service"></a>İzleme ölçümlerini ve günlüklerini Azure ön kapısı hizmetinde

Azure ön kapısı hizmetini kullanarak, aşağıdaki yollarla kaynaklarını izleyebilir:

- **Ölçümleri**. Application Gateway şu anda performans sayaçlarını görüntülemek için yedi ölçümleri sahiptir.
- **Günlükleri**. Etkinlik ve tanılama günlükleri, performans, erişim ve kaydedilen ya da bir kaynaktan izleme amacıyla kullanılan diğer verileri sağlar.

### <a name="metrics"></a>Ölçümler

Ölçümler, performans sayaçları portalda görmenize izin veren belirli Azure kaynakları için bir özelliğidir. Kullanılabilir ön kapısı ölçümler şunlardır:

| Ölçüm | Ölçüm görünen adı | Birim | Boyutlar | Açıklama |
| --- | --- | --- | --- | --- |
| RequestCount | İstek Sayısı | Sayı | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Ön kapısı tarafından sunulan istemci isteklerinin sayısı.  |
| RequestSize | İstek boyutu | Bayt | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | İstekleri olarak ön kapısı istemcilerden gönderilen bayt sayısı. |
| ResponseSize | Yanıt boyutu | Bayt | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | İstemcilere ön kapısı yanıt gönderilen bayt sayısı. |
| TotalLatency | Toplam gecikme süresi | Milisaniye | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | İstemci ön kapısı son yanıt bayttan onaylanır kadar ön kapısı tarafından alınan istemci isteği zaman hesaplanır. |
| BackendRequestCount | Arka uç istek sayısı | Count | Httpstatus'a</br>HttpStatusGroup</br>Arka uç | Arka uçları için ön kapı gönderilen isteklerin sayısı. |
| BackendRequestLatency | Arka uç istek gecikme süresi | Milisaniye | Arka uç | Zaman zaman ön kapısı son yanıt bayt arka uçtan alınan kadar isteği arka uca ön kapısı tarafından gönderildiği hesaplanır. |
| BackendHealthPercentage | Arka uç sistem durumu yüzdesi | Yüzde | Arka uç</br>BackendPool | Başarılı sistem durumu yüzdesi arka uçları için ön kapı araştırmaları. |
| WebApplicationFirewallRequestCount | Web uygulaması güvenlik duvarı istek sayısı | Count | PolicyName</br>RuleName</br>Eylem | Ön kapısı uygulama katman güvenlik tarafından işlenen istemci isteklerinin sayısı. |

## <a name="activity-log"></a>Etkinlik günlükleri

Etkinlik günlükleri ön kapısı hizmet üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Bunlar ayrıca belirlemek kim ve ne zaman tüm yazma işlemlerini (put, post veya Sil) ön kapısı hizmette yapılan.

>[!NOTE]
>Etkinlik günlükleri, okuma (get) işlemlerini dahil değildir. Ayrıca Azure portalında veya özgün yönetim API'sini kullanarak gerçekleştirdiğiniz işlemler dahil değildir.

Ön kapısı hizmetinizin veya Azure İzleyici'de, Azure kaynaklarınızın tüm günlükler erişim etkinliğini kaydeder. Etkinlik günlüklerini görüntülemek için:

1. Ön kapısı örneğinizi seçin.
2. Seçin **etkinlik günlüğü**.

    ![Etkinlik günlüğü](./media/front-door-diagnostics/activity-log.png)

3. Bir filtre kapsamı seçin ve ardından **Uygula**.

## <a name="diagnostic-logging"></a>Tanılama günlükleri
Tanılama günlükleri, işlemleri ve denetim ve sorun giderme için önemli olan hatalar hakkında zengin bilgiler sağlar. Tanılama günlükleri, etkinlik günlüklerinden farklıdır.

Etkinlik günlükleri, Azure kaynaklarında yapılan işlemleri hakkında Öngörüler sağlar. Tanılama günlükleri, kaynağınızın gerçekleştirilen işlemler hakkında bilgi sağlar. Daha fazla bilgi için [Azure İzleyici tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-overview.md).

![Tanılama günlükleri](./media/front-door-diagnostics/diagnostic-log.png)

Ön kapısı hizmetiniz için tanılama günlüklerini yapılandırmak için:

1. Azure ön kapısı hizmetinizi seçin.

2. Seçin **tanılama ayarları**.

3. Seçin **tanılamayı Aç**. Bir depolama hesabına ölçümlerle birlikte tanılama günlüklerini arşivleme, bunları bir olay hub'ına akış veya bunları Azure İzleyici günlüklerine gönderin.

Ön kapısı hizmeti şu anda tanılama günlükleri (saatlik olarak toplanır) sağlar. Tanılama günlükleri, API istekleri ayrı ayrı her bir girdi aşağıdaki şemayı içerecek sağlar:

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

- [Bir ön kapısı profili oluşturma](quickstart-create-front-door.md)
- [Ön kapısı işleyişi](front-door-routing-architecture.md)
