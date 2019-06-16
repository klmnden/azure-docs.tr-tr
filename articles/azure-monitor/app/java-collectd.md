---
title: Linux - Azure üzerinde Java web uygulaması performansını izleme | Microsoft Docs
description: Uygulama izleme için Application Insights performans toplanan eklentisi ile Java Web sitenizin, genişletilmiş.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 40c68f45-197a-4624-bf89-541eb7323002
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: mbullwin
ms.openlocfilehash: c6e947dfed3169f346f43ab08225056815e8b487
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061202"
---
# <a name="collectd-linux-performance-metrics-in-application-insights"></a>toplanan: Application ınsights Linux performans ölçümleri


İçindeki Linux sistem performans ölçümlerini keşfetmek için [Application Insights](../../azure-monitor/app/app-insights-overview.md), yükleme [toplanan](https://collectd.org/)eklentisi, Application Insights ile birlikte. Bu açık kaynaklı çözüm çeşitli sistem ve ağ istatistiklerini toplar.

Genellikle zaten varsa, toplanan kullanacağınız [Application Insights ile Java web hizmetinizin izleme eklenmiş][java]. Uygulamanızın performansını geliştirmek veya sorunları tanılamak için yardımcı olacak daha fazla veri sağlar. 

## <a name="get-your-instrumentation-key"></a>İzleme anahtarınızı alın
İçinde [Microsoft Azure Portal'da](https://portal.azure.com)açın [Application Insights](../../azure-monitor/app/app-insights-overview.md) verilerin görünmesini istediğiniz kaynak. (Veya [yeni kaynak Oluştur](../../azure-monitor/app/create-new-resource.md ).)

Kaynağı tanımlayan izleme anahtarını bir kopyasını alın.

![Tümüne Gözat, kaynağınızı açın ve ardından temel bileşenler açılan listeyi seçin ve izleme anahtarını kopyalama](./media/java-collectd/instrumentation-key-001.png)

## <a name="install-collectd-and-the-plug-in"></a>Toplanan ve eklenti yükleme
Linux sunucusu makinelerinizde:

1. Yükleme [toplanan](https://collectd.org/) sürüm 5.4.0 ya da daha sonra.
2. İndirme [Application Insights toplanan yazan eklentisi](https://aka.ms/aijavasdk). Sürüm numarasını not edin.
3. Eklenti JAR içine kopyalama `/usr/share/collectd/java`.
4. Düzen `/etc/collectd/collectd.conf`:
   * Emin [Java eklentisi](https://collectd.org/wiki/index.php/Plugin:Java) etkinleştirilir.
   * Aşağıdaki JAR içerecek şekilde JVMArg java.class.path için güncelleştirin. Sürüm numarasını indirdiğiniz olanla eşleşecek şekilde güncelleştirin:
   * `/usr/share/collectd/java/applicationinsights-collectd-1.0.5.jar`
   * Kaynağınızın izleme anahtarını kullanarak, bu kod parçacığını ekleyin:

```XML

     LoadPlugin "com.microsoft.applicationinsights.collectd.ApplicationInsightsWriter"
     <Plugin ApplicationInsightsWriter>
        InstrumentationKey "Your key"
     </Plugin>
```

Örnek bir yapılandırma dosyası bir bölümü aşağıda verilmiştir:

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

Diğer yapılandırma [toplanan eklentileri](https://collectd.org/wiki/index.php/Table_of_Plugins), hangi çeşitli verilerini toplayabilir farklı kaynaklardan gelen.

Toplanan göre yeniden kendi [el ile](https://collectd.org/wiki/index.php/First_steps).

## <a name="view-the-data-in-application-insights"></a>Application Insights'ta verileri görüntüleyin
Application Insights kaynağınızı açın [ölçüm ve grafikler ekleyin][metrics], özel kategori görmek istediğiniz ölçümleri seçme.

Varsayılan olarak, kendisinden ölçümleri toplanmış olan tüm konak makinelerde ölçümleri toplanır. Grafik ayrıntıları dikey penceresinde, konak başına ölçümleri görüntülemek için gruplandırma'yı açın ve tarafından toplanan konak grubu seçin.

## <a name="to-exclude-upload-of-specific-statistics"></a>Karşıya yükleme belirli İstatistikler dışlamak için
Varsayılan olarak, Application Insights eklentisi 'eklentileri okuma' tüm etkin toplanan tarafından toplanan tüm verileri gönderir. 

Özel eklentiler ya da veri kaynağından verileri dışlamak için:

* Yapılandırma dosyasını düzenleyin. 
* İçinde `<Plugin ApplicationInsightsWriter>`, böyle yönerge satırları ekleyin:

| Yönergesi | Etki |
| --- | --- |
| `Exclude disk` |Tarafından toplanan tüm verileri dışlamak `disk` eklentisi |
| `Exclude disk:read,write` |Adlı kaynaklarını hariç tutma `read` ve `write` gelen `disk` eklentisi. |

Yeni bir satır ile ayrı yönergeleri.

## <a name="problems"></a>Sorunlarınız mı var?
*Verileri portalında görmüyorum*

* Açık [arama] [ diagnostic] ham olaylar gelmiş görmek için. Bazen, ölçüm Gezgini'nde görünür daha uzun sürer.
* Gerekebilir [giden veriler için güvenlik duvarı özel durumlarını ayarlama](../../azure-monitor/app/ip-addresses.md)
* Application Insights eklentisi içinde izlemeyi etkinleştirin. İçinde bu satırı ekleyin `<Plugin ApplicationInsightsWriter>`:
  * `SDKLogger true`
* Bir terminal açın ve raporlama tüm sorunları görmek için ayrıntılı modda toplanan başlayın:
  * `sudo collectd -f`

## <a name="known-issue"></a>Bilinen sorun

Application Insights yazma eklentisi, belirli okuma eklenti ile uyumlu değil. Bazı eklentiler, bazen "NaN burada bir kayan noktalı sayı olan Application Insights eklentisi bekliyor" gönderin.

Belirti: Toplanan günlük içeren "AI:... hataları gösterir. SyntaxError: Beklenmeyen belirteç N".

Geçici çözüm: Sorun yazma eklentileri tarafından toplanan veriler hariç tutun. 

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md


