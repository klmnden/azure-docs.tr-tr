---
title: "Linux - Azure Java web uygulaması performans izleme | Microsoft Docs"
description: "Uygulama izleme için Application Insights performans eklenti CollectD ile Java Web sitenizin genişletilmiş."
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 40c68f45-197a-4624-bf89-541eb7323002
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: cde0fc020f1774e0e7669e7573e4aaff3534b34c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>collectd: Application Insights Linux performans ölçümlerini


Linux sistem performans ölçümlerini keşfetmek için [Application Insights](app-insights-overview.md), yükleme [collectd](http://collectd.org/), eklenti, Application Insights ile birlikte. Bu açık kaynak çözümü çeşitli sistem ve ağ istatistiklerini toplar.

Genellikle, zaten varsa collectd kullanacağınız [Application Insights ile Java web hizmetiniz izlenmiş][java]. Bu, uygulamanızın performansını artırmak veya sorunları tanılamak için yardımcı olması için daha fazla veri verir. 

![Örnek grafikler](./media/app-insights-java-collectd/sample.png)

## <a name="get-your-instrumentation-key"></a>İzleme anahtarı edinme
İçinde [Microsoft Azure portal](https://portal.azure.com), açık [Application Insights](app-insights-overview.md) verinin görünmesini istediğiniz yere kaynak. (Veya [yeni bir kaynak oluşturmak](app-insights-create-new-resource.md).)

Kaynağı tanımlayan izleme anahtarını bir kopyasını alın.

![Tümüne Gözat, kaynak açın ve ardından Essentials açılan listeyi seçin ve izleme anahtarını kopyalama](./media/app-insights-java-collectd/02-props.png)

## <a name="install-collectd-and-the-plug-in"></a>Collectd ve eklenti yükleme
Linux sunucusu makineleri üzerinde:

1. Yükleme [collectd](http://collectd.org/) sürüm 5.4.0 veya sonraki bir sürümü.
2. Karşıdan [Application Insights collectd yazıcı eklentisi](https://aka.ms/aijavasdk). Sürüm numarasını not edin.
3. Eklenti JAR içine kopyalama `/usr/share/collectd/java`.
4. Düzen `/etc/collectd/collectd.conf`:
   * Emin [Java eklentisi](https://collectd.org/wiki/index.php/Plugin:Java) etkinleştirilir.
   * Aşağıdaki JAR içerecek şekilde JVMArg java.class.path için güncelleştirin. Sürüm numarası indirdiğiniz olanla eşleşecek şekilde güncelleştirin:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Kaynağınız araçları anahtarından kullanarak bu parçacığını ekleyin:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Örnek bir yapılandırma dosyası parçası şöyledir:

```XML

    ...
    # collectd plugins
    LoadPlugin cpu
    LoadPlugin disk
    LoadPlugin load
    ...

    # Enable Java Plugin
    LoadPlugin "java"

    # Configure Java Plugin
    <Plugin "java">
      JVMArg "-verbose:jni"
      JVMArg "-Djava.class.path=/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar:/usr/share/collectd/java/collectd-api.jar"

      # Enabling Application Insights plugin
      LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"

      # Configuring Application Insights plugin
      <Plugin ApplicationInsightsWriter>
        InstrumentationKey "12345678-1234-1234-1234-123456781234"
      </Plugin>

      # Other plugin configurations ...
      ...
    </Plugin>
    ...
```

Diğer yapılandırma [collectd eklentileri](https://collectd.org/wiki/index.php/Table_of_Plugins), hangi çeşitli verilerini toplayabilir farklı kaynaklardan.

Collectd göre yeniden kendi [el ile](https://collectd.org/wiki/index.php/First_steps).

## <a name="view-the-data-in-application-insights"></a>Application Insights'ta verileri görüntüleme
Application Insights kaynağınıza açmak [ölçüm Gezgini ve grafikler ekleyin][metrics], özel kategoriden görmek istediğiniz ölçümleri seçme.

![](./media/app-insights-java-collectd/result.png)

Varsayılan olarak, ölçümleri ölçümleri toplanmış olan tüm ana bilgisayar makine genelinde toplanır. Ölçümleri ana bilgisayar başına grafik ayrıntıları dikey penceresinde görüntülemek için gruplandırma'yı açın ve ardından tarafından CollectD konak grubu seçin.

## <a name="to-exclude-upload-of-specific-statistics"></a>Karşıya yükleme belirli istatistik dışlamak için
Varsayılan olarak, Application Insights eklentisiyle 'eklentileri okuma' tüm etkin collectd tarafından toplanan tüm verileri gönderir. 

Belirli eklentileri ya da veri kaynağından veri dışlamak için:

* Yapılandırma dosyasını düzenleyin. 
* İçinde `<Plugin ApplicationInsightsWriter>`, böyle yönerge satırları ekleyin:

| Yönergesi | Etki |
| --- | --- |
| `Exclude disk` |Tarafından toplanan tüm veriler hariç `disk` eklentisi |
| `Exclude disk:read,write` |Adlı kaynaklarını Dışla `read` ve `write` gelen `disk` eklentisi. |

Bir satır başı karakteri ile ayrı yönergeleri.

## <a name="problems"></a>Sorunlarınız mı var?
*Veri portalında görmüyorum*

* Açık [arama] [ diagnostic] ham olayları ulaşmış olan görmek için. Bazı durumlarda bunlar ölçümleri Explorer'da görünen daha uzun sürer.
* İçin gerekebilecek [ayarlamak giden veriler için güvenlik duvarı özel durumlar](app-insights-ip-addresses.md)
* Application Insights eklentisiyle izlemeyi etkinleştirin. İçinde bu satırı ekleyin `<Plugin ApplicationInsightsWriter>`:
  * `SDKLogger true`
* Bir Terminali açın ve raporlama herhangi bir sorunla görmek için ayrıntılı modda collectd başlatın:
  * `sudo collectd -f`

## <a name="known-issue"></a>Bilinen bir sorun

Uygulama Öngörüler yazma eklentisi belirli okuma eklentileri ile uyumlu değil. Bazı eklentiler bazen "NaN burada Application Insights eklentisiyle bir kayan noktalı sayı bekliyor" gönderin.

Belirti: Collectd günlük "AI:... dahil hataları gösterir. SyntaxError: beklenmeyen bir belirteç N ".

Geçici çözüm: sorunu yazma eklentileri tarafından toplanan verileri dışlayın. 

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md


