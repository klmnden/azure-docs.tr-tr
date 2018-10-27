---
title: Azure Application Insights Profiler ayarlar dikey penceresi | Microsoft Docs
description: Profil Oluşturucu durumunu görmek ve oturumları profilini oluşturmaya başla
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.reviewer: cawa
ms.date: 08/06/2018
ms.author: mbullwin
ms.openlocfilehash: 81608dd7281ceddce7e0701535ad99e1c9e44315
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142507"
---
# <a name="configure-application-insights-profiler"></a>Application Insights Profiler'ı Yapılandır

## <a name="profiler-settings-page"></a>Profiler Ayarları sayfası

Profil Oluşturucu Ayarları sayfasında Application Insights performans sayfasından tuşlarına basarak açılabilir **Profiler** düğmesi.

![Profil Oluşturucu bölmesi girişinin yapılandırın][configure-profiler-entry]

Application Insights Profiler ' ı yapılandırma sayfası dört özellikleri içerir: 
1. **Artık profil** -bu düğmeye tıklandığında Application Insights'ın bu örneğine bağlı olan tüm uygulamalar için başlatmak profil oluşturma oturumları neden olur
1. **Bağlı uygulamalar** -profil oluşturucu bu Application Insights kaynağına gönderme uygulamaların listesi
1. **Devam eden oturumları** - bastığınızda **profili artık**, oturum durumu burada görüntülenir)
1. **Son oturumlar profil oluşturma** -geçmiş profil oluşturma oturumları hakkındaki bilgileri gösterir.

![Profiler isteğe bağlı][profiler-on-demand]

## <a name="app-service-environments-ase"></a>App Service ortamları (ASE)
ASE'NİZİN nasıl yapılandırıldığına bağlı olarak, aracı durumunu denetlemek için çağrı engellenmiş olabilir. Bu sayfa, aracının aslında onu olduğunda çalışmadığından emin yazar. Webjob emin olmak için uygulamanızı denetleyebilirsiniz. Ancak, tüm uygulama ayarlarının doğru belirlendiğinden ve App Insights site uzantısını uygulama yüklü değilse, profil oluşturucuyu çalışan ve uygulamanız için yeterli bir trafik varsa son profil oluşturma oturum listesinde görmeniz gerekir.

## <a id="profileondemand"></a> Profiler el ile tetikleme

Profiler el ile bir düğme tıklatma ile tetiklenebilir. Web performans testi çalıştırdığınızı varsayın. Web uygulamanızı yük altında nasıl performans gösterdiğini anlamak için izlemeleri gerekir. Yük testi çalıştırma, ancak bu rastgele bir örnekleme aralığı kaçırabilirsiniz bildiğiniz izlemeleri olduğunda yakalandığını denetime sahip olmak önemlidir.
Aşağıdaki adımlar, bu senaryoda nasıl çalıştığını göstermektedir:

### <a name="optional-step-1-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>(İsteğe bağlı) 1. adım: web performans testi başlatarak trafiğin web uygulamanıza oluşturur.

Web uygulamanıza gelen trafik varsa veya trafiği el ile oluşturmak istiyorsanız, bu bölümü atlayın ve adım 2'ye geçin.

Application Insights portalına gidin **yapılandırma > performans testi**. Yeni performans testi başlatmak için yeni düğmesine tıklayın.

![Yeni performans testi oluşturma][create-performance-test]

İçinde **yeni performans testi** bölmesinde test hedef URL'yi yapılandırın. Tüm varsayılan ayarları kabul edin ve yük testi çalıştırmaya başlayın.

![Yük testi Yapılandır][configure-performance-test]

Yeni test durumu 'Sürüyor' izleyen ilk olarak sıraya alınır görürsünüz.

![Yük testi gönderildi ve kuyruğa alındı][load-test-queued]

![devam eden yük testi çalışırken][load-test-in-progress]

### <a name="step-2-start-profiler-on-demand"></a>2. adım: Profil Oluşturucu üzerine Başlat

Yük testi çalışmaya başladığında, biz yük alırken web uygulamasında izlemeleri yakalamak amacıyla profil oluşturucu başlayabilirsiniz.
Yapılandırma Profiler bölmesine gidin:


### <a name="step-3-view-traces"></a>3. adım: Görünüm izlemeleri

Profil Oluşturucu çalışmayı tamamladıktan sonra performans sayfası ve görünüm izlemeleri gitmek için bildirim yönergeleri izleyin.

## <a name="troubleshooting-on-demand-profiler"></a>İsteğe bağlı profil oluşturucu sorunlarını giderme

Bazen bir isteğe bağlı oturum sonra Profiler zaman aşımı hata iletisini görebilirsiniz:

![Profiler zaman aşımı hatası][profiler-timeout]

Bu hatayı neden gördüğünüz iki nedeni olabilir:

1. İsteğe bağlı Profil Oluşturucu oturumu başarılı oldu ancak Application Insights toplanan verileri işlemek için uzun sürdü. Verileri 15 dakika içinde işlenmekte olan bitmedi, portal bir zaman aşımı iletisi görüntüler. Bir süre sonra ancak Profiler izlemeleri gösterilir. Yalnızca bu durumda, hata iletisi şimdilik yoksayın. Etkin bir düzeltme üzerinde çalışıyoruz.

1. Web uygulamanızın isteğe bağlı özellik yok Profiler Aracısı daha eski bir sürümü vardır. Application Insights profilinde önceden etkinleştirilmişse, isteğe bağlı özelliği kullanmaya başlamak için Profiler aracınızı güncelleştirmeye gerek duyduğunuz yüksektir.
  
En son Profiler'ı yüklemek ve denetlemek için aşağıdaki adımları izleyin:

1. Uygulama Hizmetleri uygulama ayarlarına gidin ve aşağıdaki ayarları ayarlanıp ayarlanmadığını denetleyin:
    * **Appınsıghts_ınstrumentatıonkey**: Application Insights için uygun bir izleme anahtarı ile değiştirin.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0 Bu ayarlardan herhangi birini ayarlanmazsa, en son site uzantısını yüklemek için Application ınsights'ı etkinleştirme bölmesine gidin.

1. Uygulama Hizmetleri Portalı'nda Application Insights bölmesine gidin.

    ![Uygulama Hizmetleri portalından Application Insights'ı etkinleştir][enable-app-insights]

1. Aşağıdaki sayfada bir 'Güncelleştirme' düğmesi görürseniz, en son Profiler Aracısı yükleyecek Application Insights site uzantısını güncelleştirmek için tıklayın.

    ![Güncelleştirme site uzantısı][update-site-extension]

1. Ardından **değiştirme** emin olmak için Profiler select açık olduğundan ve **Tamam** değişiklikleri kaydetmek için.

    ![App ınsights kaydedin ve değiştirme][change-and-save-appinsights]

1. Geri Git **uygulama ayarları** App Service uygulama ayarları aşağıdakileri denetleyin sekmesinde ayarlanır:
    * **Appınsıghts_ınstrumentatıonkey**: application ınsights için uygun bir izleme anahtarı ile değiştirin.
    * **APPINSIGHTS_PORTALINFO**: ASP.NET
    * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

    ![Profil Oluşturucu için uygulama ayarları][app-settings-for-profiler]

1. İsteğe bağlı olarak, uzantı sürümü ve kullanılabilir güncelleştirme yok sağlamaktan denetleyin.

    ![Uzantı güncelleştirme denetleme][check-for-extension-update]

## <a name="next-steps"></a>Sonraki Adımlar
[Profiler etkinleştirme ve görüntüleme izlemeleri](app-insights-profiler-overview.md?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/app-insights-profiler/Profiler-on-demand.png
[configure-profiler-entry]: ./media/app-insights-profiler/configure-profiler-entry.png
[create-performance-test]: ./media/app-insights-profiler/new-performance-test.png
[configure-performance-test]: ./media/app-insights-profiler/configure-performance-test.png
[load-test-queued]: ./media/app-insights-profiler/load-test-queued.png
[load-test-in-progress]: ./media/app-insights-profiler/load-test-inprogress.png
[enable-app-insights]: ./media/app-insights-profiler/enable-app-insights-blade-01.png
[update-site-extension]: ./media/app-insights-profiler/update-site-extension-01.png
[change-and-save-appinsights]: ./media/app-insights-profiler/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/app-insights-profiler/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/app-insights-profiler/check-extension-update-01.png
[profiler-timeout]: ./media/app-insights-profiler/profiler-timeout.png