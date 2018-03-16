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
ms.openlocfilehash: 2b4a5f548578b563c92f8d7ff85457b50b02fbd4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="snapshot-debugger-troubleshooting-guide"></a>Anlık görüntü hata ayıklayıcı: Sorun giderme kılavuzu

Uygulama Öngörüler anlık görüntü hata ayıklayıcısı hata ayıklama anlık görüntü otomatik olarak canlı web uygulamalarından toplamanızı sağlar. Anlık görüntü şu anda bir özel durum oluştu kaynak kodu ve değişkenleri durumunu gösterir. Bu makalede, hata ayıklayıcı nasıl çalıştığını ve sık karşılaşılan sorunların çözümleri sağlar açıklanmaktadır.

## <a name="how-does-application-insights-snapshot-debugger-work"></a>Uygulama Öngörüler anlık görüntü hata ayıklayıcı nasıl çalışır

Uygulama Öngörüler anlık görüntü hata ayıklayıcı Application Insights telemetri ardışık düzen (ITelemetryProcessor örneğini) bir parçasıdır. Anlık görüntü Toplayıcı kodunuzda (AppDomain.FirstChanceException) oluşturulan özel durumları ve Application Insights bildirilen özel durum telemetrisi izler `TelemetryClient.TrackException`. Projeniz için anlık görüntü Toplayıcı paketi başarıyla ekledikten sonra 'AppInsightsSnapshotCollectorLogs' ve özel verileri ' SnapshotCollectorEnabled' adlı özel bir olay telemetri ardışık düzeninde bir olay algıladı gönderdi. 'SnapshotUploader.exe' adlı bir işlem, aynı anda başlayacaktır (sürüm 1.2.0 veya üstü) veya (önce sürüm 1.2.0), ' MinidumpUploader.exe' toplanan karşıya yüklemek için Application Insights veri dosyalarını anlık görüntüsünü alın.  Yükleyici işlemi başladığında, 'UploaderStart' adlı özel bir olay gönderir. Bundan sonra anlık görüntü Toplayıcı normal izleme davranışını girin.

Anlık görüntü Toplayıcı Application Insights özel durum telemetrisi izleme sırasında tanımlanan parametreler (örneğin, ThresholdForSnapshotting, MaximumSnapshotsRequired, MaximumCollectionPlanSize, ProblemCounterResetInterval) kullanır bir anlık görüntü toplamak ne zaman belirlemek için yapılandırma. Tüm kurallar karşılandığında, Toplayıcı aynı noktada bir sonraki durum için bir anlık görüntü ister. Aynı anda bir Application Insights özel olay 'AppInsightsSnapshotCollectorLogs' adlı ve 'RequestSnapshots' gönderilir. Derleyici 'Sürüm' kod en iyi duruma getirir olduğundan, yerel değişkenleri toplanan anlık görünür olmayabilir. Anlık görüntü Toplayıcı anlık görüntüleri istediğinde özel durum oluşturdu yöntemi deoptimize dener. Bu süre boyunca, Application Insights özel olayda adı 'AppInsightsSnapshotCollectorLogs' ve 'ProductionBreakpointsDeOptimizeMethod' ile özel verileri gönderilir.  Anlık görüntü sonraki özel durumun toplandığında, yerel değişkenleri kullanılabilir. Anlık görüntü toplandıktan sonra kodu reoptimize. 

> [!NOTE]
> Deoptimization Application Insights site uzantısı yüklü olmasını gerektirir.

Bir anlık görüntü için belirli bir durum istendiğinde, anlık görüntü Toplayıcı uygulamanızın özel durum (AppDomain.FirstChanceException) ardışık düzen işleme izleme başlayacak. Özel durum yeniden gerçekleştiğinde, Toplayıcı bir anlık görüntü (Application Insights özel olay 'AppInsightsSnapshotCollectorLogs' adıyla ve özel verileri ' SnapshotStart') başlatın. Çalışan işlemi gölge kopyası yapıldıktan sonra (sayfa tablosu çoğaltılır). Bu, normal olarak 10 ila 20 milisaniye sürecek. Bundan sonra bir Application Insights özel olay 'AppInsightsSnapshotCollectorLogs' adlı ve 'SnapshotStop' özel veri gönderilir. Forked işlem oluşturulduğunda, toplam disk belleği belleği (çalışma kümesi çok daha küçük olur), çalışan uygulamanızın aynı miktarı daha yüksek. Gölge kopya uygulama işlemi normal olarak çalışırken işleminin bellek diske yazılan ve Application Insights'a yüklenir. Anlık görüntü yüklendikten sonra 'UploadSnapshotFinish' adında bir Application Insights özel olay gönderilir.

## <a name="is-the-snapshot-collector-working-properly"></a>Anlık görüntü Toplayıcı düzgün çalışıyor mu?

### <a name="how-to-find-snapshot-collector-logs"></a>Anlık görüntü Toplayıcı Günlüklerini bulma
Anlık görüntü Toplayıcı günlükleri, uygulama Insight hesabınıza gönderilen [anlık görüntü Toplayıcı NuGet paketi](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) 1.1.0 sürümü veya sonraki bir sürümü. Emin olun *ProvideAnonymousTelemetry* (değeri varsayılan olarak true) false ayarlı değil.

* Azure portalında Application Insights kaynağınıza gidin.
* Tıklatın *arama* genel bakış bölümünde.
* Aşağıdaki dize arama kutusuna girin:
    ```
    AppInsightsSnapshotCollectorLogs OR AppInsightsSnapshotUploaderLogs OR UploadSnapshotFinish OR UploaderStart OR UploaderStop
    ```
* Not: değiştirme *zaman aralığı* gerekiyorsa.

![Arama anlık görüntü Toplayıcı ekran günlükleri](./media/app-insights-troubleshoot-snapshot-debugger/001.png)


### <a name="examine-snapshot-collector-logs"></a>Anlık görüntü Toplayıcı günlüklerini inceleyin
Anlık görüntü Toplayıcı günlükleri için arama yaparken, hedeflenen zaman aralığı içinde 'UploadSnapshotFinish' olayları olmalıdır. Hala anlık görüntü açmak için 'Hata ayıklama anlık Aç' düğmesini görmüyorsanız, e-posta Gönder snapshothelp@microsoft.com uygulama Insights izleme anahtarı ile.

![Anlık görüntü Toplayıcı günlükleri, ekran inceleyin](./media/app-insights-troubleshoot-snapshot-debugger/005.png)

## <a name="i-cannot-find-a-snapshot-to-open"></a>Açık bir anlık görüntü bulunamadı.
Aşağıdaki adımlar sorunu gidermenize yardımcı olmuyorsa, e-posta Gönder snapshothelp@microsoft.com , Application Insights izleme anahtarı ile.

### <a name="step-1-make-sure-your-application-is-sending-telemetry-data-and-exception-data-to-application-insights"></a>1. adım: uygulamanızı için Application Insights telemetri verilerini ve özel durum verileri gönderiyor emin olun
Application Insights kaynağına gidin, uygulamanızdan gönderilen veriler olup olmadığını denetleyin.

### <a name="step-2-make-sure-snapshot-collector-is-added-correctly-to-your-applications-application-insights-telemetry-pipeline"></a>2. adım: Anlık görüntü Toplayıcı doğru uygulamanızın Application Insights Telemetrisi ardışık düzene eklendiğinden emin olun
'Anlık görüntü Toplayıcı Günlüklerini bulmak nasıl' adımda günlükleri bulabilirsiniz, anlık görüntü Toplayıcı doğru projenize eklenir ve bu adımı yoksayabilirsiniz.

Anlık görüntü Toplayıcı günlük varsa, aşağıdakileri doğrulayın:
* Bu satır için Klasik ASP.NET uygulamaları için denetleme  *<Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">*  içinde *Applicationınsights.config* dosya.

* ASP.NET Core uygulamaları emin olun *ITelemetryProcessorFactory* ile *SnapshotCollectorTelemetryProcessor* eklenen *IServiceCollection* Hizmetleri .

* Ayrıca yayımlanan uygulamanızda doğru izleme anahtarını kullanıyorsanız denetleyin.

* Anlık görüntü Toplayıcı bir uygulama içinde birden çok izleme anahtarı desteklemiyor, anlık görüntüler, gözlemleyen ilk özel durum izleme anahtarını gönderir.

* Ayarlarsanız *InstrmentationKey* kodunuzda el ile güncelleştirmeniz *InstrumentationKey* öğesinden *Applicationınsights.config*.

### <a name="step-3-make-sure-the-minidump-uploader-is-started"></a>3. adım: mini döküm karşıya yükleyen başlatıldığından emin olun
Anlık görüntü Toplayıcı Günlüklerini aramak *UploaderStart* (UploaderStart arama metin kutusuna yazın). Anlık görüntü Toplayıcı ilk özel durum izlenen bir olay olması gerekir. Bu olay yoksa, diğer ayrıntılar için günlükleri denetleyin. Bu sorunu çözmek için bir olası yolu, uygulamanızın yeniden başlatılıyor.

### <a name="step-4-make-sure-snapshot-collector-expressed-its-intent-to-collect-snapshots"></a>4. adım: Anlık görüntü Toplayıcı anlık görüntüleri toplamak yapma ifade emin olun
Anlık görüntü Toplayıcı Günlüklerini aramak *RequestSnapshots* (tür ```RequestSnapshots``` arama metin kutusuna).  Hiç varsa, yapılandırmanızı denetleyin. Örneğin, *ThresholdForSnapshotting*, anlık görüntüleri başlatılmadan önce oluşabilecek belirli bir özel durum sayısını belirtir.

### <a name="step-5-make-sure-that-snapshot-is-not-disabled-due-to-memory-protection"></a>5. adım: Anlık görüntü nedeniyle bellek koruması devre dışı olduğundan emin olun
İyi bellek arabelleği olduğunda, uygulamanızın performansı korumak için bir anlık görüntü yalnızca yakalanmasını. Anlık görüntü Toplayıcı günlükleri 'CannotSnapshotDueToMemoryUsage' için arama yapın. Olayın özel verilerde ayrıntılı bir nedenle sahip olur. Uygulamanız bir Azure Web uygulaması çalıştırıyorsa, kısıtlama katı olması olabilir. Bu yana bazı bellek kuralları karşılandığında Azure Web uygulaması, uygulamanızın yeniden başlatılır. Bu sorunu çözmek için hizmet planınızı makinelere daha fazla bellek ile ölçeklendirin deneyebilirsiniz.

### <a name="step-6-make-sure-snapshots-were-captured"></a>6. adım: anlık görüntüleri yakalanan emin olun
Anlık görüntü Toplayıcı Günlüklerini aramak ```RequestSnapshots```.  Hiçbiri mevcut değilse yapılandırmanızı denetleyin. Örneğin, *ThresholdForSnapshotting*, bir anlık görüntüyü topladıktan önce belirli bir özel durum sayısını belirtir.

### <a name="step-7-make-sure-snapshots-are-uploaded-correctly"></a>7. adım: anlık görüntüleri doğru karşıya emin olun
Anlık görüntü Toplayıcı Günlüklerini aramak ```UploadSnapshotFinish```.  Bu mevcut değilse, e-posta Gönder snapshothelp@microsoft.com , Application Insights izleme anahtarı ile. Bu olay varsa günlükleri birini açın ve özel verileri 'SnapshotId' değerini kopyalayın. Ardından değeri arayın. Bu anlık görüntüye karşılık gelen özel durum bulur. Özel durumu ve açık hata ayıklama anlık görüntü'ye tıklayın. (Karşılık gelen hiçbir özel durum ise, özel durum telemetrisi nedeniyle yüksek hacimli örneklenebilir. Başka bir snapshotId deneyin.)

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

Durumunuz farklı olması durumunda bir hata olduğunu gösteriyor olabilir. E-posta Gönder snapshothelp@microsoft.com kod parçacığında yanı sıra, Application Insights izleme anahtarı ile.

## <a name="snapshot-view-cannot-obtain-value-of-the-local-variable-or-argument"></a>Anlık görünümü: yerel değişken veya değişken değeri alınamıyor
Emin olun [Application Insights site uzantısı](https://www.siteextensions.net/packages/Microsoft.ApplicationInsights.AzureWebSites/) yüklenir. Sorun devam ederse, e-posta Gönder snapshothelp@microsoft.com , Application Insights izleme anahtarı ile.
