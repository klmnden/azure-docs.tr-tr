---
title: Azure SignalR nedir? | Microsoft Docs
description: Azure SignalR hizmetine genel bakış.
services: signalr
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.service: signalr
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 04/17/2018
ms.author: wesmc
ms.openlocfilehash: e24091b017a1c6c82cfe8d12873223b98c165c63
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="what-is-azure-signalr-service"></a>Azure SignalR Hizmeti nedir?

Azure SignalR Hizmeti, [ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction)’yi temel alan bir Azure hizmetidir. ASP.NET Core SignalR, HTTP üzerinden uygulamalara gerçek zamanlı web işlevselliği ekleme işlemini basitleştiren bir [açık kaynak kitaplıktır](https://github.com/aspnet/signalr). Bu gerçek zamanlı işlevsellik, web sunucusunun bağlı istemcilere içerik göndermesine olanak tanır. Sonuç olarak, istemciler sunucuyu yoklama veya yeni HTTP isteklerini güncelleştirmek üzere gönderme gereksinimi olmadan güncelleştirilir.

Bu makalede Azure SignalR Hizmeti’ne genel bir bakış sunulmaktadır. Çalışmaya başlamak istiyorsanız [ASP.NET Core hızlı başlangıcı](signalr-quickstart-dotnet-core.md) ile başlayın.

## <a name="what-is-signalr-service-used-for"></a>SignalR Hizmeti ne için kullanılır? 

Gerçek zamanlı içerik güncelleştirmeleri gerektiren çok sayıda uygulama türü vardır. Aşağıdaki örnek uygulama türleri, Azure SignalR Hizmeti’ni kullanmak için iyi adaylardır:

* Sunucudan yüksek sıklıkta güncelleştirmeler gerektiren uygulamalar. Oyun, sosyal ağlar, oylama, açık artırma, haritalar ve GPS uygulamaları bunlara örnektir.
* Panolar ve izleme uygulamaları. Şirket panoları, anlık satış güncelleştirmeleri veya seyahat uyarıları bunlara örnektir.
* İş birliği uygulamaları. Beyaz tahta uygulamaları ve takım toplantısı yazılımları, iş birliği uygulamalarına örnektir.
* Bildirim gerektiren uygulamalar. Sosyal ağlar, e-posta, sohbet, oyunlar, seyahat uyarıları ve diğer birçok uygulama, bildirimleri kullanır.

Dahili olarak SignalR, gerçek zamanlı web uygulamaları derlemek için kullanılan birkaç tekniğin özetidir. [WebSockets](https://wikipedia.org/wiki/WebSocket) ideal aktarım yöntemidir ancak başka seçenek olmadığında [Sunucu ile Gönderilen Olaylar (SSE)](https://wikipedia.org/wiki/Server-sent_events) ve Uzun Yoklama gibi diğer teknikler de kullanılır. SignalR, sunucu ve istemci üzerinde desteklenen özelliklere göre uygun taşıma tekniğini otomatik olarak algılar ve başlatır.

## <a name="developing-signalr-apps"></a>SignalR uygulamaları geliştirme

Şu anda web uygulamalarınızla birlikte kullanabileceğiniz iki SignalR sürümü mevcuttur: ASP.NET için SignalR ve en yeni sürüm olan ASP.NET Core SignalR. Aynı zamanda *SignalR Hizmeti* olarak bilinen Azure SignalR Hizmeti, ASP.NET Core SignalR’yi temel alan, yönetilen bir Azure hizmetidir. 

ASP.NET Core SignalR, önceki sürümün yeniden üretimidir. Sonuç olarak ASP.NET Core SignalR, önceki SignalR sürümüyle geriye dönük olarak uyumlu değildir. API’ler ve davranışlar farklıdır. ASP.NET Core SignalR SDK’sı, .NET Standard’dır; bu nedenle .NET Framework ile hala kullanabilirsiniz. Ancak, eski API’ler yerine yenilerini kullanmanız gerekir. SignalR kullanıyor ve ASP.NET Core SignalR’ye veya Azure SignalR Hizmeti’ne geçmek istiyorsanız, kodunuzu API’lerdeki farklılıkları işleyecek şekilde değiştirmeniz gerekir.

Azure SignalR Hizmeti ile, ASP.NET Core SignalR’nin sunucu tarafı bileşeni Azure’da barındırılır. Ancak, teknoloji ASP.NET Core temel alınarak oluşturulduğu için gerçek web uygulamanızı birden fazla platform (Windows, Linux ve MacOS) üzerinde çalıştırırken [Azure App Service](../app-service/app-service-web-overview.md), [IIS](https://docs.microsoft.com/aspnet/core/host-and-deploy/iis/index), [Nginx](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-nginx), [Apache](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-apache), [Docker](https://docs.microsoft.com/aspnet/core/host-and-deploy/docker/index) ile barındırabilirsiniz. Ayrıca, işleminizin içinde kendi kendine barındırmayı da kullanabilirsiniz.

Uygulamanızın amaçları arasında web istemcilerine gerçek zamanlı içerik güncelleştirmeleri uygulamak için en son işlevleri desteklemek, birden fazla platform üzerinde çalışmak (Azure, Windows, Linux ve MacOS) ve farklı ortamlarda barındırmak varsa, Azure SignalR Hizmeti’nden yararlanmak en iyi seçim olacaktır.


## <a name="why-not-deploy-signalr-myself"></a>SignalR’yi neden kendim dağıtamıyorum?

Tüm web uygulamanıza arka uç bileşeni olarak ASP.NET Core SignalR’yi destekleyen kendi Azure web uygulamanızı dağıtmak hala geçerli bir yaklaşımdır.

Azure SignalR Hizmeti’ni kullanmanın başlıca nedenlerinden biri kolaylık sağlamasıdır. Azure SignalR Hizmeti ile performans, ölçeklenebilirlik ve kullanılabilirlik gibi sorunları ele almanız gerekmez. Bu sorunlar % 99,9 hizmet düzeyi sözleşmesi ile sizin yerinize ele alınır.

Ayrıca, gerçek zamanlı içerik güncelleştirmelerini desteklemek için WebSockets genellikle tercih edilen yöntemdir. Bununla birlikte, çok sayıda kalıcı WebSocket bağlantısı ile yük dengelemesi yapmak, ölçeği artırdıkça çözülmesi karmaşık bir sorun haline gelir. Yaygın sorunlar şunlardan yararlanır: DNS yük dengelemesi, donanım yük dengeleyicileri ve yazılım yük dengelemesi. Azure SignalR Hizmeti bu sorunu sizin için halleder.

Geçerli olabilecek başka bir neden ise gerçekten bir web uygulaması barındırmak zorunda olmamanızdır. Web uygulamanızın mantığı, [Sunucusuz bilgi işlem](https://azure.microsoft.com/overview/serverless-computing/)’den yararlanabilir. Örneğin, kodunuz yalnızca [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/) tetikleyicileri ile isteğe bağlı olarak barındırılıyor ve yürütülüyor olabilir. Kodunuz yalnızca isteğe bağlı olarak çalıştığından ve istemcilerle uzun bağlantılar sürdürmediğinden bu senaryo aldatıcı olabilir. Azure SignalR Hizmeti, bağlantıları zaten yönettiği için bu durumu sizin yerinize çözebilir.

## <a name="how-does-it-scale"></a>Nasıl ölçeklendirilir?

SignalR’yi SQL Server, Azure Service Bus veya Redis Cache ile ölçeklendirmek yaygın bir yöntemdir. Azure SignalR Hizmeti, ölçeklendirme yaklaşımını sizin yerinize ele alır. Performans ve maliyet, bu yaklaşımlarla benzerdir ve diğer hizmetlerdeki gibi karmaşık değildir. Tüm yapmanız gereken, hizmetiniz için birim sayısını güncelleştirmektir. Her hizmet birimi en fazla 1000 istemci bağlantısını destekler.

## <a name="next-steps"></a>Sonraki adımlar
* [Hızlı Başlangıç: Azure SignalR ile sohbet odası oluşturma](signalr-quickstart-dotnet-core.md)  
  

