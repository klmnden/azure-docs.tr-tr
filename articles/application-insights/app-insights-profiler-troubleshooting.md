---
title: Azure Application Insights Profiler ile ilgili sorunları giderme | Microsoft Docs
description: Bu sayfa, sorun giderme adımları ve etkinleştirme veya Application Insights profiler'ı kullanarak sorun geliştiriciler sağlayacak bilgiler içerir.
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
ms.openlocfilehash: 6013c0a1b404336ad7cca21edafb7adec5c7f7ca
ms.sourcegitcommit: 1fc949dab883453ac960e02d882e613806fabe6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2018
ms.locfileid: "50978851"
---
# <a name="troubleshoot-problems-enabling-or-viewing-application-insights-profiler"></a>Etkinleştirme veya Application Insights Profiler ' ı görüntüleme sorunlarını giderme

## <a id="troubleshooting"></a>Genel sorun giderme

### <a name="profiles-are-only-uploaded-if-there-are-requests-to-your-application-while-the-profiler-is-running"></a>İsteği yoksa uygulamanız için profil oluşturucu çalıştırılırken profilleri yalnızca yüklenir
Application Insights Profiler, iki dakika her saat veya zaman profil oluşturucu verileri toplar [ **profili artık** ](app-insights-profiler-settings.md?toc=/azure/azure-monitor/toc.json) düğmesine basıldığında **yapılandırma Application Insights Profiler**sayfası. Ancak, profil oluşturucu çalışırken gerçekleşen bir isteğine eklenebilecek yaptığınızda profil oluşturma verilerinin yalnızca yüklenir. 

Profil Oluşturucu, application ınsights kaynağınızın izleme iletileri ve özel olaylar yazar. Profil oluşturucuyu çalışan nasıl görmek için bu olayları kullanabilirsiniz. Profil oluşturucuyu çalışan verilecek ve izlemeleri yakalama, ancak performans sayfasında gönderdiklerimizi düşünüyorsanız, profil oluşturucuyu çalışan nasıl kontrol edebilirsiniz:

1. İzleme iletileri ve Application Insights kaynağınıza Profil Oluşturucu tarafından gönderilen özel olaylar arayın. Bu arama dizesi, ilgili verileri bulmak için kullanabilirsiniz:

    ```
    stopprofiler OR startprofiler OR upload OR ServiceProfilerSample
    ```
    Burada, aşağıdaki ekran görüntüsünde iki farklı AI kaynak iki aramalardan bir örnek verilmiştir. 
    
    * Profil Oluşturucu çalıştırılırken isteklerinin alınması olmayan bir uygulamaya ait sol taraftaki paroladır. Karşıya yükleme hiçbir etkinlik nedeniyle iptal edildi iletisi görebilirsiniz. 

    * Sağ taraftaki örnekte, profil oluşturucu başlatıldı ve profil oluşturucu çalışırken gerçekleşen istekleri algılandığında özel olaylar gönderilen görebilirsiniz. ServiceProfilerSample özel olay görürseniz, profil oluşturucu izleme bir isteğe bağlı ve izleme Application Insights performans sayfasından görüntüleyebilirsiniz anlamına gelir.

    * Tüm telemetri hiç görmüyorsanız, profil oluşturucu çalışmıyor. Bu belgede aşağıdaki belirli uygulama türüne yönelik sorun giderme bölümlerini okumak gerekebilir.  

     ![Profiler Telemetri arama][profiler-search-telemetry]

1. Dönem boyunca isteği yoksa profil oluşturucuyu çalışan, profil oluşturucu etkin olan, bir uygulama bölümü tarafından işlenen istekleri emin olun. Bazen uygulamaları birden çok bileşenden oluşur ancak Profiler yalnızca bazıları için değil, bileşenlerin tümünü etkindir. Application Insights Profiler ' ı yapılandırma sayfası izlemeleri karşıya yüklediğiniz bileşenleri gösterir.

### <a name="net-core-21-bug"></a>.Net Core 2.1 hata
İzlemeleri ASP.NET Core 2.1 üzerinde çalışan uygulamalardan alınan karşıya yükleme engelleyen Profil Oluşturucu aracı bir hata yoktur. Biz bir düzeltme üzerinde çalışıyoruz ve sahip olur, hazır olan en kısa sürede. Bu hata düzeltmesini Ekim sonuna dağıtılır.

### <a name="other-things-to-check"></a>Denetlenecek başka şeyler:
* Uygulamanızı .NET Framework 4.6 üzerinde çalışıyor.
* Web uygulamanız için bir ASP.NET Core uygulaması ise, en az çalıştırmalıdır ASP.NET Core 2.0.
* Görüntülemeye çalıştığınız veriler birkaç haftadır eskiyse, zaman filtrenizi görüntülemeyi deneyin ve yeniden deneyin. İzlemeleri yedi gün sonra silinir.
* Olun proxy'leri veya bir güvenlik duvarı olması erişimi engelledi https://gateway.azureserviceprofiler.net.

### <a id="double-counting"></a>Çift paralel iş parçacığı sayımı

Bazı durumlarda, toplam süre ölçümü yığın Görüntüleyicisi'nde istek süresi büyük.

İki veya daha fazla iş parçacığı bir istekle ilişkilendirilen ve paralel olarak çalışan bu durum oluşabilir. Bu durumda, toplam iş parçacığı geçen süre uzun süredir. Bir iş parçacığından diğerine tamamlanması bekliyor olabilir. Görüntüleyici bu durumu algılayabilir dener ve sizi ilgilendirmeyen bekleme atlar, ancak çok fazla bilgileri görüntüleme kenarındaki errs yerine atlamak ne kritik bilgiler olabilir.

Paralel iş parçacıkları, izlemelerinde gördüğünüzde, istek için kritik yolu olmadığından emin olmak için hangi iş parçacıkları bekleyen belirleyin. Çoğu durumda, iş parçacığı hızlı bir şekilde bir bekleme durumuna geçtiğinde yalnızca diğer iş parçacıkları üzerinde bekliyor. Diğer iş parçacıkları hakkında yoğunlaşabilirsiniz ve bekleyen iş parçacıklarının sürede yoksay.

### <a name="error-report-in-the-profile-viewer"></a>Profil Görüntüleyicisi'nde hata raporu
Portalında bir destek bileti gönderin. Hata iletisindeki bağıntı Kimliğini eklediğinizden emin olun.

## <a name="troubleshooting-profiler-on-app-services"></a>Profiler uygulama hizmetlerinde sorunlarını giderme
### <a name="for-the-profiler-to-work-properly"></a>Düzgün çalışması profil oluşturucu için:
* Temel katman web app service planınızın olması gerekir veya üzeri.
* Web uygulamanızı uygulama Hizmetleri (2.6.5) Application Insights uzantısının yüklü olması gerekir.
* Web uygulamanızı olmalıdır **appınsıghts_ınstrumentatıonkey** Application Insights SDK'sı tarafından kullanılan aynı izleme anahtarı ile yapılandırılmış uygulama ayarı.
* Web uygulamanızı olmalıdır **APPINSIGHTS_PROFILERFEATURE_VERSION** uygulama ayarı tanımlanan ve 1.0.0 sürümüne ayarlayın.
* **ApplicationInsightsProfiler2** web işi çalışıyor olmalıdır. Web işi giderek denetleyebilirsiniz [Kudu](https://blogs.msdn.microsoft.com/cdndevs/2015/04/01/the-kudu-debug-console-azure-websites-best-kept-secret/)ve açma **Web işleri Panosu'nu** Araçlar menüsü altına. Aşağıdaki ekran görüntülerinde ApplicationInsightsProfiler2 bağlantıya tıklayarak görebileceğiniz gibi günlük'dahil olmak üzere Web işi ayrıntıları görebilirsiniz.

    Webjob ayrıntıları görmek için'ye tıklamanız bağlantı şu şekildedir: 

    ![Profil Oluşturucu webjob]

    İşte ayrıntılarını gösteren sayfası. Günlüğü indir ve neden bir profil oluşturucu sizin için çalışmayan şekil olamaz, ekibimize gönderin.
    
    ![Profil Oluşturucu webjob günlüğü]

### <a name="manual-installation"></a>El ile yükleme

Profiler'ı yapılandırırken, güncelleştirmeler web uygulamasının Ayarlar hale getirilir. Ortamınızı gerektiriyorsa, güncelleştirmelerin el ile uygulayabilirsiniz. Örnek uygulamanız PowerApps için Web Apps ortamda çalışıyor olabilir.

1. İçinde **Web uygulaması denetimi** bölmesi açık **ayarları**.
1. Ayarlama **.Net Framework sürümü** için **v4.6**.
1. Ayarlama **her zaman açık** için **üzerinde**.
1. Ekleme **appınsıghts_ınstrumentatıonkey** uygulama ayarı ve değeri SDK'sı tarafından kullanılan aynı izleme anahtarını ayarlayın.
1. Ekleme **APPINSIGHTS_PROFILERFEATURE_VERSION** uygulama ayarı ve 1.0.0 değeri ayarlayın.
1. Açık **Gelişmiş Araçlar**.
1. Seçin **Git** Kudu Web sitesini açın.
1. Kudu Web sitesinde seçin **Site uzantıları**.
1. Yükleme **Application Insights** Azure Web uygulamaları Galerisi.
1. Web uygulamasını yeniden başlatın.

### <a name="too-many-active-profiling-sessions"></a>Profil oluşturma çok fazla etkin oturumlar

Şu anda en fazla dört Azure web uygulamaları ve aynı hizmet planında çalıştırdığınız dağıtım yuvalarını Profiler etkinleştirebilirsiniz. Daha fazla web uygulaması, bir app service planında çalıştırmayı varsa, Profil Oluşturucu tarafından oluşturulan bir Microsoft.ServiceProfiler.Exceptions.TooManyETWSessionException görebilirsiniz. Profil Oluşturucu, her web uygulaması için ayrı olarak çalışır ve her uygulama için bir ETW oturumu başlatmak çalışır. Ancak aynı anda etkin olabilen ETW oturumlarına sınırlı sayıda vardır. Profiler web işi profil oluşturma çok fazla etkin oturum yapıyorsa, bazı web uygulamaları, farklı hizmet planına taşıyın.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Dağıtım hatası: Dizin boş değil ' D:\\giriş\\site\\wwwroot\\App_Data\\işlerin

Profiler etkin ile web uygulamanıza Web Apps kaynak dağıtıyorsanız, şu iletiyi görebilirsiniz:

*Dizin boş değil ' D:\\giriş\\site\\wwwroot\\App_Data\\işlerin*

Web dağıtımı komut dosyaları veya Azure DevOps dağıtım işlem hattı çalıştırırsanız, bu hata oluşur. Web dağıtımı görevi aşağıdaki ek dağıtım parametreleri eklemek için bir çözümdür:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Bu parametreler, Application Insights Profiler ' ı tarafından kullanılan ve yeniden dağıtma işlemi engellemesini klasörü silin. Bunlar şu anda çalışıyor Profiler örneği etkilemez.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Application Insights Profiler azalmadığını nasıl belirlerim?

Profiler web uygulamasında sürekli web işi çalıştırır. Web uygulaması kaynak açabileceğiniz [Azure portalında](https://portal.azure.com). İçinde **WebJobs** bölmesinde, durumunu **ApplicationInsightsProfiler**. Çalışmıyorsa, açık **günlükleri** daha fazla bilgi için.

## <a name="troubleshooting-problems-with-profiler-and-wad"></a>Profiler ve WAD ile ilgili sorunları giderme

Profil Oluşturucu tarafından WAD doğru şekilde yapılandırıldığını kontrol etmek için üç nokta vardır. İlk olarak, dağıtılan içeriği WAD yapılandırmasının beklediğiniz olduğunu denetlemek gerekir. İkinci olarak, WAD uygun iKey profil oluşturucu komut satırında geçirir denetlemek gerekir. Üçüncü olarak, profil oluşturucu çalıştı, ancak alınırken bir hata görmek için profil oluşturucu günlük dosyasını kontrol edebilirsiniz. 

WAD yapılandırmak için kullanılan ayarları denetlemek için VM'de oturum açın ve bu konumdaki günlük dosyasını açmak gerekir: 
```
c:\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.logs  
```
Bu dosyanın "WadCfg" dize için arama yapabilirsiniz ve WAD yapılandırmak için VM geçirilen ayarları göreceksiniz. Profil Oluşturucu havuzu tarafından kullanılan, iKey doğru olduğunu kontrol edebilirsiniz.

İkinci olarak, profil oluşturucuyu başlatmak için kullanılan komut satırı kontrol edebilirsiniz. Aşağıdaki dosya, profil oluşturucuyu başlatmak için kullanılan bağımsız değişkenler içerir.
```
D:\ProgramData\ApplicationInsightsProfiler\config.json
```
Profil oluşturucu komut satırında ikey doğru olduğundan emin olun. 

Üçüncü olarak, yukarıdaki config.json dosyasında bulunan yolunu kullanarak, profil oluşturucu günlük dosyasını denetleyin. Profil Oluşturucu gelen durum ve hata iletileri ve profil oluşturucuyu kullanarak ayarlarını belirten hata ayıklama bilgilerini gösterecektir. Profil Oluşturucu, uygulamanızın istekleri alırken çalışıyorsa, bu iletiyi görürsünüz: etkinlik iKey algılandı. İzleme karşıya yüklendiğinde, bu iletiyi görürsünüz: izleme yüklenmeye başlıyor. 


[profiler-search-telemetry]:./media/app-insights-profiler/Profiler-Search-Telemetry.png
[Profil Oluşturucu webjob]:./media/app-insights-profiler/Profiler-webjob.png
[Profil Oluşturucu webjob günlüğü]:./media/app-insights-profiler/Profiler-webjob-log.png








