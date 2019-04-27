---
title: Tanılama genel bakış - Azure App Service | Microsoft Docs
description: Web uygulamanızı App Service tanılamasını ile sorunları nasıl giderebileceğinizi öğrenin.
keywords: App service, azure app service, tanılama, destek, web uygulaması, sorun giderme, kendi kendine yardım
services: app-service
documentationcenter: ''
author: jen7714
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: jennile
ms.custom: seodec18
ms.openlocfilehash: a8b256f43d8e4103404ab4276431ceb06d9ed36a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60839445"
---
# <a name="azure-app-service-diagnostics-overview"></a>Azure App Service tanılama genel bakış 

Bir web uygulaması çalıştırırken, sitenizin kapalı olması, kullanıcılara bildiren 500 hataları kaynaklanabilecek sorunları için hazırlıklı olmak istiyorsunuz. App Service tanılamasını yapılandırma gerektirmeden web uygulamanızı gidermenize yardımcı olması için akıllı ve etkileşimli bir deneyimdir. Web uygulamanızla bir sorunla karşılaşırsanız çalıştırdığınızda, App Service tanılamasını daha kolay ve hızlı bir şekilde ve sorunu gidermek için doğru bilgileri size yol göstermesi sorunun ne olduğunu gösterir. 
 
Son 24 saat içinde web uygulamanızla sorunları yaşıyorsanız, bir bu deneyim en yararlı olsa da, tüm tanılama grafikler, her zaman analiz etmek kullanılabilir. Ek sorun giderme araçları ve yardımcı belgeleri ve forumlar için bağlantılar sağ sütunda yer alır.

App Service tanılamasını çalışır yalnızca, uygulama Windows üzerinde ancak uygulamalar için de [Linux/kapsayıcılar](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro), [App Service ortamı](https://docs.microsoft.com/azure/app-service/environment/intro), ve [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview). 

## <a name="open-app-service-diagnostics"></a>App Service tanılamasını Aç

App Service tanılamasını erişmek için App Service uygulamanızı veya App Service Ortamı'nda gidin [Azure portalında](https://portal.azure.com). Sol gezinti bölmesinde, tıklayarak **tanılayın ve sorunlarını çözmek**. 

Azure işlevleri için işlev uygulamanıza ve üst gezinti gidin, tıklayarak **Platform özellikleri** seçip **Tanıla ve problemleri çözmenize** gelen **izleme**bölümü. 

![Giriş sayfası](./media/app-service-diagnostics/Homepage1.png)

## <a name="health-checkup"></a>Sistem durumu kontrolü

Web uygulamanızı nerede olduğunu bilmiyorsanız veya sorunlarınızı giderme araştırmaya nereden başlayacağınızı bilmiyorsanız, sistem durumu kontrolü başlatmak için iyi bir yerdir. Sistem durumu kontrolü size sağlıklı nedir ve sorunu araştırmak için aranacağı belirten, sorunun ne olduğunu işaret eden bir hızlı, etkileşimli genel bakış sağlayacak, web uygulamalarınızın analiz eder. Akıllı ve etkileşimli arabirimi, sorun giderme işlemi boyunca rehberlik sağlar.  

![Sistem durumu kontrolü](./media/app-service-diagnostics/HealthCheckup2.png)

Bir sorun, son 24 saat içinde belirli bir sorun kategorisi ile algılanırsa, tam tanılama raporu görüntüleyebilir ve daha fazla sorun giderme tavsiyeleri ve daha destekli bir deneyim için sonraki adımları görüntülemek için App Service tanılamasını isteyebilir.

![Sorun giderme ve sonraki adımlar](./media/app-service-diagnostics/Troubleshooting3.png)

## <a name="tile-shortcuts"></a>Kutucuk kısayolları

Aradığınız bilgiler sorun giderme ne tür tam olarak biliyorsanız, kutucuk kısayolları doğrudan ilgilendiğiniz sorun kategorisi tam tanılama raporu yönlendirilirsiniz. İçin sistem durumu kontrolü karşılaştırıldığında daha fazla doğrudan kutucuk kısayollarıdır, ancak daha az destekli tanılama ölçümlerinizi erişme yolu. Kutucuk kısayolları bir parçası olarak da burada bulabilirsiniz budur **tanılama araçları** hangi daha gelişmiş uygulama kod sorunlarını, yavaşlık, bağlantı dizeleri ve daha fazla bilgi için ilgili sorunları araştırmanıza yardımcı olacak araçlar. 

![Kutucuk kısayolları](./media/app-service-diagnostics/TileShortcuts4.png)

## <a name="diagnostic-report"></a>Tanılama raporu

Çalıştırdıktan sonra daha fazla bilgi isteyip istemediğiniz bir [sistem durumu kontrolü](#health-checkup) veya üzerine tıkladığınız [kutucuğuna kısayolları](#tile-shortcuts), son 24 saat oluşturabilme ilgili ölçümleri tam tanılama raporu gösterilir. Uygulamanızı kapalı kalma süresi karşılaşırsa, zaman çizelgesi alttaki turuncu bir çubuk gösterilir. Bu kapalı kalma süresi ve önerilen sorun giderme adımları hakkında daha fazla gözlem görmek için kapalı kalma süresi seçmek için turuncu çubuklardan birine seçebilirsiniz. 

![Tanılama raporu](./media/app-service-diagnostics/DiagnosticReport5.png)


## <a name="investigating-application-code-issues"></a>Uygulama kodu sorunlarını araştırma

Birçok uygulama sorunları için uygulama kodunuzdaki sorunları birbiriyle ilişkili olduğundan, App Service tanılamasını ile tümleşir [Application Insights](https://azure.microsoft.com/services/application-insights/) özel durumları ve seçili kapalı kalma süresi ile ilişkilendirmek için bağımlılık sorunlarını vurgulayın. Application Insights ayrı olarak etkinleştirilmesi gerekir. 

Application Insights özel durumları ve bağımlılıklarını görüntülemek için seçin **Web uygulaması kapalı** veya **Web uygulaması yavaş** kutucuğuna kısayolları. 

![Application ınsights](./media/app-service-diagnostics/AppInsights6.png)

