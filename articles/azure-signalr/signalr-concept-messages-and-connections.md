---
title: İletileri ve Azure SignalR hizmeti bağlantıları
description: Temel kavramları iletileri ve Azure SignalR hizmeti bağlantıları hakkında genel bakış.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: e82ce8f5c97aed7e2cb832d8e808ff84691f7c9e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61401211"
---
# <a name="messages-and-connections-in-azure-signalr-service"></a>İletileri ve Azure SignalR hizmeti bağlantıları

Azure SignalR hizmeti için faturalandırma modeli, bağlantı sayısı ve iletilerin sayısını temel alır. Bu makalede nasıl iletileri ve bağlantıları tanımlanır ve faturalandırma için sayılan açıklanmaktadır.


## <a name="message-formats"></a>İleti formatları 

Azure SignalR hizmeti, ASP.NET Core SignalR olarak aynı biçimi destekler: [JSON](https://www.json.org/) ve [MessagePack](/aspnet/core/signalr/messagepackhubprotocol).

## <a name="message-size"></a>İleti boyutu

Azure SignalR hizmeti, iletileri için boyut sınırı vardır.

Büyük iletileri en fazla 2 KB her küçük iletilere bölme ve ayrı olarak iletilebilir. SDK'ları, bölme ve birleştirme ileti işler. Hiçbir Geliştirici çalışmalarını gereklidir.

Büyük iletileri Mesajlaşma performansı olumsuz şekilde etkileyebilir. Her kullanım örneği senaryosu için en iyi ileti boyutunu belirlemek için test ve küçük ileti mümkün olduğunca kullanın.

## <a name="how-messages-are-counted-for-billing"></a>İletileri için faturalandırma nasıl sayılır

Azure SignalR hizmeti yalnızca giden iletiler için faturalandırma, sayılır. İstemciler ve sunucular arasında ping iletilerine göz ardı edilir.

2 KB'den daha büyük iletiler 2 KB birden çok ileti olarak sayılır. Azure portalında ileti sayısı grafiği güncelleştirilmiş hub başına 100 her ileti.

Örneğin, üç istemcileri ve bir uygulama sunucusu olduğunu düşünün. Bir istemci, sunucunun tüm istemcilere yayın bildirmek için 4 KB'lık ileti gönderir. İleti sayısı sekiz olduğu: uygulama sunucusu ve istemcilere hizmetinden üç iletileri hizmetinden bir ileti. Her ileti iki 2 KB'lık mesaj olarak sayılır.

Azure portalında gösterilen mesaj sayısı 0 ermesine 100'den büyük olarak toplanır.

## <a name="how-connections-are-counted"></a>Bağlantıları nasıl sayılır

Sunucu bağlantısı ve istemci bağlantıları vardır. Varsayılan olarak, her uygulama sunucusu başına hub'ı Azure SignalR hizmeti ile beş bağlantıları vardır ve her bir istemciye Azure SignalR hizmeti ile bir istemci bağlantısı olur.

Azure portalında gösterilen bağlantı sayısı, hem sunucu bağlantıları hem de istemci bağlantılarını içerir.

Örneğin, iki uygulama sunucuları vardır ve kodun içinde beş hub'ları tanımlamak varsayılır. Sunucu bağlantısı sayısı 50 olur: 2 uygulama sunucuları * 5 hubs * hub başına 5 bağlantı.

ASP.NET SignalR, farklı bir yolla sunucu bağlantılarını hesaplar. Bu, tanımladığınız hub'ları ek olarak bir varsayılan hub içerir. Varsayılan olarak, her uygulama sunucusunun beş daha fazla sunucu bağlantısı gerekir. Varsayılan hub için bağlantı sayısı, diğer hub'lar ile tutarlı kalır.

## <a name="how-inboundoutbound-traffic-is-counted"></a>Gelen/giden trafik sayılır nasıl

Azure SignalR hizmeti perspektifi de gelen ve giden trafiği arasındaki ayrımı temel alır. Trafiğin bayt cinsinden hesaplanır. Trafik, ileti sayısı gibi bir örnekleme hızı de vardır. Azure portalında gelen/giden grafik güncelleştirilir hub başına her 100 KB.

## <a name="related-resources"></a>İlgili kaynaklar

- [Azure İzleyici'de toplama türleri](/azure/azure-monitor/platform/metrics-supported#microsoftsignalrservicesignalr )
- [ASP.NET Core SignalR yapılandırma](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)