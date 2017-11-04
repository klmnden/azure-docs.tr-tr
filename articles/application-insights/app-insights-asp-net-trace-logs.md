---
title: ".NET izleme günlükleri Application ınsights'ta keşfedin"
description: "İzleme, NLog veya Log4Net ile oluşturulan günlükleri arayın."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0c2a084f-6e71-467b-a6aa-4ab222f17153
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: mbullwin
ms.openlocfilehash: 6da0bf009fa71885d7d8e3bd5376c5a7c9d4a344
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="explore-net-trace-logs-in-application-insights"></a>.NET izleme günlükleri Application ınsights'ta keşfedin
NLog, log4Net veya System.Diagnostics.Trace, ASP.NET uygulamanızda Tanılama izleme için kullanıyorsanız günlüklerinizi gönderilmesini sağlayabilirsiniz [Azure Application Insights][start], burada keşfedin aramak ve bunları. Günlüklerinizi, böylece her kullanıcı isteği hizmeti ile ilişkilendirilmiş izlemeleri belirlemek ve diğer olayları ve özel durum raporları ile ilişkilendirmek, uygulamadan gelen telemetri ile birleştirilir.

> [!NOTE]
> Günlük yakalama modülü gerekiyor mu? 3. taraf günlükçüleri için yararlı bir bağdaştırıcı olduğundan, ancak NLog kullanmıyorsanız, log4Net veya System.Diagnostics.Trace, göz önünde bulundurun yalnızca arama [uygulama Öngörüler TrackTrace()](app-insights-api-custom-events-metrics.md#tracktrace) doğrudan.
>
>

## <a name="install-logging-on-your-app"></a>Uygulamanıza oturum yükleyin
Seçilen günlük framework projenize yükleyin. Bu app.config veya web.config bir girişe neden.

System.Diagnostics.Trace kullanıyorsanız, web.config dosyasına bir giriş eklemeniz gerekir:

```XML

    <configuration>
     <system.diagnostics>
       <trace autoflush="false" indentsize="4">
         <listeners>
           <add name="myListener"
             type="System.Diagnostics.TextWriterTraceListener"
             initializeData="TextWriterOutput.log" />
           <remove name="Default" />
         </listeners>
       </trace>
     </system.diagnostics>
   </configuration>
```
## <a name="configure-application-insights-to-collect-logs"></a>Application Insights'ın günlükleri toplamak için yapılandırma
**[Application Insights projenize eklemek](app-insights-asp-net.md)**  bunu, henüz yapmadıysanız. Günlük Toplayıcı dahil etmek için bir seçenek görürsünüz.

Veya **yapılandırma Application Insights** Çözüm Gezgini'nde projenize sağ tarafından. Seçeneğini **izleme koleksiyonu yapılandırma**.

*Application Insights menüsü veya günlük Toplayıcı seçeneği yok?* Deneyin [sorun giderme](#troubleshooting).

## <a name="manual-installation"></a>El ile yükleme
Proje türü (örneğin Windows Masaüstü projesi) Application Insights yükleyici tarafından desteklenmiyor, bu yöntemi kullanın.

1. Log4Net veya NLog kullanmayı planlıyorsanız, projenize yükleyin.
2. Çözüm Gezgini'nde, projenize sağ tıklayın ve seçin **NuGet paketlerini Yönet**.
3. "Application Insights" araması yapın
4. Uygun paket - aşağıdakilerden birini seçin:

   * Microsoft.ApplicationInsights.TraceListener (System.Diagnostics.Trace çağrıları yakalamak için)
   * Microsoft.ApplicationInsights.EventSourceListener (EventSource olaylarını yakalamak için)
   * Microsoft.ApplicationInsights.EtwListener (ETW olaylarını yakalamak için)
   * Microsoft.ApplicationInsights.NLogTarget
   * Microsoft.ApplicationInsights.Log4NetAppender

NuGet paketi gerekli derlemeleri yükler ve ayrıca web.config veya app.config değiştirir.

## <a name="insert-diagnostic-log-calls"></a>Tanılama günlük çağrıları Ekle
System.Diagnostics.Trace kullanırsanız, tipik bir çağrı olacaktır:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Log4net veya NLog isterseniz:

    logger.Warn("Slow response - database01");

## <a name="using-eventsource-events"></a>EventSource olaylarını kullanma
Yapılandırabileceğiniz [System.Diagnostics.Tracing.EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) Application Insights izlemeleri olarak gönderilmesini olaylar. İlk olarak, yükleme `Microsoft.ApplicationInsights.EventSourceListener` NuGet paketi. Daha sonra Düzenle `TelemetryModules` bölümünü [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md) dosya.

```xml
    <Add Type="Microsoft.ApplicationInsights.EventSourceListener.EventSourceTelemetryModule, Microsoft.ApplicationInsights.EventSourceListener">
      <Sources>
        <Add Name="MyCompany" Level="Verbose" />
      </Sources>
    </Add>
```

Her bir kaynağı için şu parametreleri ayarlayabilirsiniz:
 * `Name`Toplanacak EventSource adını belirtir.
 * `Level`Toplanacak günlüğe kaydetme düzeyini belirtir. Aşağıdakilerden biri olabilir `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords`(İsteğe bağlı) kullanmak için anahtar sözcükler birleşimleri tamsayı değerini belirtir.

## <a name="using-diagnosticsource-events"></a>DiagnosticSource olaylarını kullanma
Yapılandırabileceğiniz [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) Application Insights izlemeleri olarak gönderilmesini olaylar. İlk olarak, yükleme [ `Microsoft.ApplicationInsights.DiagnosticSourceListener` ](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener) NuGet paketi. Daha sonra Düzenle `TelemetryModules` bölümünü [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md) dosya.

```xml
    <Add Type="Microsoft.ApplicationInsights.DiagnsoticSourceListener.DiagnosticSourceTelemetryModule, Microsoft.ApplicationInsights.DiagnosticSourceListener">
      <Sources>
        <Add Name="MyDiagnosticSourceName" />
      </Sources>
    </Add>
```

İzlemek istediğiniz her DiagnosticSource için olan bir giriş eklemek `Name` özniteliği, DiagnosticSource adına ayarlayın.

## <a name="using-etw-events"></a>ETW olayları kullanma
Application Insights izlemeleri olarak gönderilmesini ETW olayları yapılandırabilirsiniz. İlk olarak, yükleme `Microsoft.ApplicationInsights.EtwCollector` NuGet paketi. Daha sonra Düzenle `TelemetryModules` bölümünü [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md) dosya.

> [!NOTE] 
> SDK barındırma işlemi, "Performans günlüğü kullanıcıları" veya yöneticileri üyesi olan bir kimlik altında çalışıyorsa, ETW olayları yalnızca toplanabilir.

```xml
    <Add Type="Microsoft.ApplicationInsights.EtwCollector.EtwCollectorTelemetryModule, Microsoft.ApplicationInsights.EtwCollector">
      <Sources>
        <Add ProviderName="MyCompanyEventSourceName" Level="Verbose" />
      </Sources>
    </Add>
```

Her bir kaynağı için şu parametreleri ayarlayabilirsiniz:
 * `ProviderName`Toplanacak ETW sağlayıcı adıdır.
 * `ProviderGuid`Toplama ETW sağlayıcı GUID belirtir yerine kullanılabilir `ProviderName`.
 * `Level`Toplanacak günlüğe kaydetme düzeyi ayarlar. Aşağıdakilerden biri olabilir `Critical`, `Error`, `Informational`, `LogAlways`, `Verbose`, `Warning`.
 * `Keywords`(İsteğe bağlı) kullanmak için anahtar sözcüğü birleşimleri tamsayı değerini ayarlar.

## <a name="using-the-trace-api-directly"></a>Kullanarak doğrudan API izleme
Application Insights izleme API doğrudan çağırabilir. Günlüğe kaydetme bağdaştırıcıları bu API'yi kullanın.

Örneğin:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

TrackTrace avantajı, iletide oldukça uzun veri koyabilirsiniz ' dir. Örneğin, gönderme verisi kodlayın.

Ayrıca, bir önem düzeyi iletinize ekleyebilirsiniz. Ve diğer telemetri gibi filtre veya arama izlemeleri farklı kümeleri için yardımcı olmak için kullanabileceğiniz özellik değerlerini ekleyebilirsiniz. Örneğin:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });

Bu, içinde sağlayacağı [arama][diagnostic], belirli bir veritabanı ile ilgili belirli bir önemde tüm iletileri kolayca filtrelemek için.

## <a name="explore-your-logs"></a>Günlüklerinizi keşfedin
Uygulamanızı, ya da hata ayıklama modunda çalıştırmak veya dinamik dağıtın.

Uygulamanızın genel bakış dikey penceresinde de [Application Insights portalındaki][portal], seçin [arama][diagnostic].

![Application Insights'ta arama seçin](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![Arama](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

Örneğin yapabilecekleriniz:

* Günlük izlemelerini veya belirli özelliklere sahip öğeleri Filtrele
* Belirli bir öğeyi ayrıntılı inceleyin.
* Aynı kullanıcı istekle ilgili diğer telemetriyi Bul (diğer bir deyişle, aynı Operationıd ile)
* Bu sayfa yapılandırmasını sık kullanılan olarak Kaydet

> [!NOTE]
> **Örnekleme.** Uygulamanız çok miktarda veri gönderiyorsa ve ASP.NET sürüm 2.0.0-beta3 veya daha sonraki uygulamalar için Application Insights SDK kullanıyorsanız, uyarlamalı örnekleme özelliği telemetrenizin yalnızca yüzdesini çalıştırır ve gönderir. [Örnekleme hakkında daha fazla bilgi edinin.](app-insights-sampling.md)
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Hataları ve ASP.NET özel durumları tanılama][exceptions]

[Arama hakkında daha fazla bilgi][diagnostic].

## <a name="troubleshooting"></a>Sorun giderme
### <a name="how-do-i-do-this-for-java"></a>Bunu nasıl yapabilirim Java için?
Kullanım [Java günlük bağdaştırıcıları](app-insights-java-trace-logs.md).

### <a name="theres-no-application-insights-option-on-the-project-context-menu"></a>Proje bağlam menüsünde Application Insights seçeneği yoktur
* Application Insights araçları bu geliştirme makinede yüklü denetleyin. Visual Studio menü Araçlar, uzantılar ve güncelleştirmeler, için Application Insights araçları arayın. Yüklü sekmesinde değilse, çevrimiçi sekmesini açın ve yükleyin.
* Bu proje Application Insights araçları tarafından desteklenmeyen bir türde olabilir. Kullanım [el ile yükleme](#manual-installation).

### <a name="no-log-adapter-option-in-the-configuration-tool"></a>Hiçbir yapılandırma aracı günlük bağdaştırıcısı seçeneğinde
* Günlüğe kaydetme framework yüklemeniz gerekir.
* System.Diagnostics.Trace kullanıyorsanız, emin [içinde yapılandırılmış `web.config` ](https://msdn.microsoft.com/library/system.diagnostics.eventlogtracelistener.aspx).
* Application Insights'ın en son sürümünü var mı? Visual Studio'da **Araçları** menüsünde seçin **Uzantılar ve güncelleştirmeler**, açarak **güncelleştirmeleri** sekmesi. Geliştirici analiz araçları vardır, güncelleştirmek için'yı tıklatın.

### <a name="emptykey"></a>"İzleme anahtarını boş olamaz" hata alıyorum
Application Insights yüklemeden günlük bağdaştırıcısı Nuget paketi yüklü gibi görünüyor.

Çözüm Gezgini'nde sağ `ApplicationInsights.config` ve **güncelleştirme Application Insights**. Azure'da oturum açmanız başvurulmasını bir iletişim kutusu elde edersiniz ve ya da bir Application Insights kaynağı oluşturun veya var olan bir yeniden kullanın. Bu sorununu çözer.

### <a name="i-can-see-traces-in-diagnostic-search-but-not-the-other-events"></a>Tanılama arama, ancak olmayan diğer olayları izlemelerini görmek için
Bu, bazen tüm olayları ve istek ardışık düzenine ulaşması biraz zaman alabilir.

### <a name="limits"></a>Ne kadar veri tutulur?
Pek çok etken korunur veri miktarını etkileyebilir. Bkz: [sınırları](app-insights-api-custom-events-metrics.md#limits) daha fazla bilgi için müşteri olay ölçümleri sayfasının bölümünde. 

### <a name="im-not-seeing-some-of-the-log-entries-that-i-expect"></a>Bazı t beklediğiniz günlük girdileri görüyorum değil
Uygulamanız çok miktarda veri gönderiyorsa ve ASP.NET sürüm 2.0.0-beta3 veya daha sonraki uygulamalar için Application Insights SDK kullanıyorsanız, uyarlamalı örnekleme özelliği telemetrenizin yalnızca yüzdesini çalıştırır ve gönderir. [Örnekleme hakkında daha fazla bilgi edinin.](app-insights-sampling.md)

## <a name="add"></a>Sonraki adımlar
* [Kullanılabilirlik ve yanıt hızını testleri ayarlama][availability]
* [Sorun giderme][qna]

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[portal]: https://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
