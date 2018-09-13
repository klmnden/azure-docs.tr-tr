---
title: Azure Application Insights Snapshot Debugger sorun giderme kılavuzu | Microsoft Docs
description: Sık sorulan Azure Application Insights Snapshot Debugger hakkında sorular.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: ''
ms.service: application-insights
ms.workload: ''
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2017
ms.author: mbullwin
ms.openlocfilehash: 285f42a6b52819077b92abce78c1f51756780604
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35650886"
---
# <a name="snapshot-debugger-troubleshooting-guide"></a>Anlık görüntü hata ayıklayıcısı: Sorun giderme kılavuzu

Application Insights Snapshot Debugger, hata ayıklama anlık görüntüsünü canlı web uygulamalarını da otomatik olarak toplamasını sağlar. Anlık görüntü şu anda bir özel durum oluştu kaynak kodu ve değişkenleri durumunu gösterir. Bu makalede, hata ayıklayıcı nasıl çalıştığını ve sık karşılaşılan sorunlara çözümler sağlar açıklanmaktadır.

## <a name="how-does-application-insights-snapshot-debugger-work"></a>Application Insights Snapshot Debugger nasıl çalışır

Application Insights Snapshot Debugger, Application Insights telemetri ardışık düzen (ITelemetryProcessor örneği) bir parçasıdır. Anlık görüntü toplayıcının kodunuzda (AppDomain.FirstChanceException) oluşturulan özel durumları hem Application Insights ile bildirilen özel durum telemetrisi izler `TelemetryClient.TrackException`. Anlık görüntü toplayıcının paketi başarıyla projenize ekledikten sonra 'AppInsightsSnapshotCollectorLogs' ve 'SnapshotCollectorEnabled' özel verileri, adlı bir özel olay telemetri ardışık düzeninde bir olay algıladı gönderilir. Aynı zamanda, 'SnapshotUploader.exe' adlı bir işlem başlar (sürümü 1.2.0 veya üzeri) ya da 'MinidumpUploader.exe' (önce sürümü 1.2.0 olarak güncelleştirilir), toplanan karşıya yüklemek için veri dosyaları için Application Insights anlık görüntü.  Yükleyici işlemi başladığında, 'UploaderStart' adlı özel bir olay gönderir. Bundan sonra anlık görüntü toplayıcının normal izleme davranışını girin.

Anlık görüntü toplayıcının, Application Insights özel durum telemetrisi izleme sırasında tanımlanan parametreler (örneğin, ThresholdForSnapshotting MaximumSnapshotsRequired, MaximumCollectionPlanSize, ProblemCounterResetInterval) kullanır. Anlık görüntü toplamak ne zaman belirlemek için yapılandırma. Tüm kurallar karşılandığında, Toplayıcı aynı noktada İleri durum için bir anlık görüntü ister. Aynı anda bir Application Insights özel olay adıyla 'AppInsightsSnapshotCollectorLogs' ve 'RequestSnapshots' gönderilir. Yerel değişkenler, derleyicinin 'Sürüm' kod iyileştirir olduğundan, toplanan anlık görünür olmayabilir. Anlık görüntü toplayıcının anlık görüntüleri istediğinde, özel durum oluşturdu yöntemi deoptimize dener. Bu süre boyunca, bir Application Insights özel olay adı 'AppInsightsSnapshotCollectorLogs' ve 'ProductionBreakpointsDeOptimizeMethod' özel veri gönderilir.  Sonraki özel durumun anlık görüntü toplandığında, yerel değişkenler kullanılabilir. Anlık görüntü toplandıktan sonra kodu reoptimize. 

> [!NOTE]
> Deoptimization Application Insights site uzantısı yüklü olmasını gerektirir.

Belirli bir özel durum için bir anlık görüntü istendiğinde, anlık görüntü toplayıcının, uygulamanızın özel durum işleme (AppDomain.FirstChanceException) işlem hattı izleme başlar. Özel durum yeniden oluşur, Toplayıcı ('AppInsightsSnapshotCollectorLogs' adlı Application Insights özel olay ve özel veri ' SnapshotStart') bir anlık görüntü başlar. Çalışan işlemi bir gölge kopyasını yapılan sonra (sayfa tablosu çoğaltılır). Bu, normalde 10 ila 20 milisaniye götürür. Bundan sonra bir 'AppInsightsSnapshotCollectorLogs' adlı Application Insights özel olay ve özel veri ' SnapshotStop' gönderilir. Çatalı oluşturulan işlem oluşturulduğunda, toplam disk belleği aynı miktarda belleği (çalışma kümesi çok daha küçük olur), çalışan uygulamanızın olarak yükseltilecektir. Uygulama işleminizi normalde, gölge kopya çalışırken işlemin belleğinin diske yazılan ve Application Insights'a karşıya yüklendi. Bir Application Insights özel olay 'UploadSnapshotFinish' adlı anlık görüntü yüklendikten sonra gönderilir.

## <a name="is-the-snapshot-collector-working-properly"></a>Anlık görüntü toplayıcının düzgün çalışıyor mu?

### <a name="how-to-find-snapshot-collector-logs"></a>Anlık görüntü toplayıcının günlüklerini bulma
Anlık görüntü toplayıcısı günlükleri, Application Insight hesabınıza gönderilen [Snapshot Collector NuGet paketini](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) 1.1.0 sürümü veya üzeri. Emin *ProvideAnonymousTelemetry* false (değeri varsayılan olarak true) ayarlanmadı.

* Azure portalında Application Insights kaynağınıza gidin.
* Tıklayın *arama* genel bakış bölümünde.
* Arama kutusuna aşağıdaki dizeyi girin:
    ```
    AppInsightsSnapshotCollectorLogs OR AppInsightsSnapshotUploaderLogs OR UploadSnapshotFinish OR UploaderStart OR UploaderStop
    ```
* Not: değiştirmek *zaman aralığı* gerekirse.

![Arama anlık görüntü toplayıcının ekran günlükleri](./media/app-insights-troubleshoot-snapshot-debugger/001.png)


### <a name="examine-snapshot-collector-logs"></a>Anlık görüntü toplayıcısı günlüklerini inceleyin
Anlık görüntü toplayıcının günlükleri için arama yaparken, hedeflenen bir zaman aralığında 'UploadSnapshotFinish' olaylar olması gerekir. Anlık görüntü açmak için ' Aç hata ayıklama anlık görüntüsü' düğmesini hala görmüyorsanız, e-posta Gönder snapshothelp@microsoft.com , Application Insights izleme anahtarı ile.

![Anlık görüntü toplayıcısı günlüklerini, ekran inceleyin](./media/app-insights-troubleshoot-snapshot-debugger/005.png)

## <a name="i-cannot-find-a-snapshot-to-open"></a>Açık bir anlık görüntüye bulamıyorum
Aşağıdaki adımlar sorunu gidermenize yardımcı yoksa e-posta gönderin snapshothelp@microsoft.com , Application Insights izleme anahtarı ile.

### <a name="step-1-make-sure-your-application-is-sending-telemetry-data-and-exception-data-to-application-insights"></a>1. adım: uygulamanızı Application Insights'a telemetri verilerini ve özel durum veri gönderiyor emin olun
Application Insights kaynağına gidin, uygulamanızdan gönderilen verilerin olup olmadığını denetleyin.

### <a name="step-2-make-sure-snapshot-collector-is-added-correctly-to-your-applications-application-insights-telemetry-pipeline"></a>2. adım: Anlık görüntü toplayıcının düzgün uygulamanızda Application Insights Telemetrisi ardışık düzenine eklenen emin olun
Günlükleri 'anlık görüntü toplayıcının günlüklerini nasıl' adımda bulabilirsiniz, anlık görüntü toplayıcının düzgün olarak projenize eklenir ve bu adımı yoksayabilirsiniz.

Anlık görüntü toplayıcısı günlük varsa, aşağıdakileri doğrulayın:
* Klasik ASP.NET uygulamaları için bu satır için denetleme *<Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">* içinde *Applicationınsights.config* dosya.

* ASP.NET Core uygulamaları için emin *ITelemetryProcessorFactory* ile *SnapshotCollectorTelemetryProcessor* eklenir *IServiceCollection* Hizmetleri .

* Ayrıca, yayımlanan uygulamanızın doğru izleme anahtarını kullandığınızdan emin denetleyin.

* Anlık görüntü toplayıcının bir uygulama içinde birden çok izleme anahtarı desteklemiyor; bunu uyan ilk özel durum izleme anahtarı için anlık görüntü gönderir.

* Ayarlarsanız *InstrmentationKey* kodunuzda el ile güncelleştirmeniz *Instrumentationkey* öğesinden *Applicationınsights.config*.

### <a name="step-3-make-sure-the-minidump-uploader-is-started"></a>3. adım: mini döküm uploader başlatıldığından emin olun
Anlık görüntü toplayıcısı günlüklerinde arama *UploaderStart* (UploaderStart arama metin kutusuna yazın). Anlık görüntü toplayıcının ilk özel durum izlenen bir olay olmalıdır. Bu olay yoksa, diğer günlükler için ayrıntıları kontrol edin. Bu sorunu çözmek için bir olası yolu, uygulamanızı yeniden başlatılıyor.

### <a name="step-4-make-sure-snapshot-collector-expressed-its-intent-to-collect-snapshots"></a>4. adım: Anlık görüntü toplayıcının anlık görüntüleri toplamak için konuşmanın niyetini ifade emin olun
Anlık görüntü toplayıcısı günlüklerinde arama *RequestSnapshots* (tür ```RequestSnapshots``` arama metin kutusuna).  Yoksa herhangi yapılandırmanızı denetleyin. Örneğin, *ThresholdForSnapshotting*, anlık görüntüleri toplama başlamadan önce oluşabilecek belirli bir özel durum sayısını belirtir.

### <a name="step-5-make-sure-that-snapshot-is-not-disabled-due-to-memory-protection"></a>5. adım: Anlık görüntü nedeniyle bellek koruması devre dışı olduğunu doğrulayın
İyi bellek arabelleği olduğunda uygulamanızın performansını korumak için bir anlık görüntü yalnızca yakalanması. Anlık görüntü toplayıcısı günlüklere 'CannotSnapshotDueToMemoryUsage' için arama yapın. Olayın özel verilerde, ayrıntılı bir neden olacaktır. Uygulamanızı bir Azure Web uygulamasında çalışıyor, kısıtlama katı olması olabilir. Bu yana bazı bellek kuralları karşılandığında uygulamanızı Azure Web uygulaması yeniden başlatılır. Bu sorunu çözmek için hizmet planınızı makinelere daha fazla bellek ile ölçekleme deneyebilirsiniz.

### <a name="step-6-make-sure-snapshots-were-captured"></a>6. adım: anlık görüntü yakalanan emin olun
Anlık görüntü toplayıcısı günlüklerinde arama ```RequestSnapshots```.  Hiçbiri mevcutsa yapılandırmanızı denetleyin. Örneğin, *ThresholdForSnapshotting*, anlık görüntü topladıktan önce belirli bir özel durum sayısını belirtir.

### <a name="step-7-make-sure-snapshots-are-uploaded-correctly"></a>7. adım: anlık görüntü doğru bir şekilde karşıya emin olun
Anlık görüntü toplayıcısı günlüklerinde arama ```UploadSnapshotFinish```.  Bu mevcut değilse, e-posta Gönder snapshothelp@microsoft.com , Application Insights izleme anahtarı ile. Bu olay varsa, günlükleri birini açın ve özel verilerinde 'Snapshotıd' değerini kopyalayın. Ardından değeri arayın. Bu anlık görüntüye karşılık gelen özel durumu bulabilirsiniz. Özel durum ve açık hata ayıklama anlık görüntü'ye tıklayın. (Karşılık gelen hiçbir özel durum varsa, özel durum telemetrisi nedeniyle yüksek hacimli tümcelerindeki. Başka bir Snapshotıd deneyin.)

![Bul Snapshotıd ekran görüntüsü](./media/app-insights-troubleshoot-snapshot-debugger/002.png)

![Açık bir özel durumun ekran görüntüsü](./media/app-insights-troubleshoot-snapshot-debugger/004b.png)

![Açık ekran hata ayıklama anlık görüntüsü](./media/app-insights-troubleshoot-snapshot-debugger/003.png)

## <a name="snapshot-view-local-variables-are-not-complete"></a>Anlık görüntü görünümü yerel değişkenler tamamlanmamış

Bazı yerel değişkenler eksik. Uygulamanızı sürüm kodu çalıştırıyorsa, derleyicinin bazı değişkenler yerine iyileştirir. Örneğin:

```csharp
    const int a = 1; // a will be discarded by compiler and the value 1 will be inline.
    Random rand = new Random();
    int b = rand.Next() % 300; // b will be discarded and the value will be directly put to the 'FindNthPrimeNumber' call stack.
    long primeNumber = FindNthPrimeNumber(b);
```

Durumunuz farklı ise, bir hata olduğunu gösteriyor olabilir. E-posta Gönder snapshothelp@microsoft.com kod parçacığını yanı sıra, Application Insights izleme anahtarı ile.

## <a name="snapshot-view-cannot-obtain-value-of-the-local-variable-or-argument"></a>Anlık görüntü görünümü: yerel değişkenin veya bağımsız değişken değeri alınamıyor
Emin [Application Insights site uzantısı](https://www.siteextensions.net/packages/Microsoft.ApplicationInsights.AzureWebSites/) yüklenir. Sorun devam ederse, e-posta Gönder snapshothelp@microsoft.com , Application Insights izleme anahtarı ile.
