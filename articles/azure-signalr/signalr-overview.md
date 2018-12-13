---
title: Azure SignalR nedir
description: Azure SignalR hizmetine genel bakış.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: e66326c6c4d93a92c579255cb00b6614ecc03b8c
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53255185"
---
# <a name="what-is-azure-signalr-service"></a>Azure SignalR Hizmeti nedir?

Azure SignalR Hizmeti, HTTP üzerinden uygulamalara gerçek zamanlı web işlevselliği ekleme işlemini basitleştirir. Bu gerçek zamanlı işlevsellik, hizmetin bağlı istemcilere tek sayfalık bir web veya mobil uygulaması gibi içerik güncelleştirmeleri göndermesine olanak tanır. Sonuç olarak, istemciler sunucuyu yoklama veya yeni HTTP isteklerini güncelleştirmek üzere gönderme gereksinimi olmadan güncelleştirilir.

Bu makalede Azure SignalR Hizmetine genel bir bakış sunulmaktadır.

## <a name="what-is-azure-signalr-service-used-for"></a>Azure SignalR Hizmeti ne için kullanılır?

Gerçek zamanlı içerik güncelleştirmeleri gerektiren çok sayıda uygulama türü vardır. Aşağıdaki örnekler, Azure SignalR Hizmeti’ni kullanmak için iyi adaylardır:

* Sunucudan yüksek sıklıkta güncelleştirmeler gerektiren uygulamalar. Oyun, oylama, açık artırma, haritalar ve GPS uygulamaları bunlara örnektir.
* Panolar ve izleme uygulamaları. Şirket panoları ve anlık satış güncelleştirmeleri bunlara örnektir.
* İş birliği uygulamaları. Beyaz tahta uygulamaları ve takım toplantısı yazılımları, iş birliği uygulamalarına örnektir.
* Bildirim gerektiren uygulamalar. Sosyal ağlar, e-posta, sohbet, oyunlar, seyahat uyarıları ve diğer birçok uygulama, bildirimleri kullanır.

SignalR, gerçek zamanlı web uygulamaları derlemek için kullanılan birkaç tekniğin özetini sunar. [WebSockets](https://wikipedia.org/wiki/WebSocket) ideal aktarım yöntemidir ancak başka seçenek olmadığında [Sunucu ile Gönderilen Olaylar (SSE)](https://wikipedia.org/wiki/Server-sent_events) ve Uzun Yoklama gibi diğer teknikler de kullanılır. SignalR, sunucu ve istemci üzerinde desteklenen özelliklere göre uygun taşıma tekniğini otomatik olarak algılar ve başlatır.

SignalR ayrıca gerçek zamanlı uygulamalar için sunucunun bütün bağlantılara veya bağlantıların belirli bir kullanıcıya ait olan bir alt kümesine ya da rastgele bir gruba yerleştirilmiş olanlara mesajlar göndermesini sağlayan bir programlama modeli sunar.

## <a name="how-to-use-azure-signalr-service"></a>Azure SignalR Service Nasıl Kullanılır

Şu anda Azure SignalR Hizmetini kullanmanın üç yolu vardır:

- **[Bir ASP.NET Core SignalR Uygulamasını Ölçeklendirme](signalr-overview-scale-aspnet-core.md)** - Yüzbinlerce bağlantıya ölçeklendirmek için Azure SignalR Hizmetini bir ASP.NET Core SignalR uygulaması ile tümleştirin.
- **[Sunucusuz, gerçek zamanlı uygulamalar oluşturma](signalr-overview-azure-functions.md)** JavaScript, C# ve Java gibi dillerde sunucusuz, gerçek zamanlı uygulamalar oluşturmak için Azure İşlevleri tümleştirmesini Azure SignalR Hizmeti ile kullanın.
- **[REST API aracılığıyla istemcilere sunucudan iletileri gönderme](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md)** - Azure SignalR Hizmeti, REST kullanılabilen tüm dillerle uygulamaların SignalR Hizmeti ile bağlı istemcilere iletiler gönderebilmesi için REST API’sini sunar.