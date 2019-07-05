---
title: Azure Application Insights telemetri kanalları | Microsoft Docs
description: .NET ve .NET Core için Azure Application Insights SDK'ları telemetri kanalları özelleştirme yapma.
services: application-insights
documentationcenter: .net
author: cijothomas
manager: carmonm
ms.assetid: 015ab744-d514-42c0-8553-8410eef00368
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/14/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: af00641123354831c7bf174a743ded2886343579
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561358"
---
# <a name="telemetry-channels-in-application-insights"></a>Application Insights telemetri kanalları

Telemetri kanalları bir parçası olan [Azure Application Insights SDK'ları](../../azure-monitor/app/app-insights-overview.md). Bunlar, arabelleğe alma ve Application Insights hizmetine telemetri iletimini yönetin. .NET ve .NET Core SDK'ları iki yerleşik telemetri kanal sürümü: `InMemoryChannel` ve `ServerTelemetryChannel`. Bu makalede her kanal kanal davranışını özelleştirmek nasıl dahil olmak üzere ayrıntılı olarak açıklanır.

## <a name="what-are-telemetry-channels"></a>Telemetri kanallar nedir?

Telemetri Kanallar, telemetri öğelerinin arabelleğe alma ve sorgulama ve analiz için depolandıkları Application Insights hizmetine göndererek sorumludur. Uygulayan sınıfa bir telemetri kanalıdır [ `Microsoft.ApplicationInsights.ITelemetryChannel` ](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.channel.itelemetrychannel?view=azure-dotnet) arabirimi.

`Send(ITelemetry item)` Sonra tüm telemetri başlatıcılar telemetri kanal yöntemi çağrılır ve telemetri işlemci çağrılır. Bu nedenle, bir telemetri işlemcisi tarafından atılan tüm öğeler kanal ulaşın olmaz. `Send()` öğeleri genellikle arka uca anında göndermez. Genellikle, bunları arabelleğe ve verimli aktarım için toplu işler halinde gönderir.

[Canlı ölçümler Stream](live-stream.md) telemetri canlı akış'ı destekleyen özel bir kanalda de vardır. Bu kanal normal telemetri kanalı bağımsızdır ve bu belge için geçerli değildir.

## <a name="built-in-telemetry-channels"></a>Yerleşik telemetri kanallar

Application Insights .NET ve .NET Core SDK'ları iki yerleşik kanallarla gönderin:

* `InMemoryChannel`: Bunlar gönderildiniz kadar öğeleri arabelleğe basit kanal. Öğeler bellekte arabelleğe alınır ve her 30 saniyede Temizlenen ya da her 500 öğeleri arabelleğe alınır. Telemetri gönderme bir hatadan sonra yeniden değil çünkü bu kanal en alt düzeyde güvenilirlik garantisi sunar. Gönderilmemiş tüm öğeler kalıcı olarak uygulama kapatma sonrasında kayıp olduğundan bu kanal ayrıca öğeleri diskte korumayarak (normal veya değil). Bu kanal uygulayan bir `Flush()` herhangi bir bellek içi telemetri öğelerinin zaman uyumlu olarak zorla-temizlemesi için kullanılan yöntem. Bu kanal zaman uyumlu bir temizleme ideal olduğu kısa çalışan uygulamalar için oldukça uygundur.

    Bu kanal, daha büyük Microsoft.applicationınsights NuGet paketinin parçası olan ve başka hiçbir şey yapılandırıldığında, SDK kullanan varsayılan kanal.

* `ServerTelemetryChannel`: Yeniden deneme ilkelerini ve verileri bir yerel diskte depolama kapasitesine sahip daha gelişmiş bir kanal. Bu kanal, geçici bir hata oluşursa telemetri gönderme yeniden dener. Bu kanal, yerel disk depolama alanı ayrıca ağ kesintileri veya yüksek telemetri birimleri sırasında disk üzerinde öğeleri tutmak için kullanır. Bu yeniden deneme mekanizmaları ve yerel disk depolama alanı, nedeniyle bu kanal daha güvenilir olarak kabul edilir ve tüm üretim senaryoları için önerilir. Bu kanal için varsayılan değer [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) ve [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) resmi belgelerine göre yapılandırılan uygulamalar. Bu kanal sunucusu senaryoları ile uzun süre çalışan işlemler için optimize edilmiştir. [ `Flush()` ](#which-channel-should-i-use) Bu kanalı tarafından uygulanan yöntem zaman uyumlu değil.

    Bu kanal Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel NuGet paketi olarak sevk edilir ve Microsoft.applicationınsights.Web veya Microsoft.ApplicationInsights.AspNetCore NuGet kullandığınızda otomatik olarak alınır Paket.

## <a name="configure-a-telemetry-channel"></a>Telemetri kanal yapılandırma

Telemetri kanal, etkin telemetri yapılandırmaya ayarlayarak yapılandırın. ASP.NET uygulamaları için yapılandırma ayarını telemetri kanal örneği içerir `TelemetryConfiguration.Active`, veya değiştirerek `ApplicationInsights.config`. ASP.NET Core uygulamaları için bağımlılık ekleme kapsayıcısına kanal ekleme yapılandırması içerir.

Aşağıdaki bölümlerde, yapılandırma örnekler `StorageFolder` çeşitli uygulama türleri kanalda ayarlama. `StorageFolder` sadece yapılandırılabilir ayarları biridir. Yapılandırma ayarlarının tam listesi için bkz. [Ayarlar bölümünde](telemetry-channels.md#configurable-settings-in-channels) bu makalenin ilerleyen bölümlerinde.

### <a name="configuration-by-using-applicationinsightsconfig-for-aspnet-applications"></a>Applicationınsights.config dosyasını kullanarak ASP.NET uygulamaları için yapılandırma

Aşağıdaki bölüm gelen [Applicationınsights.config](configuration-with-applicationinsights-config.md) gösterir `ServerTelemetryChannel` kanalı ile yapılandırılmış `StorageFolder` özel bir konuma ayarlayın:

```xml
    <TelemetrySinks>
        <Add Name="default">
            <TelemetryProcessors>
                <!-- Telemetry processors omitted for brevity  -->
            </TelemetryProcessors>
            <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel">
                <StorageFolder>d:\temp\applicationinsights</StorageFolder>
            </TelemetryChannel>
        </Add>
    </TelemetrySinks>
```

### <a name="configuration-in-code-for-aspnet-applications"></a>ASP.NET uygulamaları için kod yapılandırma

Aşağıdaki kod bir 'ServerTelemetryChannel' örneğini ayarlar `StorageFolder` özel bir konuma ayarlayın. Bu kod uygulama başlangıcında genellikle eklemek `Application_Start()` Global.aspx.cs yöntemi.

```csharp
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;
protected void Application_Start()
{
    var serverTelemetryChannel = new ServerTelemetryChannel();
    serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
    serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
    TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
}
```

### <a name="configuration-in-code-for-aspnet-core-applications"></a>ASP.NET Core uygulamaları için kod yapılandırma

Değiştirme `ConfigureServices` yöntemi `Startup.cs` sınıfı burada gösterildiği gibi:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

public void ConfigureServices(IServiceCollection services)
{
    // This sets up ServerTelemetryChannel with StorageFolder set to a custom location.
    services.AddSingleton(typeof(ITelemetryChannel), new ServerTelemetryChannel() {StorageFolder = @"d:\temp\applicationinsights" });

    services.AddApplicationInsightsTelemetry();
}

```

> [!IMPORTANT]
> Kanal kullanarak yapılandırma `TelemetryConfiguration.Active` ASP.NET Core uygulamaları için önerilmez.

### <a name="configuration-in-code-for-netnet-core-console-applications"></a>.NET/.NET Core konsol uygulamaları için kod yapılandırma

Konsol uygulamaları için kod hem .NET hem de .NET Core için aynıdır:

```csharp
var serverTelemetryChannel = new ServerTelemetryChannel();
serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
```

## <a name="operational-details-of-servertelemetrychannel"></a>ServerTelemetryChannel işletimsel ayrıntıları

`ServerTelemetryChannel` bir bellek içi arabellek öğelerinde gelen depolar. Öğeleri sıralanmış, sıkıştırılmış ve içinde depolanan bir `Transmission` her 30 saniyede bir kez örnek veya ne zaman 500 öğeye arabelleğe alınan. Tek bir `Transmission` örneği en fazla 500 öğe içeriyor ve Application Insights hizmeti için tek bir HTTPS çağrısı üzerinden gönderdiği telemetriyi toplu temsil eder.

Varsayılan olarak, en fazla 10 `Transmission` örnekleri, paralel olarak gönderilebilir. Telemetri daha hızlı hızlarda gelen ya da ağ veya Application Insights yapıyorsanız son yavaş `Transmission` örnekleri bellekte depolanır. Bu bellek içi varsayılan kapasitesini `Transmission` arabellek olan 5 MB. Bellek içi kapasitesi aşıldığında `Transmission` örnekleri, 50 MB'ın bir sınıra kadar yerel diskte depolanır. `Transmission` ağ sorunları olduğunda örnekleri de yerel diskinizde saklanır. Bir yerel diskte depolanmış olan öğeler bir uygulama kilitlenme önceliklidir. Uygulama yeniden başlatıldığında gönderildiniz.

## <a name="configurable-settings-in-channels"></a>Kanal yapılandırılabilir ayarları

Her kanal için yapılandırılabilir ayarları tam listesi için bkz:

* [InMemoryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Channel/InMemoryChannel.cs)

* [ServerTelemetryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs)

İçin en yaygın olarak kullanılan ayarları şunlardır `ServerTelemetryChannel`:

1. `MaxTransmissionBufferCapacity`: En fazla bayt cinsinden bellek arabelleği iletimleri kanala tarafından kullanılan bellek miktarı. Bu kapasite üst sınırına ulaşıldığında, yeni öğeleri doğrudan yerel disk olarak depolanır. Varsayılan değer 5 MB'dir. Daha yüksek bir değere ayarlamak daha az disk kullanımı için müşteri adayları, ancak uygulama çökerse bellekte öğeleri kaybolacak unutmayın.

1. `MaxTransmissionSenderCapacity`: En fazla `Transmission` örnekleri aynı anda Application Insights'a gönderilir. Varsayılan değer 10'dur. Bu ayar, büyük hacimli telemetri oluşturulduğunda, önerilen daha yüksek bir sayı için yapılandırılabilir. Yüksek hacimli genellikle örnekleme ne zaman devre dışı veya yük testi sırasında gerçekleşir.

1. `StorageFolder`: Kanal tarafından öğeleri gerektiğinde diske depolamak için kullanılan klasör. Başka bir yol açıkça belirtilmediği takdirde Windows, % LOCALAPPDATA % veya % TEMP % kullanılır. Windows dışındaki ortamlarda, geçerli bir konum veya telemetri yerel diskinize kaydedilmez belirtmeniz gerekir.

## <a name="which-channel-should-i-use"></a>Hangi kanal kullanmalıyım?

`ServerTelemetryChannel` uzun süre çalışan uygulamalar içeren çoğu üretim senaryoları için önerilir. `Flush()` Tarafından gerçekleştirilen yöntem `ServerTelemetryChannel` zaman uyumlu değildir ve ayrıca tüm bekleyen öğeleri bellek veya disk gönderme garanti etmez. Bu kanal, uygulama hakkında kapatmak için olduğu senaryolarda kullanırsanız çağırdıktan sonra bazı gecikme tanıtmak öneririz `Flush()`. Tam gerektirebilir gecikmesi miktarı tahmin edilebilir değil. Ne kadar öğenin gibi etkenlere bağlıdır veya `Transmission` bellek, disk üzerinde ne kadar olduğunu, kaç arka uca aktarılıyor ve kanal ortasında üstel geri alma senaryolarını olup örnekleridir.

Zaman uyumlu bir temizleme yapmanız gerekiyorsa, kullanmanızı öneririz `InMemoryChannel`.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="does-the-application-insights-channel-guarantee-telemetry-delivery-if-not-what-are-the-scenarios-in-which-telemetry-can-be-lost"></a>Application Insights kanal telemetri teslimi garanti etmez? Aksi durumda, telemetri kaybolabilir senaryolar nelerdir?

Yerleşik kanalları hiçbiri arka ucuna telemetri teslim işlem türü garantisi sağlar kısa cevaptır. `ServerTelemetryChannel` Daha fazla ile karşılaştırılan Gelişmiş `InMemoryChannel` güvenilir teslim, ancak ayrıca yapar için telemetri göndermek için yalnızca bir mümkün olan en iyi girişim. Telemetri, yine de bu senaryoları da dahil olmak üzere çeşitli durumlarda kaybedilebilir:

1. Uygulama kilitlendiğinde bellek öğelerinde kaybolur.

1. Telemetri, genişletilmiş ağ sorunları dönemleri sırasında kaybolur. Telemetri, ağ kesintileri veya Application Insights arka ucu ile sorunları ortaya çıktığında sırasında yerel diske depolanır. Ancak, 24 saatten eski olan öğeleri atılır.

1. Windows telemetri depolamak için varsayılan disk % LOCALAPPDATA % veya % TEMP % konumlardır. Bu makinede genellikle yerel konumlardır. Uygulamayı fiziksel bir konumdan diğerine geçerse, özgün konumunda depolanan tüm telemetri kaybolur.

1. Windows üzerinde Web Apps'te varsayılan disk depolama D:\local\LocalAppData konumdur. Bu konum kalıcı değil. Bu uygulama yeniden başlatma, Ölçek çıkarmayı ve burada depolanan tüm telemetri kaybına neden böyle işlemler silindikten. Varsayılanı geçersiz kılmak ve kalıcı bir konuma D:\home gibi depolama belirtin. Ancak, böyle kalıcı konumları Uzaktaki Depolama Birimi tarafından sunulan ve bu nedenle yavaş olabilir.

### <a name="does-servertelemetrychannel-work-on-systems-other-than-windows"></a>ServerTelemetryChannel Windows dışındaki sistemleri üzerinde çalışır mı?

"WindowsServer" ad alanı ve paket adını bulunmasına karşın, bu kanal ve aşağıdaki özel durum dışında Windows sistemlerinde desteklenir. Windows dışındaki sistemlerinde kanal, yerel depolama klasörü varsayılan olarak oluşturmaz. Yerel depolama klasör oluşturun ve bunu kullanmak için bir kanal yapılandırmanız gerekir. Yerel depolama yapılandırıldıktan sonra kanal üzerindeki tüm sistemlerin aynı şekilde çalışır.

### <a name="does-the-sdk-create-temporary-local-storage-is-the-data-encrypted-at-storage"></a>SDK, yerel geçici depolama oluşturuyor mu? Veri depolama alanı şifrelenir?

SDK'sı telemetri öğelerinin azaltma sırasında veya ağ sorunları yerel depolama alanında depolar. Bu veriler, yerel olarak şifrelenmiş değil.

Windows sistemleri için SDK otomatik olarak geçici bir yerel klasöre % TEMP % veya % LOCALAPPDATA % dizinini oluşturur ve yöneticiler ve yalnızca geçerli kullanıcı için erişimi kısıtlar.

Sistemler için Windows dışındaki, hiçbir yerel depolama SDK'sı tarafından otomatik olarak oluşturulur ve bu nedenle hiçbir veri varsayılan olarak yerel olarak depolanır. Bir depolama dizini kendiniz oluşturabilir ve kanal kullanmak için yapılandırın. Bu durumda, dizin sağlandığını sağlamak için sorumlu olursunuz.
Daha fazla bilgi edinin [veri koruma ve gizlilik](data-retention-privacy.md#does-the-sdk-create-temporary-local-storage).

## <a name="open-source-sdk"></a>Açık kaynak SDK'sı
Application Insights için her bir SDK gibi Kanallar, açık kaynaklıdır. Okuma ve kod veya sorunları bildirmek için katkıda bulunan [resmi GitHub deposunu](https://github.com/Microsoft/ApplicationInsights-dotnet).

## <a name="next-steps"></a>Sonraki adımlar

* [Örnekleme](../../azure-monitor/app/sampling.md)
* [SDK sorunlarını giderme](../../azure-monitor/app/asp-net-troubleshoot-no-data.md)
