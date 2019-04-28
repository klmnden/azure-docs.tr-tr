---
title: Azure Application Insights Profiler ayarlar bölmesini kullanın | Microsoft Docs
description: Profiler durumunu görmek ve oturumları profilini oluşturmaya başla
services: application-insights
documentationcenter: ''
author: cweining
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 08/06/2018
ms.author: cweining
ms.openlocfilehash: 9603c45443c6339a127f977600eeff2ba57a283f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61228244"
---
# <a name="configure-application-insights-profiler"></a>Application Insights Profiler'ı Yapılandır

## <a name="profiler-settings-pane"></a>Profiler ayarları bölmesi

Azure Application Insights Profiler ayarlar bölmesini açmak için Application Insights performans bölmesine gidin ve ardından **Profiler** düğmesi.

![Profiler bölmesinde yapılandırın][configure-profiler-entry]

**Application Insights Profiler ' ı yapılandırma** bölmesi, dört özellikleri içerir: 
* **Artık profil**: Oturumlarının Application Insights'ın bu örneğine bağlı olan tüm uygulamalar için profil oluşturma başlar.
* **Bağlı uygulamalar**: Profil oluşturma gönderen uygulamayı listeler bu Application Insights kaynağına veri.
* **Devam eden oturumları**: Seçtiğinizde, oturum durumunu görüntüler **profili artık**. 
* **Son oturumlar profil oluşturma**: Geçmiş profil oluşturma oturumları hakkındaki bilgileri görüntüler.

![Profiler isteğe bağlı][profiler-on-demand]

## <a name="app-service-environment"></a>App Service Ortamı
Azure App Service ortamınızı nasıl yapılandırıldığına bağlı olarak, aracı durumunu denetlemek için çağrı engellenebilir. Bölmesinde bile çalışırken aracı çalışmayan bir ileti görüntülenebilir. Bunun olduğundan emin olmak için uygulamanızın üzerinde webjob'ı denetleyin. Uygulama ayarları değerlerin doğru olduğundan ve Application Insights site uzantısını uygulama yüklü değilse Profiler çalışıyor. Son profil oluşturma oturumları uygulamanızı yeterli trafik alıyorsa, listede görüntülenmesi gerekir.

## <a id="profileondemand"></a> Profiler el ile tetikleme

### <a name="minimum-requirements"></a>En düşük gereksinimleri 
Profil Oluşturucu oturumu el ile tetiklemek bir kullanıcı için en azından "rolleri için Application Insights bileşen üzerinde yazma erişimi" duyarlar. Çoğu durumda, bu otomatik olarak erişin ve herhangi bir ek çalışma gerekir. İlgili sorunlar yaşıyorsanız, "Application Insights bileşeni Katılımcısı" rolünü eklemek için abonelik kapsamında rolü olacaktır. [Rol erişim denetimini Azure izleme ile ilgili daha fazla bilgi bkz](https://docs.microsoft.com/en-us/azure/azure-monitor/app/resources-roles-access-control).

Profiler tek bir tıklamayla el ile tetikleyebilirsiniz. Web performans testini çalıştırdığınız varsayalım. Web uygulamanızı yük altında nasıl performans gösterdiğini anlamak için izlemeleri gerekir. Yük testi çalıştırırken bildiğiniz izlemeleri olduğunda yakalandığını denetime sahip olmak, çok önemlidir. Ancak, rastgele bir örnekleme aralığı eksik.

Sonraki bölümlerde, bu senaryoda nasıl çalıştığını göstermektedir:

### <a name="step-1-optional-generate-traffic-to-your-web-app-by-starting-a-web-performance-test"></a>1. Adım: (İsteğe bağlı) Web performans testi başlatarak web uygulamanıza trafiği oluşturur

Web uygulamanıza gelen trafik varsa veya trafiği el ile oluşturmak istiyorsanız, bu bölümü atlayın ve adım 2'ye geçin.

1. Application Insights portalında **yapılandırma** > **performans testi**. 

1. Yeni performans testi başlatmak için **yeni** düğmesi.

   ![Yeni performans testi oluşturma][create-performance-test]

1. İçinde **yeni performans testi** bölmesinde test hedef URL'yi yapılandırın. Tüm varsayılan ayarları kabul edin ve ardından **testi** yük testi çalıştırmaya başlamak için.

    ![Yük testi Yapılandır][configure-performance-test]

    Yeni test durumu tarafından izlenen ilk olarak sıraya alınır *sürüyor*.

    ![Yük testi gönderildi ve kuyruğa alındı][load-test-queued]

    ![devam eden yük testi çalışırken][load-test-in-progress]

### <a name="step-2-start-a-profiler-on-demand-session"></a>2. Adım: Profiler isteğe bağlı oturum başlatma

1. Yük testi çalışırken yük alırken web uygulamasında izlemeleri yakalamak amacıyla Profiler'ı başlatın.

1. Git **yapılandırma Profiler** bölmesi.


### <a name="step-3-view-traces"></a>3. Adım: Görünüm izlemeleri

Profiler çalışmayı tamamladıktan sonra performans bölmesi ve görünüm izlemeleri gitmek için bildirim yönergeleri izleyin.

## <a name="troubleshoot-the-profiler-on-demand-session"></a>Profiler isteğe bağlı oturumdaki sorunlarını giderme

İsteğe bağlı oturumundan sonra bir Profiler zaman aşımı hata iletisini alabilirsiniz:

![Profiler zaman aşımı hatası][profiler-timeout]

Aşağıdaki nedenlerden biri bu hatayı alabilirsiniz:

* İsteğe bağlı Profiler oturum başarılı oldu ancak Application Insights toplanan verileri işlemek için beklenenden daha uzun sürdü.  

  Verileri 15 dakika içinde işlenen değil, portal bir zaman aşımı iletisi görüntüler. Bir süre sonra ancak Profiler izlemeleri gösterilir. Bir hata iletisi alırsanız, şimdilik yoksayın. Etkin bir düzeltme üzerinde çalışıyoruz.

* Web Apps uygulamanız isteğe bağlı özellik yok Profiler Aracısı daha eski bir sürümü var.  

  Application Insights Profiler ' ı daha önce etkin, isteğe bağlı özelliği kullanmaya başlamak için Profiler aracınızı güncelleştirin gerekebilir.
  
Uygulama Hizmetleri Git **uygulama ayarları** bölmesi ve aşağıdaki ayarları denetleyin:
* **APPINSIGHTS_INSTRUMENTATIONKEY**: Application Insights için uygun bir izleme anahtarı ile değiştirin.
* **APPINSIGHTS_PORTALINFO**: ASP.NET
* **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

Yukarıdaki değerlerden herhangi birini ayarlanmazsa, en son site uzantısı aşağıdaki adımları izleyerek yükleyin:

1. Git **Application Insights** App Services Portalı'nda bölmesi.

    ![Application Insights uygulama hizmetleri portalından etkinleştirme][enable-app-insights]

1. Varsa **Application Insights** bölmesini görüntüler bir **güncelleştirme** düğmesi, en son Profiler Aracısı yükleyecek Application Insights site uzantısını güncelleştirmek için seçin.

    ![Güncelleştirme site uzantısı][update-site-extension]

1. Profiler açık olduğundan emin olmak için seçin **değişiklik**ve ardından **Tamam** değişiklikleri kaydedin.

    ![App ınsights kaydedin ve değiştirme][change-and-save-appinsights]

1. Geri Git **uygulama ayarları** bölmesinde aşağıdaki değerleri ayarlandığından emin olmak App Service için:
   * **APPINSIGHTS_INSTRUMENTATIONKEY**: Application ınsights için uygun bir izleme anahtarı ile değiştirin.
   * **APPINSIGHTS_PORTALINFO**: ASP.NET 
   * **APPINSIGHTS_PROFILERFEATURE_VERSION**: 1.0.0

     ![Profiler için uygulama ayarları][app-settings-for-profiler]

1. İsteğe bağlı olarak **uzantıları**, uzantı sürümü kontrol edin ve ardından bir güncelleştirme kullanılabilir olup olmadığını belirlemek.

    ![Uzantı güncelleştirme denetleme][check-for-extension-update]

## <a name="next-steps"></a>Sonraki adımlar
[Profiler'ı etkinleştirme ve izlemeleri görüntüleme](profiler-overview.md?toc=/azure/azure-monitor/toc.json)

[profiler-on-demand]: ./media/profiler-settings/Profiler-on-demand.png
[configure-profiler-entry]: ./media/profiler-settings/configure-profiler-entry.png
[create-performance-test]: ./media/profiler-settings/new-performance-test.png
[configure-performance-test]: ./media/profiler-settings/configure-performance-test.png
[load-test-queued]: ./media/profiler-settings/load-test-queued.png
[load-test-in-progress]: ./media/profiler-settings/load-test-inprogress.png
[enable-app-insights]: ./media/profiler-settings/enable-app-insights-blade-01.png
[update-site-extension]: ./media/profiler-settings/update-site-extension-01.png
[change-and-save-appinsights]: ./media/profiler-settings/change-and-save-appinsights-01.png
[app-settings-for-profiler]: ./media/profiler-settings/appsettings-for-profiler-01.png
[check-for-extension-update]: ./media/profiler-settings/check-extension-update-01.png
[profiler-timeout]: ./media/profiler-settings/profiler-timeout.png
