---
title: Azure SignalR hizmeti hakkında sık sorulan sorular
description: Azure SignalR hizmeti hakkında SSS.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: 09788f4ded66b43fd5ecae20301a28cd01d77320
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60646654"
---
# <a name="azure-signalr-service-faq"></a>Azure SignalR hizmeti hakkında SSS

## <a name="is-azure-signalr-service-ready-for-production-use"></a>Azure SignalR hizmeti, üretim kullanımı için hazır mı?

Evet.
Genel kullanılabilirlik yaptığımız duyurudan için bkz: [Azure SignalR hizmeti artık genel kullanıma sunulan](https://azure.microsoft.com/en-us/blog/azure-signalr-service-now-generally-available/). 

[ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction) tam olarak desteklenir.

ASP.NET SignalR için destek, hala *genel Önizleme*. İşte bir [kod örneği](https://github.com/aspnet/AzureSignalR-samples/tree/master/aspnet-samples/ChatRoom).

## <a name="the-client-connection-closes-with-the-error-message-no-server-available-what-does-it-mean"></a>İstemci bağlantısı "Sunucu yok" hata iletisiyle kapatır. Bu ne demektir?

Yalnızca istemciler iletileri için SignalR hizmeti gönderirken bu hata oluşur.

Yoksa herhangi bir uygulama sunucusu ve yalnızca SignalR hizmeti REST API'si kullanın, bu, davranıştır **tasarıma**.
Sunucusuz mimari, istemci bağlantıları bulunan **DİNLEME** modu ve SignalR hizmeti iletileri gönderip olacaktır.
Daha fazla bilgi [REST API](./signalr-quickstart-rest-api.md).

Uygulama sunucuları varsa, bu hata iletisi, uygulama sunucusu yok SignalR hizmet Örneğinize bağlı olduğundan emin anlamına gelir.

Olası nedenler şunlardır:
- Hiçbir uygulama sunucusu SignalR hizmeti ile bağlandı. Olası bağlantı hataları için uygulama sunucusu günlüklerini kontrol edin. Bu durumda birden fazla uygulama sunucuları ile yüksek oranda kullanılabilirlik ayarında nadir olarak rastlanıyor.
- SignalR hizmet örnekleri ile bağlantı sorunları var. Bu sorunu geçici olduğu ve otomatik olarak kurtarılacak.
Bir saatten için ederse [github'da bir sorun açın](https://github.com/Azure/azure-signalr/issues/new) veya [Azure'da bir destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request).

## <a name="when-there-are-multiple-application-servers-are-client-messages-sent-to-all-servers-or-just-one-of-them"></a>Birden çok uygulama sunucuları olduğunda, tüm sunucular veya bunlardan yalnızca birini istemci iletileri gönderilir?

Bu, istemci ve uygulama sunucusu arasında bire bir eşleme olur. Bir istemciden gelen iletilerin her zaman aynı uygulama sunucusuna gönderilir.

İstemci ve uygulama sunucusu arasındaki eşleme, istemci veya uygulama sunucusu kesilene kadar korunur.

## <a name="if-one-of-my-application-servers-is-down-how-can-i-find-it-and-get-notified"></a>Uygulama Sunucularım biri nasıl bulmak ve almak, basılı olduğunda bildirim?

SignalR Service, uygulama sunucuları gelen sinyalleri izler.
Belirtilen bir süre için sinyal alınmadı, uygulama sunucusu çevrimdışı kabul edilir. Bu uygulama sunucusuna eşlenen tüm istemci bağlantıları kesilir.

## <a name="why-does-my-custom-iuseridprovider-throw-exception-when-switching-from-aspnet-core-signalr--sdk-to-azure-signalr-service-sdk"></a>Özel neden mu `IUserIdProvider` Azure SignalR hizmeti SDK'sı için ASP.NET Core SignalR SDK'sından geçiş yaparken özel durum?

Parametre `HubConnectionContext context` ASP.NET Core SignalR SDK'sı ile Azure SignalR hizmeti SDK'sı arasında farklı olduğunda `IUserIdProvider` çağrılır.

ASP.NET Core signalr'da `HubConnectionContext context` bağlamına ait tüm özellikleri için geçerli değerlerle fiziksel istemci bağlantısı.

Azure SignalR hizmeti SDK'da `HubConnectionContext context` bağlamına ait mantıksal istemci bağlantısı. Fiziksel istemci bağlantısı SignalR hizmet örneği için bu nedenle bağlı yalnızca sınırlı sayıda özellikleri sağlanır.

Şimdilik yalnızca `HubConnectionContext.GetHttpContext()` ve `HubConnectionContext.User` erişilebilir.
Kaynak kod kontrol edebilirsiniz [burada](https://github.com/Azure/azure-signalr/blob/kevinzha/faq/src/Microsoft.Azure.SignalR/ServiceHubConnectionContext.cs).

## <a name="can-i-configure-the-transports-available-in-signalr-service-as-configuring-it-on-server-side-with-aspnet-core-signalr-for-example-disable-websocket-transport"></a>SignalR hizmetinde kullanılabilir taşımalar ASP.NET Core SignalR ile sunucu tarafında yapılandırma olarak yapılandırabilir miyim? Örneğin, WebSocket aktarım devre dışı?

Hayır.

Azure SignalR hizmeti varsayılan olarak ASP.NET Core Signalr'yi destekleyen tüm üç aktarımları sağlar. Yapılandırılabilir değildir. SignalR hizmet bağlantıları ve tüm istemci bağlantılar için aktarmaları işler.

İstemci tarafı taşımalar belgelendiği gibi yapılandırabilirsiniz [burada](https://docs.microsoft.com/aspnet/core/signalr/configuration?view=aspnetcore-2.1#configure-allowed-transports).
