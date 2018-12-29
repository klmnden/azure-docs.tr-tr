---
title: İletileri ve Azure SignalR bağlantıları
description: Anahtar kavramlar iletileri ve Azure SignalR hizmeti bağlantıları genel bakış.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: 5a0430e9ad124319147342c49fc51e11472ac8ff
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2018
ms.locfileid: "53811893"
---
# <a name="message-and-connection-in-azure-signalr-service"></a>İleti ve Azure SignalR hizmeti bağlantı

Azure SignalR hizmeti bağlantısı sayısı ve iletilerin sayısını göre faturalandırma modeli vardır. Nasıl iletileri ve bağlantıları tanımlanır ve faturalama amacıyla sayılan aşağıda açıklanmıştır.

## <a name="message-formats-supported"></a>Desteklenen ileti formatları

Azure SignalR hizmeti, ASP.NET Core SignalR desteklediği aynı biçimlerini destekler: [JSON](https://www.json.org/) ve [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)

## <a name="message-size"></a>İleti boyutu

Azure SignalR hizmeti ileti boyutu sınırı yoktur.

Uygulamada, büyük ileti küçük bölündüğünü en fazla 2 KB her, iletileri ve ayrı ileti olarak aktarılan. Bölme ve birleştirme ileti SDK'lar tarafından işlenir. Hiçbir Geliştirici çalışmalarını gereklidir.

Ancak büyük ileti Mesajlaşma performans üzerinde negatif bir etkisi yoktur. Daha küçük ileti boyutu mümkün olduğunca kullanın ve her kullanım örneği senaryosu için en iyi ileti boyutu seçmek için test edin.

## <a name="how-to-count-messages-for-billing-purpose"></a>Faturalama amaçlı iletileri saymak nasıl?

Biz yalnızca SignalR hizmetinden giden ileti sayısı ve istemciler ve sunucular arasında ping iletilerine yoksay.

İleti 2 KB 2 KB olarak birden çok ileti olarak sayılır daha büyük. İleti sayısı grafiği Azure portalında hub başına her 100 mesaj güncelleştirir.

Örneğin, bir kullanıcı 3 istemcileri ve 1 uygulama sunucusu vardır. Bir istemci, sunucunun tüm istemcilere yayın bildirmek için 4 KB'lık bir ileti gönderir. İleti sayısı 8 olacaktır: Uygulama sunucusu, istemcilere hizmetinden 3 mesaj ve her ileti 1 ileti hizmetinden 2 KB 2 ileti olarak sayılır.

Azure portalında gösterilen mesaj sayısı 100'den fazla olacak şekilde biriktirir kadar hala 0 ' dir.

## <a name="how-to-count-connections"></a>Bağlantıları sayar nasıl?

Sunucu bağlantısı ve istemci bağlantıları vardır. Varsayılan olarak her uygulama sunucusunun SignalR Service hub başına 5 bağlantı ve her istemci SignalR hizmetiyle 1 istemci bağlantısı vardır.

Azure portalında gösterilen bağlantı sayısı, hem sunucu bağlantıları hem de istemci bağlantılarını içerir.

Örneğin, bir kullanıcı iki uygulama sunucuları vardır ve kodlarını 5 hubs tanımlar. Azure portalında gösterilen sunucu bağlantısı sayısı 2 uygulama sunucuları olacak * 5 hubs * 5 bağlantıları/hub = 50 sunucu bağlantıları.

## <a name="related-resources"></a>İlgili kaynaklar

- [ASP.NET Core SignalR yapılandırma](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)