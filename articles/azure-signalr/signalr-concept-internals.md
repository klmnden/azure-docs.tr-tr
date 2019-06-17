---
title: Azure SignalR Service iç işlevleri
description: Azure SignalR hizmeti dahili bileşenleri genel bakış.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: cbcdfccfdca1dbed3b766b3f50295b1d355b3478
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61401765"
---
# <a name="azure-signalr-service-internals"></a>Azure SignalR Service iç işlevleri

Azure SignalR hizmeti, ASP.NET Core SignalR çerçevesi üzerine oluşturulmuştur. Ayrıca bir önizleme özelliği olarak ASP.NET SignalR destekler.

> Azure SignalR hizmeti ASP.NET Core framework üzerinde ASP.NET SignalR Veri Protokolü in ASP.NET Signalr'yi destekleyen için

Birkaç kod satırıyla SignalR hizmet ile çalışması için yerel bir ASP.NET Core SignalR uygulama kolayca geçirebilirsiniz.

SignalR Service, uygulama sunucusu ile kullandığınızda, aşağıdaki diyagramda tipik mimarisini açıklar.

Şirket içinde barındırılan bir ASP.NET Core SignalR uygulamadan farklar de ele alınmıştır.

![Mimari](./media/signalr-concept-internals/arch.png)

## <a name="server-connections"></a>Sunucu bağlantıları

Şirket içinde barındırılan ASP.NET Core SignalR uygulama sunucusu dinleyen ve istemcileri doğrudan bağlanır.

SignalR hizmeti ile uygulama sunucusu artık kalıcı istemci bağlantıları, bunun yerine kabul ediyor:

1. A `negotiate` uç noktası için her hub'ı Azure SignalR hizmeti SDK'sı tarafından gösterilir.
1. Bu uç nokta, istemcinin anlaşma isteklerine yanıt ve istemciler için SignalR hizmeti yeniden yönlendirme.
1. Sonuç olarak, istemciler SignalR hizmetine bağlanır.

Daha fazla bilgi için [istemci bağlantıları](#client-connections).

Uygulama sunucusu başlatıldıktan sonra 
- Azure SignalR hizmeti SDK'sı, ASP.NET Core SignalR için SignalR hizmeti için hub başına 5 WebSocket bağlantılarını açılır. 
- Azure SignalR hizmeti SDK'sı, ASP.NET SignalR için SignalR hizmeti için hub başına 5 WebSocket bağlantı ve uygulama WebSocket bağlantısı başına açılır.

5 WebSocket bağlantılarını içinde değiştirilebilen varsayılan değer olan [yapılandırma](https://github.com/Azure/azure-signalr/blob/dev/docs/use-signalr-service.md#connectioncount).

Ve istemcilerden gelen ileti, bu bağlantılar arttıkça.

Bu bağlantıların her zaman SignalR hizmete bağlı olarak kalır. Ağ sorun için bir sunucu bağlantısı kesilirse,
- Bu sunucu bağlantısını kes tarafından sunulan tüm istemciler (ilgili daha fazla bilgi için bkz. [istemci ile sunucu arasında veri aktarmak](#data-transmit-between-client-and-server));
- Sunucu bağlantısı, otomatik olarak yeniden bağlanmayı denediğinde.

## <a name="client-connections"></a>İstemci bağlantıları

SignalR hizmeti kullandığınızda, istemcilerin uygulama sunucusu yerine SignalR hizmetine bağlanın.
SignalR hizmet ve istemci arasında kalıcı bağlantılar kurmak için iki adım vardır.

1. İstemci uygulama sunucusuna bir anlaşma isteği gönderir. Azure SignalR Service SDK'sı ile uygulama sunucusu SignalR hizmet URL'si ve erişim belirteci ile bir yeniden yönlendirme yanıtı döndürür.

- ASP.NET Core SignalR için tipik bir yeniden yönlendirme yanıtı şuna benzer:
    ```
    {
        "url":"https://test.service.signalr.net/client/?hub=chat&...",
        "accessToken":"<a typical JWT token>"
    }
    ```
- ASP.NET SignalR için tipik bir yeniden yönlendirme yanıtı şuna benzer:
    ```
    {
        "ProtocolVersion":"2.0",
        "RedirectUrl":"https://test.service.signalr.net/aspnetclient",
        "AccessToken":"<a typical JWT token>"
    }
    ```

1. Yeniden yönlendirme yanıtı aldıktan sonra istemci SignalR hizmetine bağlanmak için normal işlemini başlatmak için yeni URL'si ve erişim belirtecini kullanır.

ASP.NET Core SignalR hakkında daha fazla bilgi [aktarım protokolleri](https://github.com/aspnet/SignalR/blob/release/2.2/specs/TransportProtocols.md).

## <a name="data-transmit-between-client-and-server"></a>İstemci ile sunucu arasında veri aktarmak

Bir istemci SignalR hizmete bağlandığında, hizmet çalışma zamanı bu istemci görev yapacak bir sunucu bağlantısı bulacaksınız
- Bu adım yalnızca bir kez gerçekleşir ve istemci ve sunucu bağlantıları arasında bire bir eşleme.
- Eşleme kadar istemci SignalR hizmetinde korunmadığından veya sunucu bağlantısını keser.

Bu noktada, uygulama sunucusu yeni istemci tarafından bir olay bilgileriyle alır. Mantıksal bir bağlantı, uygulama sunucusunda oluşturulur. Veri kanalı için SignalR hizmeti aracılığıyla uygulama sunucusu istemciden kurulur.

SignalR hizmeti, verileri istemciden eşleştirme uygulama sunucusuna iletir. Ve veri uygulama sunucusundan eşlenen istemcilere gönderilir.

Gördüğünüz gibi Azure SignalR hizmeti aslında bir mantıksal Aktarım katmanı uygulama sunucusu ve istemcileri arasında ' dir. Tüm kalıcı bağlantılar için SignalR hizmeti Boşaltılan.
Uygulama sunucusu, yalnızca istemci bağlantıları hakkında endişelenmeden hub sınıfında, iş mantığı işlemek gerekir.