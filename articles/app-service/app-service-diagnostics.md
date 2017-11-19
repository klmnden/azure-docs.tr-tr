---
title: "Azure uygulama hizmeti tanılama genel bakış | Microsoft Docs"
description: "Uygulama hizmeti Tanılama, web uygulamanızı ile sorunları nasıl giderebileceğinizi öğrenin."
keywords: "uygulama hizmeti, azure app service, tanılama, destek, web uygulaması, sorun giderme, kendi kendine yardım"
services: app-service
documentationcenter: 
author: jen7714
manager: cfowler
editor: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: jennile
ms.openlocfilehash: f027e7fbc5866a85e7f55460192a1c99a71e368e
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="azure-app-service-diagnostics-overview"></a>Azure uygulama hizmeti tanılama genel bakış 

Bir web uygulaması çalıştırırken, sitenizi çalışmıyor, kullanıcılarınıza söyleyen 500 hataları ortaya çıkabilecek sorunlar için hazırlanması istiyor. Uygulama hizmeti Tanılama, web uygulamanızın yapılandırma gerektirmeden gidermenize yardımcı olması için akıllı ve etkileşimli bir deneyimdir. Web uygulamanızı ile sorunla çalıştırdığınızda, uygulama hizmeti tanılama daha kolay ve hızlı bir şekilde ve sorunu gidermek için doğru bilgileri göstermesi yanlış gelin. 
 
Son 24 saat içinde web uygulamanızı ile sorunları karşılaştığınızda bir bu deneyim en yararlı olsa da, tüm tanılama grafikleri, her zaman çözümlemek kullanılabilir. Ek sorun giderme araçları ve yardımcı belgelerine ve forumlar bağlantılar sağ taraftaki sütunda yer alır.

![Giriş sayfası](./media/app-service-diagnostics/Homepage1.png)

## <a name="health-checkup"></a>Sistem durumu checkup

Web uygulamanızı ile sorunun ne olduğunu bilmiyorsanız veya sorunlarını gidermek için başlangıç noktası bilmiyorsanız, sistem durumu checkup başlatmak için uygun bir yerdir. Sistem durumu checkup sağlıklı nedir ve sorunu araştırmak için nereye bakması söyleyen sorunun ne olduğunu öğrenmek işaret eden bir hızlı, etkileşimli genel bakış vermek için web uygulamalarınızı analiz eder. Akıllı ve etkileşimli arabirimiyle sorun giderme işlemi boyunca rehberlik sağlar.  

![Sistem durumu checkup](./media/app-service-diagnostics/HealthCheckup2.png)

Son 24 saat içinde belirli bir sorun kategorisi ile ilgili bir sorun algılanırsa, daha fazla sorun giderme öneriler ve sonraki adımlar daha Kılavuzlu bir deneyim görüntülemek için uygulama hizmeti tanılama isteyebilir ve tam tanılama raporu görüntüleyebilir.

![Sorun giderme ve sonraki adımlar](./media/app-service-diagnostics/Troubleshooting3.png)

## <a name="tile-shortcuts"></a>Döşeme kısayolları

Sorun giderme bilgileri için arıyorsanız, ne tür tam olarak biliyorsanız, döşeme kısayolları, ilgilendiğiniz sorun kategorisi tam tanılama raporu için doğrudan yönlendirir. Sistem durumu checkup ile karşılaştırıldığında, daha fazla doğrudan döşeme kısayolları bağlıdır, ancak daha az tanılama ölçümlerinizi erişme yolu destekli.  

![Döşeme kısayolları](./media/app-service-diagnostics/TileShortcuts4.png)

## <a name="diagnostic-report"></a>Tanılama raporu

Daha fazla bilgi çalıştırdıktan sonra isteyip istemediğiniz bir [sistem durumu checkup](#health-checkup) veya biri üzerinde tıkladığınız [döşeme kısayolları](#tile-shortcuts), tam tanılama raporu son 24 saat ilgili grafiği çıkarılan ölçümleri gösterir. Uygulamanızı herhangi kesinti oluşursa, zaman çizelgesi altındaki turuncu çubuk olarak temsil edilir. Kapalı kalma süresi ve önerilen çözümler hakkında çözümlenen gözlemleri almak için kapalı kalma süreleri birini seçebilirsiniz. 

![Tanılama raporu](./media/app-service-diagnostics/DiagnosticReport5.png)

## <a name="open-app-service-diagnostics"></a>Açık uygulama hizmeti tanılama

Uygulama hizmeti tanılama erişmek için App Service web uygulamanızda gidin [Azure portal](https://portal.azure.com). 

Sol gezinti bölmesinde tıklayın **Tanıla ve sorunları**.