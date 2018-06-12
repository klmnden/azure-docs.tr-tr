---
title: Azure tanılama verilerini Application Insights'a göndermek için yapılandırma
description: Application Insights'a veri göndermek için Azure tanılama ortak yapılandırmasını güncelleştirin.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2016
ms.author: robb
ms.component: diagnostic-extension
ms.openlocfilehash: 3e1f4076c7a90cbb348f31b7b92e745fff79a04f
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35262146"
---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-to-application-insights"></a>Bulut hizmeti, sanal makine ya da Service Fabric tanılama verilerini Application Insights'a gönderme
Bulut Hizmetleri, sanal makineler, sanal makine ölçek kümeleri ve Service Fabric tüm Azure tanılama uzantısını verileri toplamak için kullanın.  Azure Tanılama verileri Azure Storage tablolara gönderir.  Ancak, aynı zamanda tüm kanal veya bir Azure tanılama uzantısını 1.5 veya daha sonraki kullanarak başka konumlara veri alt kümesini kullanabilirsiniz.

Bu makalede, Application Insights'a Azure tanılama uzantısını verileri göndermeyi açıklar.

## <a name="diagnostics-configuration-explained"></a>Açıklanan tanılama yapılandırması
Tanılama verileri nerede Gönder ek konumları olduğu sunulan Azure tanılama uzantısını 1.5 havuzlarını.

Bir havuz örnek yapılandırma Application Insights için:

```XML
<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "ApplicationInsights",
            "ApplicationInsights": "{Insert InstrumentationKey}",
            "Channels": {
                "Channel": [
                    {
                        "logLevel": "Error",
                        "name": "MyTopDiagData"
                    },
                    {
                        "logLevel": "Error",
                        "name": "MyLogData"
                    }
                ]
            }
        }
    ]
}
```
- **Havuz** *adı* havuzu benzersiz olarak tanımlayan bir dize değeri bir özniteliktir.

- **Applicationınsights** öğesi Azure Tanılama verileri nerede gönderilir uygulama Öngörüler kaynağın izleme anahtarını belirtir.
    - Varolan bir Application Insights kaynağı sahip değilseniz, bkz: [yeni bir Application Insights kaynağı oluşturma](../application-insights/app-insights-create-new-resource.md) kaynak oluşturma ve izleme anahtarı alma hakkında daha fazla bilgi.
    - Bir bulut hizmeti Azure SDK 2.8 ve daha sonra geliştiriyorsanız, bu izleme anahtarını otomatik olarak doldurulur. Dayanarak değeri **appınsıghts_ınstrumentatıonkey** Cloud Service projesi paketleme, hizmet yapılandırma ayarı. Bkz: [Application Insights ile birlikte bulut hizmetlerini kullanan](../application-insights/app-insights-cloudservices.md).

- **Kanalları** öğesi içeren bir veya daha fazla **kanal** öğeleri.
    - *Adı* özniteliği benzersiz olarak bu kanala başvuruyor.
    - *Loglevel* özniteliği kanal günlük düzeyini belirtmenize olanak sağlar. En az bilgilere sırada kullanılabilir günlük düzeyleri şunlardır:
        - Ayrıntılı
        - Bilgi
        - Uyarı
        - Hata
        - Kritik

Bir kanal bir filtre gibi davranır ve hedef havuz göndermek için belirli günlük düzeyleri seçmenize olanak sağlar. Örneğin, ayrıntılı günlükleri toplamak ve depolama alanına göndermeden ancak havuz yalnızca hataları gönderme.

Aşağıdaki grafikte, bu ilişkiyi gösterir.

![Tanılama genel yapılandırması](./media/azure-diagnostics-configure-applicationinsights/AzDiag_Channels_App_Insights.png)

Aşağıdaki grafikte yapılandırma değerlerini ve nasıl çalıştığını özetler. Hiyerarşideki farklı düzeylerde yapılandırmasında birden çok havuzlarını içerebilir. En üst düzeyinde havuz genel ayar olarak davranır ve tek tek öğede belirtilen bir genel Bu ayar için bir geçersiz kılma gibi davranır.

![Tanılama yapılandırması Application Insights ile iç havuzlar](./media/azure-diagnostics-configure-applicationinsights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a>Tam havuz yapılandırma örneği
İşte genel yapılandırmasının tam bir örnek dosya
1. tüm hataları Application Insights'a gönderir (belirtilen **DiagnosticMonitorConfiguration** düğüm)
2. Ayrıca ayrıntılı düzeyi günlükleri için uygulama günlüklerine gönderir (belirtilen **günlükleri** düğüm).

```XML
<WadCfg>
  <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData"> <!-- All info below sent to this channel -->
    <DiagnosticInfrastructureLogs />
    <PerformanceCounters>
      <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
      <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
    </PerformanceCounters>
    <WindowsEventLog scheduledTransferPeriod="PT1M">
      <DataSource name="Application!*" />
    </WindowsEventLog>
    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose"
            sinks="ApplicationInsights.MyLogData"/> <!-- This specific info sent to this channel -->
  </DiagnosticMonitorConfiguration>

<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
  </SinksConfig>
</WadCfg>
```
```JSON
"WadCfg": {
    "DiagnosticMonitorConfiguration": {
        "overallQuotaInMB": 4096,
        "sinks": "ApplicationInsights.MyTopDiagData", "_comment": "All info below sent to this channel",
        "DiagnosticInfrastructureLogs": {
        },
        "PerformanceCounters": {
            "PerformanceCounterConfiguration": [
                {
                    "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                    "sampleRate": "PT3M"
                },
                {
                    "counterSpecifier": "\\Memory\\Available MBytes",
                    "sampleRate": "PT3M"
                }
            ]
        },
        "WindowsEventLog": {
            "scheduledTransferPeriod": "PT1M",
            "DataSource": [
                {
                    "name": "Application!*"
                }
            ]
        },
        "Logs": {
            "scheduledTransferPeriod": "PT1M",
            "scheduledTransferLogLevelFilter": "Verbose",
            "sinks": "ApplicationInsights.MyLogData", "_comment": "This specific info sent to this channel"
        }
    },
    "SinksConfig": {
        "Sink": [
            {
                "name": "ApplicationInsights",
                "ApplicationInsights": "{Insert InstrumentationKey}",
                "Channels": {
                    "Channel": [
                        {
                            "logLevel": "Error",
                            "name": "MyTopDiagData"
                        },
                        {
                            "logLevel": "Verbose",
                            "name": "MyLogData"
                        }
                    ]
                }
            }
        ]
    }
}
```
Önceki yapılandırmada, aşağıdaki satırları şu anlama gelir:

### <a name="send-all-the-data-that-is-being-collected-by-azure-diagnostics"></a>Azure tanılama tarafından toplanan tüm verileri gönder

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights",
}
```

### <a name="send-only-error-logs-to-the-application-insights-sink"></a>Application Insights havuz yalnızca hata günlüklerini Gönder

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-to-application-insights"></a>Ayrıntılı uygulama günlüklerini Application Insights'a gönderme

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyLogData",
}
```

## <a name="limitations"></a>Sınırlamalar

- **Kanallar yalnızca türü ve değil performans sayaçlarını günlüğe kaydeder.** Bir performans sayacı öğesi ile bir kanal belirtirseniz, yoksayılır.
- **Günlük düzeyi için bir kanal tarafından Azure tanılama toplanmakta için günlük düzeyi aşamaz.** Örneğin, olamaz toplama günlükleri öğesindeki uygulama günlüğü hataları ve ayrıntılı göndermeye uygulama Insight havuz günlükleri. *ScheduledTransferLogLevelFilter* özniteliği gerekir her zaman toplamak eşit veya günlükleri'den daha fazla günlükleri göndermek için bir havuz çalışıyorsunuz.
- **Blob verilerini Application Insights'a Azure tanılama uzantısını tarafından toplanan gönderemez.** Örneğin, altında belirtilen hiçbir şey *dizinleri* düğümü. Kilitlenme dökümleri için gerçek kilitlenme döküm BLOB depolamaya gönderilir ve Application Insights'a yalnızca kilitlenme döküm oluşturulan bir bildirim gönderilir.

## <a name="next-steps"></a>Sonraki Adımlar
* Bilgi edinmek için nasıl [Azure tanılama bilgilerinizi görüntülemek](https://docs.microsoft.com/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) Application ınsights'ta.
* Kullanım [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) uygulamanız için Azure tanılama uzantısını etkinleştirmek için.
* Kullanım [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) uygulamanız için Azure tanılama uzantısını etkinleştirmek için
