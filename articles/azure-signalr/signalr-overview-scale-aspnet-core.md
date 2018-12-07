---
title: Azure SignalR ile ASP.NET Core SignalR’yi ölçeklendirme | Microsoft Belgeleri
description: ASP.NET Core SignalR uygulamalarını ölçeklendirmek için Azure SignalR hizmetini kullanmaya genel bir bakış.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.service: signalr
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 09/13/2018
ms.author: zhshang
ms.openlocfilehash: 1492b2145187f7334d1e7d9df91adc109ca826ee
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53016938"
---
# <a name="scale-aspnet-core-signalr-applications-with-azure-signalr-service"></a>Azure SignalR Hizmeti ile ASP.NET Core SignalR uygulamalarını ölçeklendirme

## <a name="developing-signalr-apps"></a>SignalR uygulamaları geliştirme

Şu anda web uygulamalarınızla birlikte kullanabileceğiniz [iki SignalR sürümü](https://docs.microsoft.com/aspnet/core/signalr/version-differences) mevcuttur: ASP.NET için SignalR ve en yeni sürüm olan ASP.NET Core SignalR. Azure SignalR Hizmeti, ASP.NET Core SignalR’yi temel alan, yönetilen bir Azure hizmetidir. 

ASP.NET Core SignalR, önceki sürümün yeniden üretimidir. Sonuç olarak ASP.NET Core SignalR, önceki SignalR sürümüyle geriye dönük olarak uyumlu değildir. API’ler ve davranışlar farklıdır. ASP.NET Core SignalR SDK’sı, .NET Standard’ı hedef alır; bu nedenle .NET Framework ile hala kullanabilirsiniz. Ancak, eski API’ler yerine yenilerini kullanmanız gerekir. SignalR kullanıyor ve ASP.NET Core SignalR’ye veya Azure SignalR Hizmeti’ne geçmek istiyorsanız, kodunuzu API’lerdeki farklılıkları işleyecek şekilde değiştirmeniz gerekir.

Azure SignalR Hizmeti ile, ASP.NET Core SignalR’nin sunucu tarafı bileşeni Azure’da barındırılır. Ancak, teknoloji ASP.NET Core temel alınarak oluşturulduğu için gerçek web uygulamanızı birden fazla platform (Windows, Linux ve MacOS) üzerinde çalıştırırken [Azure App Service](../app-service/app-service-web-overview.md), [IIS](https://docs.microsoft.com/aspnet/core/host-and-deploy/iis/index), [Nginx](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-nginx), [Apache](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-apache), [Docker](https://docs.microsoft.com/aspnet/core/host-and-deploy/docker/index) ile barındırabilirsiniz. Ayrıca, işleminizin içinde kendi kendine barındırmayı da kullanabilirsiniz.

Uygulamanızın amaçları arasında web istemcilerine gerçek zamanlı içerik güncelleştirmeleri uygulamak için en son işlevleri desteklemek, birden fazla platform üzerinde çalışmak (Azure, Windows, Linux ve MacOS) ve farklı ortamlarda barındırmak varsa, Azure SignalR Hizmetinden yararlanmak en iyi seçim olacaktır.


## <a name="why-not-deploy-signalr-myself"></a>SignalR’yi neden kendim dağıtamıyorum?

Tüm web uygulamanıza arka uç bileşeni olarak ASP.NET Core SignalR’yi destekleyen kendi Azure web uygulamanızı dağıtmak hala geçerli bir yaklaşımdır.

Azure SignalR Hizmeti’ni kullanmanın başlıca nedenlerinden biri kolaylık sağlamasıdır. Azure SignalR Hizmeti ile performans, ölçeklenebilirlik ve kullanılabilirlik gibi sorunları ele almanız gerekmez. Bu sorunlar % 99,9 hizmet düzeyi sözleşmesi ile sizin yerinize ele alınır.

Ayrıca, gerçek zamanlı içerik güncelleştirmelerini desteklemek için WebSockets genellikle tercih edilen yöntemdir. Bununla birlikte, çok sayıda kalıcı WebSocket bağlantısı ile yük dengelemesi yapmak, ölçeği artırdıkça çözülmesi karmaşık bir sorun haline gelir. Yaygın sorunlar şunlardan yararlanır: DNS yük dengelemesi, donanım yük dengeleyicileri ve yazılım yük dengelemesi. Azure SignalR Hizmeti bu sorunu sizin için halleder.

Geçerli olabilecek başka bir neden ise gerçekten bir web uygulaması barındırmak zorunda olmamanızdır. Web uygulamanızın mantığı, [Sunucusuz bilgi işlem](https://azure.microsoft.com/overview/serverless-computing/)’den yararlanabilir. Örneğin, kodunuz yalnızca [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/) tetikleyicileri ile isteğe bağlı olarak barındırılıyor ve yürütülüyor olabilir. Kodunuz yalnızca isteğe bağlı olarak çalıştığından ve istemcilerle uzun bağlantılar sürdürmediğinden bu senaryo aldatıcı olabilir. Azure SignalR Hizmeti, bağlantıları zaten yönettiği için bu durumu sizin yerinize çözebilir. Daha fazla ayrıntı için bkz. [SignalR Hizmetini Azure İşlevleri ile kullanmaya genel bakış](signalr-overview-azure-functions.md). 

## <a name="how-does-it-scale"></a>Nasıl ölçeklendirilir?

SignalR ile SQL Server, Azure Service Bus veya Azure önbelleği için Redis ölçeklendirmek için yaygındır. Azure SignalR Hizmeti, ölçeklendirme yaklaşımını sizin yerinize ele alır. Performans ve maliyet, bu yaklaşımlarla benzerdir ve diğer hizmetlerdeki gibi karmaşık değildir. Tüm yapmanız gereken, hizmetiniz için birim sayısını güncelleştirmektir. Her birim en fazla 1000 istemci bağlantısını destekler.

## <a name="next-steps"></a>Sonraki adımlar
* [Hızlı Başlangıç: Azure SignalR ile sohbet odası oluşturma](signalr-quickstart-dotnet-core.md)  
  

