---
title: Azure Application Insights telemetri kanalları | Microsoft Docs
description: .NET/.NET Core için Azure Application Insights SDK'ları telemetri kanalları özelleştirme yapma.
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
ms.openlocfilehash: fa99e29e9ec6c2bef7296a50dd381ee5fc60a1cb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66255816"
---
# <a name="telemetrychannel-in-application-insights"></a>Application ınsights'ta TelemetryChannel

TelemetryChannel bir parçası olan [Azure Application Insights SDK'ları](../../azure-monitor/app/app-insights-overview.md). Bu, arabelleğe alma ve Application Insights hizmetine telemetri iletimini yönetir. .NET ve .NET Core SDK'ları iki yerleşik TelemetryChannels - sürümü `InMemoryChannel` ve `ServerTelemetryChannel`. Bu makalede her kanal kullanıcılar kanal davranışını nasıl özelleştirebileceğinizi dahil olmak üzere ayrıntılı olarak açıklanır.

## <a name="what-is-a-telemetrychannel"></a>Bir TelemetryChannel nedir?

`TelemetryChannel` arabelleğe alma ve sorgulama ve analiz için nerede depolandıklarından Application Insights hizmetine telemetri öğelerinin gönderme sorumludur. Arabirimini uygulayan herhangi bir sınıf olan [`Microsoft.ApplicationInsights.ITelemetryChannel`](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.channel.itelemetrychannel?view=azure-dotnet)

`Send(ITelemetry item)` TelemetryChannel yöntemi tüm adlı `TelemetryInitializer`s ve `TelemetryProcessor`s çağrılır. Buna herhangi bir öğe bırakılan `TelemetryProcessor` kanal ulaşmak olmaz. `Send()` genellikle öğeleri için arka uç anında göndermez. Bunlar genellikle arabelleğe alınan bellek içi ve verimli aktarım için toplu işler halinde gönderilir.

[LiveMetrics](live-stream.md) telemetri canlı akış'ı destekleyen özel bir kanal de vardır. Bu kanal normal telemetri kanalı bağımsızdır ve bu belge tarafından kullanılan kanal uygulanmaz `LiveMetrics`.

## <a name="built-in-telemetrychannels"></a>Yerleşik TelemetryChannels

Application Insights.NET/.NET Core SDK'sı, iki yerleşik kanallar ile birlikte gelir:

* **InMemoryChannel** 
 `InMemoryChannel` gönderilene kadar olan öğeleri arabelleğe hafif kanal. Öğeler bellekte arabelleğe alınır ve her 30 saniyede Temizlenen veya 500 öğeleri arabelleğe alınan herhangi bir zamanda. Telemetri gönderme hataları sırasında yeniden değil olarak bu kanal en alt düzeyde güvenilirlik garantisi sunar. Gönderilmemiş tüm öğeler kalıcı olarak uygulama kapatma sonrasında kayıp olduğundan (düzgün bir şekilde veya değil) bu kanal diskte, öğeleri sakla değil. Var olan bir `Flush()` herhangi bir bellek içi telemetri öğelerinin zaman uyumlu olarak zorla-temizlemesi için kullanılan bu kanalı tarafından uygulanan yöntem. Bu zaman uyumlu bir temizleme ideal olduğu kısa çalışan uygulamalar için uygundur.

    Bu kanal parçası olarak gönderildiğini `Microsoft.ApplicationInsights` nuget paketini kendisi ve SDK'sı kullanan başka bir şey yapılandırıldığında varsayılan kanal.

* **ServerTelemetryChannel** 
 `ServerTelemetryChannel` yeniden deneme ilkelerini ve yerel diskte veri depolama kapasitesine sahip daha gelişmiş bir kanaldır. Bu kanal, geçici bir hata oluşursa telemetri gönderme yeniden dener. Bu kanal, yerel disk depolama alanı ayrıca ağ kesintileri veya yüksek telemetri birimleri sırasında disk üzerinde öğeleri tutmak için kullanır. Bu yeniden deneme mekanizmaları ve yerel disk depolama alanı, nedeniyle bu kanal daha güvenilir olarak kabul edilir ve tüm üretim senaryoları için önerilir. Bu kanal için varsayılan değer [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) ve [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) bağlı resmi belgeleri göre yapılandırılmış olan uygulamalar. Bu kanal, uzun süre çalışan işlemler sunucusu senaryoları için optimize edilmiştir. [ `Flush()` ](#which-channel-should-i-use) Bu kanalı tarafından uygulanan yöntem zaman uyumlu değil.

    Bu kanal, NuGet paketi olarak sevk edilir `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel`ve NuGet paketlerini birini kullanırken otomatik olarak getirildikten `Microsoft.ApplicationInsights.Web` veya `Microsoft.ApplicationInsights.AspNetCore`.

## <a name="configuring-telemetrychannel"></a>TelemetryChannel yapılandırma

İstenen ayarlayarak telemetri kanal yapılandırılabilir `TelemetryChannel` etkin `TelemetryConfiguration`. Asp.Net uygulamaları için yapılandırma ayarı içerir `TelemetryChannel` üzerinde `TelemetryConfiguration.Active`, ya da değiştirirken `ApplicationInsights.config`. ASP.NET Core uygulamaları için istenen kanal bağımlılık ekleme kapsayıcısına ekleme yapılandırması içerir.

Aşağıdaki örnek kullanıcı burada yapılandırıyor gösterir `StorageFolder` kanalı. `StorageFolder` sadece yapılandırılabilir ayarları biridir. Yapılandırma ayarlarının tam listesi açıklanan [Ayarlar bölümünde](telemetry-channels.md#configurable-settings-in-channel).

## <a name="configuration-using-applicationinsightsconfig-for-aspnet-applications"></a>Applicationınsights.config dosyasını kullanarak ASP.NET uygulamaları için yapılandırma

Aşağıdaki bölüm gelen [Applicationınsights.config](configuration-with-applicationinsights-config.md) ile yapılandırılmış ServerTelemetryChannel gösterir `StorageFolder` özel bir konuma ayarlayın.

```xml
    <TelemetrySinks>
        <Add Name="default">
            <TelemetryProcessors>
                <!--TelemetryProcessors omitted for brevity  -->
            </TelemetryProcessors>
            <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel">
                <StorageFolder>d:\temp\applicationinsights</StorageFolder>
            </TelemetryChannel>
        </Add>
    </TelemetrySinks>
```

## <a name="configuration-in-code-for-aspnet-applications"></a>ASP.NET uygulamaları için kod yapılandırma

Aşağıdaki kod ile ServerTelemetryChannel ayarlar `StorageFolder` özel bir konuma ayarlayın. Bu kod, genellikle de Application_Start() yönteminde uygulama başına eklenmelidir `Global.aspx.cs`

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

## <a name="configuration-in-code-for-aspnet-core-applications"></a>ASP.NET Core uygulamaları için kod yapılandırma

Değiştirme `ConfigureServices` yöntemi `Startup.cs` aşağıda gösterildiği gibi sınıf.

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

> [!NOTE]
> Bu kanal kullanarak yapılandırma dikkat edin önemlidir `TelemetryConfiguration.Active` ASP.NET Core uygulamaları için önerilmez.

## <a name="configuring-telemetrychannel-in-code-for-netnet-core-console-applications"></a>TelemetryChannel kodda.NET/.NET Core konsol uygulamaları için yapılandırma

Kod, konsol uygulamaları için .NET ve .NET Core için aynı ve aşağıda gösterilmiştir.

```csharp
var serverTelemetryChannel = new ServerTelemetryChannel();
serverTelemetryChannel.StorageFolder = @"d:\temp\applicationinsights";
serverTelemetryChannel.Initialize(TelemetryConfiguration.Active);
TelemetryConfiguration.Active.TelemetryChannel = serverTelemetryChannel;
```

## <a name="operational-details-of-servertelemetrychannel"></a>ServerTelemetryChannel işletimsel ayrıntıları

`ServerTelemetryChannel` Gelen öğeleri bir bellek içi arabellek arabelleğe alır. Seri hale getirilmiş, sıkıştırılır ve içinde depolanan `Transmission` her 30 saniye veya zaman 500 öğe arabelleğe sonra örnek. Tek bir `Transmission` örneği en fazla 500 öğelerini içeren ve Application Insights hizmeti tek https çağrısı üzerinden gönderilen telemetri toplu temsil eder. Varsayılan olarak, olabilir en fazla 10 `Transmission`s olan paralel olarak gönderilir. Telemetri, daha hızlı hızlarda gelen veya ağ/Application Insights arka uç ardından yavaş ise, `Transmission`s belleğe depolanan. Bu, bellek aktarımı arabelleğinin varsayılan 5 MB kapasitesidir. Bellek içi kapasitesini aştığında `Transmission`s yerel diskte depolanmış olan en fazla 50 MB. `Transmission`s ağ sorunları olduğunda yerel diskte depolanır. Yalnızca yerel disk depolanmış öğelerini, uygulama yeniden başlatıldığında gönderildiği bir uygulama kilitlenme önceliklidir.

## <a name="configurable-settings-in-channel"></a>Kanal yapılandırılabilir ayarları

Her kanal için yapılandırılabilen ayarların tam listesi aşağıda verilmiştir:

[InMemoryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Channel/InMemoryChannel.cs)

[ServerTelemetryChannel](https://github.com/microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs)

Ayarları için'en yaygın olarak kullanılan `ServerTelemetryChannel` aşağıda listelenmiştir:

1. `MaxTransmissionBufferCapacity` -En fazla bayt cinsinden bellek arabelleği iletimleri kanala tarafından kullanılan bellek miktarı. Bu kapasiteyi ulaşıldığında, yeni öğeleri doğrudan yerel disk olarak depolanır. 5 MB varsayılandır. Müşteri adayları, daha az disk kullanımı, ancak bu daha yüksek bir değer ayarlama, uygulama çökerse bellekte öğeleri kaybolacak dikkate almak önemlidir.

2. `MaxTransmissionSenderCapacity` -En fazla `Transmission`aynı anda Application Insights'a gönderilir s. Varsayılan değer 10'dur ancak daha yüksek bir sayı için yapılandırılabilir. Bu, büyük hacimli telemetri oluşturulduğunda, genellikle yük testi ve/veya örnekleme ne zaman devre dışı olduğunda yapılması önerilir.

3. `StorageFolder` -Öğeleri gerektiğinde diske depolamak için bir kanal tarafından kullanılan klasör. Hiçbir şey açıkça yapılandırılmışsa, Windows % LocalAppData % veya % Temp % kullanılır. Windows olmayan ortamlarda kullanıcı **gerekir** hangi telemetri olmaz depolanan yerel diske geçerli bir konum yapılandırın.

## <a name="which-channel-should-i-use"></a>Hangi kanal kullanmalıyım?

`ServerTelemetryChannel` uzun süre çalışan uygulamaların çoğu üretim senaryoları için önerilir. `Flush()` Yöntem uygulaması `ServerTelemetryChannel` zaman uyumlu değil ve `Flush()` bellek/diskten tüm bekleyen öğeleri gönderme garanti etmez. Bu kanal burada hakkında kapatmak için uygulamasıdır ve ardından bazı gecikme çağırdıktan sonra yapmak için önerilen senaryolar kullanılırsa `Flush()` bu kanalda. Ne kadar öğenin gibi etkenlere bağlı olarak gecikme gerekli miktarda tahmin değil veya `Transmissions` kaç kaç için desteklenen aktarılıyor disk, bellek, bulunan ve kanal ortasında üstel geri alma senaryolarını ise. Zaman uyumlu flush, daha sonra ihtiyaç yoksa `InMemoryChannel` önerilir.

## <a name="frequently-asked-questions"></a>Sıkça Sorulan Sorular

### <a name="does-applicationinsights-channel-offer-guaranteed-telemetry-delivery-or-what-are-the-scenarios-where-telemetry-can-be-lost"></a>*Applicationınsights kanal sunduğu telemetri garantili teslim veya burada telemetri kaybolabilir senaryolar nelerdir?*

* Yerleşik kanalları hiçbiri hakkında telemetri teslim arka uca işlem türü garantisi sağlar kısa cevaptır. Sırada `ServerTelemetryChannel` daha karşılaştırıldığında Gelişmiş `InMemoryChannel` güvenilir telemetri teslim için de telemetri göndermek için mümkün olan en iyi girişim kolaylaştırır ve telemetri hala kaybedilen çeşitli senaryolar. Bazı telemetri kayıp olduğu bir yaygın senaryolar şunlardır:

1. Uygulama kilitlenmeleri olduğunda bellek öğelerinde kaybolur.
1. Telemetri, ağ kesintileri veya Application Insights arka ucu ile sorunları sırasında yerel diske depolanır. Ancak, 24 saatten eski olan öğeleri atılır. Bu nedenle telemetri ağ sorunları uzun süre sırasında kaybolur.
1. Windows telemetri depolamak için varsayılan disk % LocalAppData % veya % Temp % konumlardır. Bu makinede genellikle yerel konumlardır. Bu konumda depolanan tüm telemetri, uygulamayı fiziksel bir konumdan diğerine geçerse, kaybolur.
1. Azure Web Apps (Windows), varsayılan disk konumu "D:\local\LocalAppData" şeklindedir. Bu konum kalıcı değildir ve uygulamayı yeniden başlatılır silindikten, ölçek ve benzeri bu konumlarda depolanan telemetri kaybı için önde gelen Dahası. Kullanıcılar kalıcı bir konuma "D:\home" gibi depolama kılabilirsiniz, ancak bu kalıcı konumlar altındaki Uzaktaki Depolama Birimi tarafından sunulan ve yavaş olabilir.

### <a name="does-servertelemetrychannel-work-in-non-windows-systems"></a>*ServerTelemetryChannel olmayan Windows sistemlerinde çalışır mı?*

* Windows Server'ı olan paket/ad alanı adına rağmen bu kanal şu özel durumla Windows olmayan sistemler desteklenir. Varsayılan olmayan-Windows, kanal yerel depolama klasörü yeniden oluşturmaz. Kullanıcılar, yerel depolama klasör oluşturun ve bunu kullanmak için bir kanal yapılandırmanız gerekir. Yerel depolama yapılandırıldıktan sonra kanal aynı Windows ve Windows olmayan sistemler çalışır.

### <a name="does-the-sdk-create-temporary-local-storage-is-the-data-encrypted-at-storage"></a>*SDK, yerel geçici depolama oluşturuyor mu? Veri depolama alanı şifrelenir?*

* SDK'sı telemetri öğelerinin azaltma sırasında veya ağ sorunları yerel depolama alanında depolar. Yerel olarak, bu veriler şifrelenmez.
Windows sistemleri için SDK'yı otomatik olarak TEMP veya APPDATA dizini içinde geçici bir yerel klasör oluşturur ve yöneticiler ve yalnızca geçerli kullanıcı için erişimi kısıtlar.
Non-Windows için yerel depo SDK'sı tarafından otomatik olarak oluşturulur ve bu nedenle hiçbir veri varsayılan olarak yerel olarak depolanır. Kullanıcılar, kendilerini bir depolama dizini oluşturma ve bunu kullanmak için kanal yapılandırın. Bu durumda, kullanıcı bu dizine güvenli sağlamaktan sorumludur.
Daha fazla bilgi edinin [veri koruma ve gizlilik](data-retention-privacy.md#does-the-sdk-create-temporary-local-storage).

## <a name="open-source-sdk"></a>Açık kaynak SDK'sı
Her Application Insights SDK'ları gibi kanalları da açık kaynaktır. Okuma ve koda katkıda bulunan veya rapor sorunu [resmi GitHub deposunu](https://github.com/Microsoft/ApplicationInsights-dotnet).

## <a name="next-steps"></a>Sonraki Adımlar

* [Örnekleme](../../azure-monitor/app/sampling.md)
* [SDK sorunlarını giderme](../../azure-monitor/app/asp-net-troubleshoot-no-data.md)
