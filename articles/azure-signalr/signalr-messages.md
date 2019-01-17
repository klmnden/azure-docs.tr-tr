---
title: İletileri ve Azure SignalR bağlantıları
description: Anahtar kavramlar iletileri ve Azure SignalR hizmeti bağlantıları genel bakış.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: c2348df7a1a55584807a03216e294486ddadfc52
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352606"
---
# <a name="message-and-connection-in-azure-signalr-service"></a>İleti ve Azure SignalR hizmeti bağlantı

Azure SignalR hizmeti bağlantısı sayısı ve iletilerin sayısını göre faturalandırma modeli vardır. Nasıl iletileri ve bağlantıları tanımlanır ve faturalama amacıyla sayılan aşağıda açıklanmıştır.

## <a name="message-formats-supported"></a>Desteklenen ileti formatları

Azure SignalR hizmeti, ASP.NET Core SignalR desteklediği aynı biçimlerini destekler: [JSON](https://www.json.org/) ve [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)

## <a name="message-size"></a>İleti boyutu

Azure SignalR hizmeti ileti boyutu sınırı yoktur.

Uygulamada, büyük ileti küçük bölündüğünü en fazla 2 KB her, iletileri ve ayrı ileti olarak aktarılan. SDK'ları, bölme ve birleştirme ileti işler. Hiçbir Geliştirici çalışmalarını gereklidir.

Ancak büyük ileti Mesajlaşma performans üzerinde negatif bir etkisi yoktur. Daha küçük ileti boyutu mümkün olduğunca kullanın ve her kullanım örneği senaryosu için en iyi ileti boyutu seçmek için test edin.

## <a name="how-to-count-messages-for-billing-purpose"></a>Faturalama amaçlı iletileri saymak nasıl?

Biz yalnızca SignalR hizmetinden giden ileti sayısı ve istemciler ve sunucular arasında ping iletilerine yoksay.

İleti 2 KB 2 KB olarak birden çok ileti olarak sayılır daha büyük. İleti sayısı grafiği Azure portalında hub başına her 100 mesaj güncelleştirir.

Örneğin, üç istemcileri ve bir uygulama sunucusu vardır. Bir istemci, sunucunun tüm istemcilere yayın bildirmek için 4 KB'lık bir ileti gönderir. İleti sayısı 8'dir: Uygulama sunucusu, istemcilerin hizmetten üç iletileri ve her bir iletiyi hizmetinden bir ileti, iki 2 KB'lık mesaj olarak sayılır.

Azure portalında gösterilen mesaj sayısı 100'den fazla olacak şekilde biriktirir kadar hala 0 ' dir.

## <a name="how-to-count-connections"></a>Bağlantıları sayar nasıl?

Sunucu bağlantısı ve istemci bağlantıları vardır. Varsayılan olarak her uygulama sunucusunun SignalR Service hub başına beş bağlantı ve her istemci SignalR hizmeti ile bir istemci bağlantısı vardır.

Azure portalında gösterilen bağlantı sayısı, hem sunucu bağlantıları hem de istemci bağlantılarını içerir.

Örneğin, iki uygulama sunucuları vardır ve kodlarını beş hub'ları tanımlayın. Sunucu bağlantısı sayısı 50'dir: 2 uygulama sunucuları * 5 hubs * 5 bağlantıları/hub.

ASP.NET SignalR, sunucu bağlantılarını hesaplamasında farklıdır. Müşteri tanımlı hub'ları ek olarak bir varsayılan hub var. Her uygulama sunucusu, varsayılan olarak 5 daha fazla sunucu bağlantısı gerekir. Bağlantı sayısı için varsayılan hub'ı diğer hub'ları ile tutarlı kalmasını sağlar.

## <a name="how-to-count-inbound-traffic--outbound-traffic"></a>Gelen trafiği sayma / giden trafik

Gelen / giden SignalR hizmeti açısından. Trafiğin bayt cinsinden hesaplanır. Trafik, ileti sayısı gibi kendi örnekleme hızını de vardır. Gelen / giden grafik Azure portalında hub başına her 100 KB güncelleştirir.

## <a name="related-resources"></a>İlgili kaynaklar

- [Azure İzleyici'de toplama türü](/azure/azure-monitor/platform/metrics-supported#microsoftsignalrservicesignalr )
- [ASP.NET Core SignalR yapılandırma](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)