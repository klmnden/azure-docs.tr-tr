---
title: Azure Application Insights Profiler ile ilgili sorunları giderme | Microsoft Docs
description: Bu makalede, sorun giderme adımları ve etkinleştirme veya Application Insights Profiler ' ı kullanarak sorun geliştiriciler yardımcı olacak bilgiler sunulmaktadır.
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
ms.openlocfilehash: 6b57ffbd3cb2b31da3fc2882e941f9788d83fea8
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341666"
---
# <a name="troubleshoot-problems-enabling-or-viewing-application-insights-profiler"></a>Etkinleştirme veya Application Insights Profiler ' ı görüntüleme sorunlarını giderme

## <a id="troubleshooting"></a>Genel sorun giderme

### <a name="profiles-are-uploaded-only-if-there-are-requests-to-your-application-while-profiler-is-running"></a>Yalnızca varsa, uygulamanızın isteklere Profiler çalışırken profil yüklenir

Azure Application Insights Profiler, iki dakika her saat için profil oluşturma verisi toplar. Seçeneğini belirlediğinizde ayrıca veri topladığı **profili artık** düğmesine **Application Insights Profiler ' ı yapılandırma** bölmesi. Ancak, profil oluşturma verilerinin yalnızca Profiler çalışırken gerçekleşen bir isteğine eklenebilecek yaptığınızda yüklenir. 

Profiler, Application Insights kaynağınızın izleme iletileri ve özel olaylar yazar. Profiler nasıl çalıştığı görmek için bu olayları kullanabilirsiniz. Profiler çalıştıran verilecek ve izlemeleri yakalama, ancak bunların görüntülenme değil düşünüyorsanız **performans** bölmesi, kontrol edebilirsiniz Profiler nasıl çalıştığı görmek için:

1. İzleme iletileri ve Application Insights kaynağınıza Profiler tarafından gönderilen özel olaylar arayın. Bu arama dizesi, ilgili verileri bulmak için kullanabilirsiniz:

    ```
    stopprofiler OR startprofiler OR upload OR ServiceProfilerSample
    ```
    Aşağıdaki görüntüde arama iki örnekleri iki yapay ZEKA kaynaklardan görüntüler: 
    
   * Profiler çalışırken solda, uygulama isteklerini almak değil. İleti, karşıya yükleme hiçbir etkinlik nedeniyle iptal edildi açıklanmaktadır. 

   * Sağ Profiler çalışmaya ve Profiler çalışırken gerçekleşen istekleri algılandığında özel olaylar gönderilir. Profiler bir istek için izleme eklenmiş ve izlemede görüntüleyebileceğiniz ServiceProfilerSample özel olay gösterilirse, geldiğini **Application Insights performans** bölmesi.

     Telemetri yoktu gösterilirse, Profiler çalışmıyor. Sorun gidermek için bu makalenin sonraki bölümlerinde, belirli uygulama türü için sorun giderme bölümlere bakın.  

     ![Profiler telemetri arayın][profiler-search-telemetry]

1. Profiler çalıştırdığınız sırada istekleri olsaydı istekleri Profiler etkin olan, bir uygulama bölümü tarafından işlenen emin olun. Uygulamaları bazen birden çok bileşenden oluşur ancak bileşenlerden yalnızca bazıları için Profiler etkinleştirilir. **Application Insights Profiler ' ı yapılandırma** izlemeleri karşıya yüklediğiniz bileşenlerin bölmesinde görüntülenir.

### <a name="other-things-to-check"></a>Denetlenecek başka şeyler
* Uygulamanızı .NET Framework 4.6 üzerinde çalıştığından emin olun.
* Web uygulamanız için bir ASP.NET Core uygulaması ise, en az çalıştırmalıdır ASP.NET Core 2.0.
* Görüntülemeye çalıştığınız veriler birkaç haftadır eskiyse, zaman filtrenizi görüntülemeyi deneyin ve yeniden deneyin. İzlemeleri yedi gün sonra silinir.
* Proxy'leri veya bir güvenlik duvarı erişimi engellemediğinizden emin olun https://gateway.azureserviceprofiler.net.

### <a id="double-counting"></a>Çift paralel iş parçacığı sayımı

Bazı durumlarda, toplam süre ölçümü yığın Görüntüleyicisi'nde istek süresi büyük.

İki veya daha fazla iş parçacığı bir istekle ilişkilendirilen ve paralel olarak çalışan bu durum oluşabilir. Bu durumda, toplam iş parçacığı geçen süre uzun süredir. Bir iş parçacığından diğerine tamamlanması bekliyor olabilir. Görüntüleyici, bu durum algılamaya çalışır ve sizi ilgilendirmeyen bekleme atlar. Bunun yapılması içinde çok fazla bilgileri görüntüleme kenarındaki errs yerine atlamak ne kritik bilgiler olabilir.

Paralel iş parçacıkları, izlemelerinde gördüğünüzde, istek için kritik yolu olmadığından emin olmak için hangi iş parçacıkları bekleyen belirleyin. Genellikle, iş parçacığı hızlı bir şekilde bir bekleme durumuna geçtiğinde yalnızca diğer iş parçacıkları üzerinde bekliyor. Diğer iş parçacıkları hakkında yoğunlaşabilirsiniz ve bekleyen iş parçacıklarının sürede yoksay.

### <a name="error-report-in-the-profile-viewer"></a>Profil Görüntüleyicisi'nde hata raporu
Portalında bir destek bileti gönderin. Hata iletisindeki bağıntı Kimliğini eklediğinizden emin olun.

## <a name="troubleshoot-profiler-on-azure-app-service"></a>Azure App Service'te Profiler sorunlarını giderme
Düzgün çalışması Profiler için:
* Temel katman web app service planınızın olması gerekir veya üzeri.
* Web uygulamanızı Application Insights'ın etkin olması gerekir.
* Web uygulamanız aşağıdaki uygulama ayarlarını sahip olmanız gerekir:

    |Uygulama Ayarı    | Value    |
    |---------------|----------|
    |APPINSIGHTS_INSTRUMENTATIONKEY         | Application Insights kaynağınızın iKey    |
    |APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
    |DiagnosticServices_EXTENSION_VERSION | ~3 |


* **ApplicationInsightsProfiler3** webjob çalışıyor olmalıdır. Webjob denetlemek için:
   1. Git [Kudu](https://blogs.msdn.microsoft.com/cdndevs/2015/04/01/the-kudu-debug-console-azure-websites-best-kept-secret/).
   1. İçinde **Araçları** menüsünde **Web işleri Panosu'nu**.  
      **WebJobs** bölmesi açılır. 
   
      ![Profil Oluşturucu webjob]   
   
   1. Günlük'dahil olmak üzere Web işi ayrıntılarını görüntülemek için seçin **ApplicationInsightsProfiler3** bağlantı.  
     **Sürekli WebJob ayrıntıları** bölmesi açılır.

      ![Profil Oluşturucu webjob günlüğü]

Neden Profiler sizin için çalışmayan şekil olamaz, günlüğünü indir ve Yardım almak için ekibimize göndermeden serviceprofilerhelp@microsoft.com. 
    
### <a name="manual-installation"></a>El ile yükleme

Profiler'ı yapılandırırken, güncelleştirmeler web uygulamasının Ayarlar hale getirilir. Ortamınızı gerektiriyorsa, güncelleştirmelerin el ile uygulayabilirsiniz. Örnek uygulamanız PowerApps için Web Apps ortamda çalışıyor olabilir. Güncelleştirmeleri el ile uygulamanız için:

1. İçinde **Web uygulaması denetimi** bölmesi açık **ayarları**.

1. Ayarlama **.NET Framework sürümünü** için **v4.6**.

1. Ayarlama **her zaman açık** için **üzerinde**.
1. Bu uygulama ayarları oluşturma:

    |Uygulama Ayarı    | Value    |
    |---------------|----------|
    |APPINSIGHTS_INSTRUMENTATIONKEY         | Application Insights kaynağınızın iKey    |
    |APPINSIGHTS_PROFILERFEATURE_VERSION | 1.0.0 |
    |DiagnosticServices_EXTENSION_VERSION | ~3 |

### <a name="too-many-active-profiling-sessions"></a>Profil oluşturma çok fazla etkin oturumlar

Şu anda en fazla dört Azure web uygulamaları ve aynı hizmet planında çalıştırdığınız dağıtım yuvalarını Profiler etkinleştirebilirsiniz. Bir app service planında çalıştırmayı dörtten fazla web apps varsa, Profiler fırlatabilir bir *Microsoft.ServiceProfiler.Exceptions.TooManyETWSessionException*. Profiler her web uygulaması için ayrı olarak çalışır ve her uygulama için bir olay izleme için Windows (ETW) oturumu başlatmak çalışır. Ancak ETW oturumlarına sınırlı sayıda tek seferde etkin olabilir. Profiler webjob profil oluşturma çok fazla etkin oturum bildirirse, bazı web uygulamaları, farklı hizmet planına taşıyın.

### <a name="deployment-error-directory-not-empty-dhomesitewwwrootappdatajobs"></a>Dağıtım hatası: Dizin boş değil ' D:\\giriş\\site\\wwwroot\\App_Data\\işlerin

Profiler etkin ile web uygulamanıza Web Apps kaynak dağıtarak, şu iletiyi görebilirsiniz:

*Dizin boş değil ' D:\\giriş\\site\\wwwroot\\App_Data\\işlerin*

Web dağıtımı komut dosyaları veya Azure DevOps dağıtım işlem hattı çalıştırırsanız, bu hata oluşur. Web dağıtımı görevi aşağıdaki ek dağıtım parametreleri eklemek için bir çözümdür:

```
-skip:Directory='.*\\App_Data\\jobs\\continuous\\ApplicationInsightsProfiler.*' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs\\continuous$' -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data\\jobs$'  -skip:skipAction=Delete,objectname='dirPath',absolutepath='.*\\App_Data$'
```

Bu parametreler, Application Insights Profiler ' ı tarafından kullanılan ve yeniden dağıtma işlemi engellemesini klasörü silin. Bunlar şu anda çalışıyor Profiler örneği etkilemez.

### <a name="how-do-i-determine-whether-application-insights-profiler-is-running"></a>Application Insights Profiler azalmadığını nasıl belirlerim?

Profiler web uygulamasında sürekli bir webjob olarak çalışır. Web uygulaması kaynak açabileceğiniz [Azure portalında](https://portal.azure.com). İçinde **WebJobs** bölmesinde, durumunu **ApplicationInsightsProfiler**. Çalışmıyorsa, açık **günlükleri** daha fazla bilgi için.

## <a name="troubleshoot-problems-with-profiler-and-azure-diagnostics"></a>Profiler'ı ve Azure Tanılama ile ilgili sorunları giderme

>**WAD içinde birlikte gelen bulut Hizmetleri için profil oluşturucu hata düzeltildi.** WAD (1.12.2.0) bulut Hizmetleri için en son sürümünü tüm son sürümleri App Insights SDK'sı ile çalışır. Bulut hizmet konakları WAD otomatik olarak yükseltilir, ancak hemen değildir. Yükseltme zorlamak için hizmetinizi yeniden dağıtın veya düğümü yeniden başlatma.

Profiler Azure tanılama tarafından doğru şekilde yapılandırılıp yapılandırılmadığını görmek için aşağıdakileri yapın: 
1. İlk olarak, dağıtılan Azure tanılama yapılandırması içeriğini beklediğiniz olup olmadığını denetleyin. 

1. İkinci olarak, Azure Tanılama, Profiler komut satırında uygun ikey değerini geçtiğinden emin olun. 

1. Üçüncü olarak, Profiler çalıştı, ancak bir hata döndürdü görmek için Profiler günlük dosyasını denetleyin. 

Azure Tanılama'yı yapılandırmak için kullanılan ayarları denetlemek için:

1. Sanal makine (VM) oturum açın ve ardından bu konumdaki günlük dosyasını açın. (C: sürücüsüne olabilir veya d: ve eklenti sürüm farklı olabilir.)

    ```
    c:\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.log  
    ```
    or
    ```
    c:\WindowsAzure\logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.11.3.12\DiagnosticsPlugin.log
    ```

1. Dosyada dize için arama yapabilirsiniz **WadCfg** Azure Tanılama'yı yapılandırmak için VM geçirilen ayarları bulunamıyor. Profiler havuzu tarafından kullanılan iKey doğru olup olmadığını kontrol edebilirsiniz.

1. Profiler'ı başlatmak için kullanılan komut satırını denetleyin. Aşağıdaki dosya Profiler'ı başlatmak için kullanılan bağımsız değişkenler. (Sürücü c: veya d: olabilir)

    ```
    D:\ProgramData\ApplicationInsightsProfiler\config.json
    ```

1. Profiler komut satırında iKey doğru olduğundan emin olun. 

1. Yol kullanılarak bulunan önceki *config.json* dosya, Profiler günlük dosyasını denetleyin. Profiler'ı kullanarak ayarlarını belirten hata ayıklama bilgileri görüntüler. Profiler gelen durum ve hata iletileri de görüntüler.  

    Profiler, uygulamanız istekleri alırken çalışıyorsa, aşağıdaki ileti görüntülenir: *Etkinlik iKey algılandı*. 

    İzleme karşıya yüklendiğinde, aşağıdaki ileti görüntülenir: *İzleme yüklenmeye başlıyor*. 


[profiler-search-telemetry]:./media/profiler-troubleshooting/Profiler-Search-Telemetry.png
[Profil Oluşturucu webjob]:./media/profiler-troubleshooting/Profiler-webjob.png
[Profil Oluşturucu webjob günlüğü]:./media/profiler-troubleshooting/Profiler-webjob-log.png








