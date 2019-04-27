---
title: Azure SignalR ile ASP.NET Core SignalR ölçek
description: ASP.NET Core SignalR uygulamalarını ölçeklendirmek için Azure SignalR hizmetini kullanmaya genel bir bakış.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: 8a4012d204b6dafa1233e4ce3d878590120be47d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60640234"
---
# <a name="scale-aspnet-core-signalr-applications-with-azure-signalr-service"></a>Azure SignalR Hizmeti ile ASP.NET Core SignalR uygulamalarını ölçeklendirme

## <a name="developing-signalr-apps"></a>SignalR uygulamaları geliştirme

Şu anda [iki sürümü](https://docs.microsoft.com/aspnet/core/signalr/version-differences) , SignalR web uygulamalarınızla kullanabilirsiniz: ASP.NET ve en yeni sürümü olan ASP.NET Core SignalR, SignalR. Azure SignalR Hizmeti, ASP.NET Core SignalR’yi temel alan, yönetilen bir Azure hizmetidir.

ASP.NET Core SignalR, önceki sürümün yeniden üretimidir. Sonuç olarak ASP.NET Core SignalR, önceki SignalR sürümüyle geriye dönük olarak uyumlu değildir. API’ler ve davranışlar farklıdır. ASP.NET Core SignalR SDK’sı, .NET Standard’ı hedef alır; bu nedenle .NET Framework ile hala kullanabilirsiniz. Ancak, eski API’ler yerine yenilerini kullanmanız gerekir. SignalR kullanıyor ve ASP.NET Core SignalR’ye veya Azure SignalR Hizmeti’ne geçmek istiyorsanız, kodunuzu API’lerdeki farklılıkları işleyecek şekilde değiştirmeniz gerekir.

Azure SignalR Hizmeti ile, ASP.NET Core SignalR’nin sunucu tarafı bileşeni Azure’da barındırılır. Ancak, teknoloji ASP.NET Core temel alınarak oluşturulduğu için gerçek web uygulamanızı birden fazla platform (Windows, Linux ve MacOS) üzerinde çalıştırırken [Azure App Service](../app-service/overview.md), [IIS](https://docs.microsoft.com/aspnet/core/host-and-deploy/iis/index), [Nginx](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-nginx), [Apache](https://docs.microsoft.com/aspnet/core/host-and-deploy/linux-apache), [Docker](https://docs.microsoft.com/aspnet/core/host-and-deploy/docker/index) ile barındırabilirsiniz. Ayrıca, işleminizin içinde kendi kendine barındırmayı da kullanabilirsiniz.

Uygulamanızın amaçları arasında web istemcilerine gerçek zamanlı içerik güncelleştirmeleri uygulamak için en son işlevleri desteklemek, birden fazla platform üzerinde çalışmak (Azure, Windows, Linux ve MacOS) ve farklı ortamlarda barındırmak varsa, Azure SignalR Hizmetinden yararlanmak en iyi seçim olacaktır.

## <a name="why-not-deploy-signalr-myself"></a>SignalR’yi neden kendim dağıtamıyorum?

Tüm web uygulamanıza arka uç bileşeni olarak ASP.NET Core SignalR’yi destekleyen kendi Azure web uygulamanızı dağıtmak hala geçerli bir yaklaşımdır.

Azure SignalR Hizmeti’ni kullanmanın başlıca nedenlerinden biri kolaylık sağlamasıdır. Azure SignalR Hizmeti ile performans, ölçeklenebilirlik ve kullanılabilirlik gibi sorunları ele almanız gerekmez. Bu sorunlar % 99,9 hizmet düzeyi sözleşmesi ile sizin yerinize ele alınır.

Ayrıca, gerçek zamanlı içerik güncelleştirmelerini desteklemek için WebSockets genellikle tercih edilen yöntemdir. Bununla birlikte, çok sayıda kalıcı WebSocket bağlantısı ile yük dengelemesi yapmak, ölçeği artırdıkça çözülmesi karmaşık bir sorun haline gelir. Yaygın çözümleri yararlanın: DNS Yük Dengeleme, donanım yük dengeleyicileri ve yazılım Yük Dengelemesi. Azure SignalR Hizmeti bu sorunu sizin için halleder.

Geçerli olabilecek başka bir neden ise gerçekten bir web uygulaması barındırmak zorunda olmamanızdır. Web uygulamanızın mantığı, [Sunucusuz bilgi işlem](https://azure.microsoft.com/overview/serverless-computing/)’den yararlanabilir. Örneğin, kodunuz yalnızca [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/) tetikleyicileri ile isteğe bağlı olarak barındırılıyor ve yürütülüyor olabilir. Kodunuz yalnızca isteğe bağlı olarak çalıştığından ve istemcilerle uzun bağlantılar sürdürmediğinden bu senaryo aldatıcı olabilir. Azure SignalR Hizmeti, bağlantıları zaten yönettiği için bu durumu sizin yerinize çözebilir. Daha fazla ayrıntı için bkz. [SignalR Hizmetini Azure İşlevleri ile kullanmaya genel bakış](signalr-concept-azure-functions.md).

## <a name="how-does-it-scale"></a>Nasıl ölçeklendirilir?

SignalR ile SQL Server, Azure Service Bus veya Azure önbelleği için Redis ölçeklendirmek için yaygındır. Azure SignalR Hizmeti, ölçeklendirme yaklaşımını sizin yerinize ele alır. Performans ve maliyet, bu yaklaşımlarla benzerdir ve diğer hizmetlerdeki gibi karmaşık değildir. Tüm yapmanız gereken, hizmetiniz için birim sayısını güncelleştirmektir. Her birim en fazla 1000 istemci bağlantısını destekler.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: Azure SignalR ile sohbet odası oluşturamadı](signalr-quickstart-dotnet-core.md)