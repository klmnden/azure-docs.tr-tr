---
title: "Azure Application Insights anlık görüntü hata ayıklayıcı sorun giderme kılavuzu | Microsoft Docs"
description: "Azure uygulama Öngörüler anlık görüntü hata ayıklayıcı hakkında sık sorulan sorular."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 
ms.service: application-insights
ms.workload: 
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mbullwin
ms.openlocfilehash: 5d6a2fe4c3ee373172f5324b6c7a39e4f94f4652
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="snapshot-debugger-troubleshooting-guide"></a>Anlık görüntü hata ayıklayıcı: Sorun giderme kılavuzu

Uygulama Öngörüler anlık görüntü hata ayıklayıcısı hata ayıklama anlık görüntü otomatik olarak canlı web uygulamalarından toplamanızı sağlar. Anlık görüntü şu anda bir özel durum oluştu kaynak kodu ve değişkenleri durumunu gösterir. Application Insights anlık görüntü hata ayıklayıcı Başlarken ve bu makalede çalışırken sorunlarla karşılaşıyorsanız, hata ayıklayıcı, genel sorun giderme senaryoları için çözümler ile birlikte çalışma şeklini aracılığıyla anlatılmaktadır. 

## <a name="how-does-application-insights-snapshot-debugger-work"></a>Uygulama Öngörüler anlık görüntü hata ayıklayıcı nasıl çalışır

Uygulama Öngörüler anlık görüntü hata ayıklayıcı Application Insights telemetri ardışık düzen (ITelemetryProcessor örneğini) bir parçası, anlık görüntü Toplayıcı kodunuzda (AppDomain.FirstChanceException) oluşturulan özel durumlar ve özel durumları izler Application Insights özel durum Telemetrisi ardışık düzen tarafından izlenen. Projeniz için anlık görüntü Toplayıcı başarıyla eklendi ve Application Insights telemetri bir özel durum algıladı sonra kanal, Application Insights özel olaya 'AppInsightsSnapshotCollectorLogs' adlı ve ' SnapshotCollectorEnabled' özel veriler gönderilir. Aynı anda 'Application Insights'a toplanan anlık görüntü veri dosyalarını karşıya yüklemek için MinidumpUploader.exe', adı, bir işlem başlayacaktır.  'MinidumpUploader.exe' başlatır işlerken, 'UploaderStart' adlı özel bir olay gönderilir. Önceki adımlar, anlık görüntü Toplayıcı normal izleme davranışını girer.

Anlık görüntü Toplayıcı Application Insights özel durum telemetrisi izleme sırasında tanımlanan parametreler (örneğin, ThresholdForSnapshotting, MaximumSnapshotsRequired, MaximumCollectionPlanSize, ProblemCounterResetInterval) kullanır bir anlık görüntü toplamak ne zaman belirlemek için yapılandırma. Tüm kurallar karşılandığında, Toplayıcı aynı noktada bir sonraki durum için bir anlık görüntü ister. Aynı anda bir Application Insights özel olay 'AppInsightsSnapshotCollectorLogs' adlı ve 'RequestSnapshots' gönderilir. Derleyici 'Sürüm' kod en iyi duruma getirir olduğundan, yerel değişkenleri toplanan anlık görünür olmayabilir. Anlık görüntü Toplayıcı anlık görüntüleri istediğinde özel durum oluşturdu yöntemi deoptimize dener. Bu süre boyunca, Application Insights özel olayda adı 'AppInsightsSnapshotCollectorLogs' ve 'ProductionBreakpointsDeOptimizeMethod' ile özel verileri gönderilir.  Anlık görüntü sonraki özel durumun toplandığında, yerel değişkenleri kullanılabilir. Anlık görüntü toplandıktan sonra bir performans sağlamak için kodu reoptimize. 

> [!NOTE]
> Deoptimization Application Insights site uzantısı yüklü olmasını gerektirir.

Bir anlık görüntü için belirli bir durum istendiğinde, anlık görüntü Toplayıcı uygulamanızın özel durum (AppDomain.FirstChanceException) ardışık düzen işleme izleme başlayacak. Özel durum yeniden gerçekleştiğinde, Toplayıcı bir anlık görüntü (Application Insights özel olay 'AppInsightsSnapshotCollectorLogs' adıyla ve özel verileri ' SnapshotStart') başlatın. Çalışan işlemi gölge kopyası yapıldıktan sonra (sayfa tablosu çoğaltılır). Bu, normal olarak 10 ila 20 milisaniye sürecek. Bundan sonra bir Application Insights özel olay 'AppInsightsSnapshotCollectorLogs' adlı ve 'SnapshotStop' özel veri gönderilir. Forked işlem oluşturulduğunda, toplam disk belleği belleği (çalışma kümesi çok daha küçük olur), çalışan uygulamanızın aynı miktarı daha yüksek. Gölge kopya uygulama işlemi normal olarak çalışırken işleminin bellek diske yazılan ve Application Insights'a yüklenir. Anlık görüntü yüklendikten sonra 'UploadSnapshotFinish' adında bir Application Insights özel olay gönderilir.

## <a name="is-the-snapshot-collector-working-properly"></a>Anlık görüntü Toplayıcı düzgün çalışıyor mu?

### <a name="how-to-find-snapshot-collector-logs"></a>Anlık görüntü Toplayıcı Günlüklerini bulma
Anlık görüntü Toplayıcı günlükleri, uygulama Insight hesabınıza gönderilen [anlık görüntü Toplayıcı NuGet paketi](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) sürüm 1.1.0 veya üstü yüklü. Emin olun *ProvideAnonymousTelemetry* (değeri varsayılan olarak true) false ayarlı değil.

* Azure portalında Application Insights kaynağınıza gidin.
* Tıklatın *arama* genel bakış bölümünde.
* Aşağıdaki dize arama kutusuna girin:
    ```
    AppInsightsSnapshotCollectorLogs OR AppInsightsSnapshotUploaderLogs OR UploadSnapshotFinish OR UploaderStart OR UploaderStop
    ```
* Not: değiştirme *zaman aralığı* gerekiyorsa.

![Arama anlık görüntü Toplayıcı ekran günlükleri](./media/app-insights-troubleshoot-snapshot-debugger/001.png)


### <a name="examine-snapshot-collector-logs"></a>Anlık görüntü Toplayıcı günlüklerini inceleyin
Anlık görüntü Toplayıcı günlükleri için arama yaparken, hedeflenen zaman aralığı içinde 'UploadSnapshotFinish' olayları olmalıdır. Hala anlık görüntü açmak için 'Hata ayıklama anlık Aç' düğmesini görmüyorsanız, lütfen e-posta Gönder snapshothelp@microsoft.com , Application Insights izleme anahtarı ile.

![Anlık görüntü Toplayıcı günlükleri, ekran inceleyin](./media/app-insights-troubleshoot-snapshot-debugger/005.png)

## <a name="i-cannot-find-a-snapshot-to-open"></a>Açık bir anlık görüntü bulunamadı.
Aşağıdaki adımlar sorunu gidermenize yardımcı yok, lütfen e-posta Gönder snapshothelp@microsoft.com , Application Insights izleme anahtarı ile.

### <a name="step-1-make-sure-your-application-is-sending-telemetry-data-and-exception-data-to-application-insights"></a>1. adım: uygulamanızı için Application Insights telemetri verilerini ve özel durum verileri gönderiyor emin olun
Application Insights kaynağına gidin, uygulamanızdan gönderilen veriler olup olmadığını denetleyin.

### <a name="step-2-make-sure-snapshot-collector-is-added-correctly-to-your-applications-application-insights-telemetry-pipeline"></a>2. adım: Anlık görüntü Toplayıcı doğru uygulamanızın Application Insights Telemetrisi ardışık düzene eklendiğinden emin olun
Anlık görüntü Toplayıcı doğru şekilde projeniz için eklenir 'anlık görüntü Toplayıcı Günlüklerini bulmak nasıl' adımda günlükleri bulabilirsiniz kullanmıyorsanız bu adımı yoksayabilirsiniz.

Anlık görüntü Toplayıcı günlük varsa, aşağıdakileri doğrulayın:
* Bu satır için Klasik ASP.NET uygulamaları için denetleme  *<Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">*  içinde *Applicationınsights.config* dosya.

* ASP.NET Core uygulamaları emin olun *ITelemetryProcessorFactory* ile *SnapshotCollectorTelemetryProcessor* eklenen *IServiceCollection* Hizmetleri .

* Ayrıca yayımlanan uygulamanızda doğru izleme anahtarını kullanıyorsanız denetleyin.

* Anlık görüntü Toplayıcı bir uygulama içinde birden çok izleme anahtarı desteklemiyor, anlık görüntüler, gözlemleyen ilk özel durum izleme anahtarını gönderir.

* Ayarlarsanız *InstrmentationKey* kodunuzda, lütfen düzeltmeli *InstrumentationKey* öğesinden *Applicationınsights.config*.

### <a name="step-3-make-sure-the-minidump-uploader-is-started"></a>3. adım: mini döküm karşıya yükleyen başlatıldığından emin olun
Anlık görüntü Toplayıcı Günlüklerini aramak *UploaderStart* (UploaderStart arama metin kutusuna yazın). Anlık görüntü Toplayıcı ilk özel durum izlenen bir olay olması gerekir. Bu olay yoksa, diğer ayrıntılar için günlükleri denetleyin. Bu sorunu çözmek için bir olası yolu, uygulamanızın yeniden başlatılıyor.

### <a name="step-4-make-sure-snapshot-collector-expressed-its-intent-to-collect-snapshots"></a>4. adım: Anlık görüntü Toplayıcı anlık görüntüleri toplamak yapma ifade emin olun
Anlık görüntü Toplayıcı Günlüklerini aramak *RequestSnapshots* (tür ```RequestSnapshots``` arama metin kutusuna).  Hiç varsa, Lütfen yapılandırmanızı, örneğin denetleyin *ThresholdForSnapshotting* anlık görüntüleri başlatılmadan önce oluşabilecek belirli bir özel durum sayısını belirtir.

### <a name="step-5-make-sure-that-snapshot-is-not-disabled-due-to-memory-protection"></a>5. adım: Anlık görüntü nedeniyle bellek koruması devre dışı olduğundan emin olun
İyi bellek arabelleği olduğunda, uygulamanızın performansı korumak için bir anlık görüntü yalnızca yakalanmasını. Anlık görüntü Toplayıcı günlükleri 'CannotSnapshotDueToMemoryUsage' için arama yapın. Olayın özel verilerde ayrıntılı bir nedenle sahip olur. Uygulamanız bir Azure Web uygulaması çalıştırıyorsa, kısıtlama katı olması olabilir. Bu yana bazı bellek kuralları karşılandığında Azure Web uygulaması, uygulamanızın yeniden başlatılır. Bu sorunu çözmek için hizmet planınızı makinelere daha fazla bellek ile ölçeklendirin deneyebilirsiniz.

### <a name="step-6-make-sure-snapshots-were-captured"></a>6. adım: anlık görüntüleri yakalanan emin olun
Anlık görüntü Toplayıcı Günlüklerini aramak ```RequestSnapshots```.  Hiçbiri yoksa, yapılandırmanıza, örneğin denetleyin ```ThresholdForSnapshotting``` bir anlık görüntüyü topladıktan önce bu özel durum sayısını gösterir.

### <a name="step-7-make-sure-snapshots-are-uploaded-correctly"></a>7. adım: anlık görüntüleri doğru karşıya emin olun
Anlık görüntü Toplayıcı Günlüklerini aramak ```UploadSnapshotFinish```.  Lütfen bu mevcut değilse, e-posta Gönder snapshothelp@microsoft.com , Application Insights izleme anahtarı ile. Bu olay varsa günlükleri birini açın ve özel verileri 'SnapshotId' değerini kopyalayın. Ardından değeri arayın. Bu anlık görüntüye karşılık gelen özel durum bulur. Özel durumu ve açık hata ayıklama anlık görüntü'ye tıklayın. (Karşılık gelen hiçbir özel durum ise, özel durum telemetrisi olabilir örneklenen, yüksek hacim nedeniyle, Lütfen başka bir snapshotId deneyin.)

![Bul SnapshotId ekran görüntüsü](./media/app-insights-troubleshoot-snapshot-debugger/002.png)

![Açık özel durumun ekran görüntüsü](./media/app-insights-troubleshoot-snapshot-debugger/004b.png)

![Açık ekran hata ayıklama anlık görüntü](./media/app-insights-troubleshoot-snapshot-debugger/003.png)

## <a name="snapshot-view-local-variables-are-not-complete"></a>Anlık görüntü görünüm yerel değişkenler tamamlanmamış

Yerel değişkenler bazıları eksik. Uygulamanızı sürüm kodu çalışıyorsa, derleyici bazı değişkenler hemen en iyi duruma getirir. Örneğin:

```csharp
    const int a = 1; // a will be discarded by compiler and the value 1 will be inline.
    Random rand = new Random();
    int b = rand.Next() % 300; // b will be discarded and the value will be directly put to the 'FindNthPrimeNumber' call stack.
    long primeNumber = FindNthPrimeNumber(b);
```

Durumunuz farklı olması durumunda bir hata olabilir. Lütfen e-posta Gönder snapshothelp@microsoft.com kod parçacığında yanı sıra, Application Insights izleme anahtarı ile.

## <a name="snapshot-view-cannot-obtain-value-of-the-local-variable-or-argument"></a>Anlık görünümü: yerel değişken veya değişken değeri alınamıyor
Lütfen emin olun [Application Insights site uzantısı](https://www.siteextensions.net/packages/Microsoft.ApplicationInsights.AzureWebSites/) yüklenir. Sorun devam ederse, lütfen e-posta Gönder snapshothelp@microsoft.com , Application Insights izleme anahtarı ile.
