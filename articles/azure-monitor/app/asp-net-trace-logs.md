---
title: .NET izleme günlüklerini Application ınsights'ı keşfedin
description: İzleme, NLog veya Log4Net tarafından oluşturulan günlükleri arayın.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: mbullwin
ms.openlocfilehash: 74cb1b3ec4e0570aa4316e6f45e99719f36815d1
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65150709"
---
# <a name="explore-netnet-core-trace-logs-in-application-insights"></a>Application Insights izleme günlükleri.NET/.NET Core keşfedin

Tanılama izleme günlüklerini ASP.NET/ASP.NET Core uygulamanız için ILogger, NLog, log4Net veya System.Diagnostics.Trace için gönderin [Azure Application Insights][start]. Ardından, keşfedin ve bunları arayın. Her kullanıcı isteği ile ilişkilendirilmiş ve bunları diğer olayları ve özel durum raporları ilişkilendirmek izlemeler belirleyebilmeniz Bu günlükleri, uygulamanızın diğer günlük dosyalarından birleştirilir.

> [!NOTE]
> Günlük yakalama modülü gerekiyor mu? Üçüncü taraf günlükçüler için kullanışlı bir bağdaştırıcıdır. Ancak, NLog, log4Net veya System.Diagnostics.Trace zaten kullanmıyorsanız, yalnızca çağırma göz önünde bulundurun [ **Application Insights TrackTrace()** ](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace) doğrudan.
>
>
## <a name="install-logging-on-your-app"></a>Uygulamanıza günlük yükle
Seçilen günlük Çerçevenizi app.config veya web.config bir girişe neden projenize yükleyin.

```XML
    <configuration>
      <system.diagnostics>
    <trace autoflush="true" indentsize="0">
      <listeners>
        <add name="myAppInsightsListener" type="Microsoft.ApplicationInsights.TraceListener.ApplicationInsightsTraceListener, Microsoft.ApplicationInsights.TraceListener" />
      </listeners>
    </trace>
  </system.diagnostics>
   </configuration>
```

## <a name="configure-application-insights-to-collect-logs"></a>Application Insights'ı günlükleri toplamak için yapılandırma
[Projenize Application Insights ekleme](../../azure-monitor/app/asp-net.md) bunu, henüz yapmadıysanız. Günlük toplayıcıyı dahil etmek için bir seçenek göreceksiniz.

Ya da Çözüm Gezgini'nde projenize sağ tıklayıp **Application ınsights'ı Yapılandır**. Seçin **izleme koleksiyonunu yapılandırma** seçeneği.

> [!NOTE]
> Application Insights menüsü ya da günlük Toplayıcı seçeneği yok? Deneyin [sorun giderme](#troubleshooting).

## <a name="manual-installation"></a>El ile yükleme
Proje türünüzü (örneğin bir Windows Masaüstü Proje) Application Insights yükleyicisi tarafından desteklenmiyor, bu yöntemi kullanın.

1. Log4Net veya NLog kullanmayı planlıyorsanız, projenize yükleyin.
2. Çözüm Gezgini'nde projenize sağ tıklayın ve seçin **NuGet paketlerini Yönet**.
3. "Application Insights" için arama
4. Aşağıdaki paketlerden birini seçin:

   - ILogger için: [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.Extensions.Logging.ApplicationInsights.svg)](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights/)
   - NLog için: [Microsoft.ApplicationInsights.NLogTarget](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.NLogTarget.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
   - Log4Net için: [Microsoft.ApplicationInsights.Log4NetAppender](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.Log4NetAppender.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
   - System.Diagnostics için: [Microsoft.ApplicationInsights.TraceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.TraceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/)
   - [Microsoft.ApplicationInsights.DiagnosticSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.DiagnosticSourceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/)
   - [Microsoft.ApplicationInsights.EtwCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/)
[![NuGet](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.EtwCollector.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/)
   - [Microsoft.ApplicationInsights.EventSourceListener](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/)
[![Nuget](https://img.shields.io/nuget/vpre/Microsoft.ApplicationInsights.EventSourceListener.svg)](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/)

NuGet paketi, gerekli bütünleştirilmiş kodları yükler ve web.config veya app.config uygulanabilirse değiştirir.

## <a name="ilogger"></a>ILogger

Konsol uygulamaları ve ASP.NET Core ile Application Insights ILogger uygulamasını kullanma örnekleri için bkz: [için .NET Core ILogger ApplicationInsightsLoggerProvider günlükleri](ilogger.md).

## <a name="insert-diagnostic-log-calls"></a>Tanılama Günlüğü çağrıları Ekle
System.Diagnostics.Trace kullanıyorsanız, tipik bir çağrı olabilir:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Log4net veya NLog tercih ederseniz, bu seçeneği kullanın:

    logger.Warn("Slow response - database01");

## <a name="use-eventsource-events"></a>EventSource olaylarını kullanın
Yapılandırabileceğiniz [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) izlemeleri olarak Application ınsights'a gönderilmek üzere olayları. İlk olarak, yükleme `Microsoft.ApplicationInsights.EventSourceListener` NuGet paketi. Ardından Düzenle `TelemetryModules` bölümünü [Applicationınsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) dosya.

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Her kaynak için aşağıdaki parametreleri ayarlayabilirsiniz:
 * **Adı** toplanacak EventSource adını belirtir.
 * **Düzey** toplamak için günlüğe kaydetme düzeyini belirtir: *Kritik*, *hata*, *bilgilendirici*, *LogAlways*, *ayrıntılı*, veya *uyarı*.
 * **Anahtar sözcükler** (isteğe bağlı) anahtar sözcüğü birleşimleri kullanmak için tamsayı değeri belirtin.

## <a name="use-diagnosticsource-events"></a>DiagnosticSource olayları kullanma
Yapılandırabileceğiniz [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) izlemeleri olarak Application ınsights'a gönderilmek üzere olayları. İlk olarak, yükleme [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet paketi. Ardından "TelemetryModules" bölümünü düzenleyin [Applicationınsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) dosya.

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnosticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

İzlemek istediğiniz her DiagnosticSource için sahip bir girdi Ekle **adı** özniteliği, DiagnosticSource adına ayarlayın.

## <a name="use-etw-events"></a>ETW olayları kullanma
Application Insights izlemelerini olarak gönderilmesi için olay izleme için Windows (ETW) olayları yapılandırabilirsiniz. İlk olarak, yükleme `Microsoft.ApplicationInsights.EtwCollector` NuGet paketi. Ardından "TelemetryModules" bölümünü düzenleyin [Applicationınsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) dosya.

> [!NOTE] 
> SDK'yı barındıran işlem performans günlük kullanıcıları veya yöneticileri üyesi olan bir kimlik altında çalışıyorsa, ETW olayları sadece toplanabilir.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Her kaynak için aşağıdaki parametreleri ayarlayabilirsiniz:
 * **ProviderName** toplanacak ETW sağlayıcısı adıdır.
 * **SağlayıcıGUID** toplanacak ETW sağlayıcısı GUID belirtir. Yerine kullanılabilir `ProviderName`.
 * **Düzey** toplamak için günlüğe kaydetme düzeyini ayarlar. Bu olabilir *kritik*, *hata*, *bilgilendirici*, *LogAlways*, *ayrıntılı*, veya *Uyarı*.
 * **Anahtar sözcükler** kullanılacak anahtar sözcüğü birleşimleri tamsayı değerini (isteğe bağlı) olarak ayarlayın.

## <a name="use-the-trace-api-directly"></a>API izleme doğrudan kullanın
Application Insights izleme API'sini doğrudan çağırabilir. Günlüğe kaydetme bağdaştırıcıları bu API'yi kullanın.

Örneğin:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

TrackTrace bir avantajı, iletide görece uzun veri koyabilirsiniz ' dir. Örneğin, gönderme verisi şifreleyebilirsiniz.

Bu gibi durumlarda, bir önem düzeyi ayrıca mesajınızı ekleyebilirsiniz. Ve diğer telemetriyi gibi filtre veya arama izlemeler farklı kümeleri için yardımcı olması için özellik değerlerini ekleyebilirsiniz. Örneğin:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Bu, kolayca filtrelemek için sağlayacak [arama] [ diagnostic] için belirli bir veritabanı ile ilgili tüm iletileri belirli bir önem düzeyi.

## <a name="explore-your-logs"></a>Günlüklerinizi keşfedin
Uygulamanızı hata ayıklama modunda çalıştırabilir veya canlı dağıtın.

Uygulamanızın genel bakış bölmesinde [Application Insights portalında][portal]seçin [arama][diagnostic].

Örneğin yapabilecekleriniz:

* Günlük izlemelerini veya belirli özelliklere sahip öğeleri filtreleyin.
* Belirli bir öğeyi ayrıntılı olarak inceleyin.
* Aynı kullanıcı isteği ilgili diğer sistem günlük verilerini bulma (aynı Operationıd'dir).
* Bir sayfa yapılandırmasını bir sık kullanılan olarak kaydedebilirsiniz.

> [!NOTE]
>Uygulamanız çok miktarda veri gönderir ve ASP.NET sürüm 2.0.0-beta3 veya daha sonra Application Insights SDK kullanıyorsanız *Uyarlamalı örnekleme* özellik çalışır ve yalnızca bir kısmını telemetrinizi gönderme. [Örnekleme hakkında daha fazla bilgi edinin.](../../azure-monitor/app/sampling.md)
>

## <a name="troubleshooting"></a>Sorun giderme
### <a name="how-do-i-do-this-for-java"></a>Nasıl Java için yapmam gerekir?
Kullanım [Java günlük bağdaştırıcıları](../../azure-monitor/app/java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a>Proje bağlam menüsünde Application Insights seçeneği yoktur
* Application Insights araçları geliştirme makinenizde yüklü olduğundan emin olun. Visual Studio, **Araçları** > **Uzantılar ve güncelleştirmeler**, Aranan **Application Insights Araçları**. Üzerinde çalışmıyorsa **yüklü** sekmesini **çevrimiçi** sekme ve yükleyin.
* Bu Application Insights araçları desteği olmayan bir proje türü olabilir. Kullanım [el ile yükleme](#manual-installation).

### <a name="theres-no-log-adapter-option-in-the-configuration-tool"></a>Yapılandırma aracında günlük bağdaştırıcısı seçeneği yoktur
* Günlüğe kaydetme çerçevesi ilk yükleyin.
* System.Diagnostics.Trace kullanıyorsanız, bunu sahip olduğunuzdan emin olun [yapılandırılan *web.config*](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Application ınsights'ı en son sürümüne sahip olduğunuzdan emin olun. Visual Studio'da Git **Araçları** > **Uzantılar ve güncelleştirmeler**açın **güncelleştirmeleri** sekmesi. Varsa **Developer Analytics Tools** olduğundan, onu güncelleştirmek için seçin.

### <a name="emptykey"></a>"İzleme anahtarını boş bırakılamaz" hata iletisi görüntüleniyor.
Büyük olasılıkla günlük bağdaştırıcısı Nuget paketi Application Insights'ı yüklemeden yüklü. Çözüm Gezgini'nde sağ *Applicationınsights.config*seçip **güncelleştirme Application Insights**. Azure'da oturum açın ve bir Application Insights kaynağı oluşturun veya mevcut bir yeniden istenir. Bu sorunu çözer.

### <a name="i-can-see-traces-but-not-other-events-in-diagnostic-search"></a>İzlemeleri ancak değil tanılama araması diğer olayları görebilirsiniz
Uygulamanın tüm olayları ve istek ardışık düzenine ulaşması biraz sürebilir.

### <a name="limits"></a>Ne kadar veri tutulur?
Çeşitli faktörler, korunan veri miktarı etkiler. Daha fazla bilgi için [sınırları](../../azure-monitor/app/api-custom-events-metrics.md#limits) müşteri olay ölçümleri sayfasının bölümünde.

### <a name="i-dont-see-some-log-entries-that-i-expected"></a>Beklediğim bazı günlük girişlerini göremiyorum
Uygulamanızı voluminous miktarda veriyi gönderir ve ASP.NET sürüm 2.0.0-beta3 veya daha sonra Application Insights SDK kullanıyorsanız, Uyarlamalı örnekleme özelliği çalışır ve yalnızca bir kısmını telemetrinizi gönderme. [Örnekleme hakkında daha fazla bilgi edinin.](../../azure-monitor/app/sampling.md)

## <a name="add"></a>Sonraki adımlar

* [Hataları ve ASP.NET özel durumları tanılama][exceptions]
* [Arama hakkında daha fazla bilgi edinin][diagnostic]
* [Kullanılabilirlik ve yanıt hızını testleri ayarlama][availability]
* [Sorun giderme][qna]

<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[exceptions]: asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[start]: ../../azure-monitor/app/app-insights-overview.md