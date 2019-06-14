---
title: Azure tanılama verilerini Application Insights'a gönderme yapılandırın
description: Application Insights'a veri göndermek için Azure tanılama genel yapılandırmasını güncelleştirin.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2016
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: f7e21b805c64522005dce3e7d04aa158e1c21032
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60396147"
---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-to-application-insights"></a>Bulut hizmeti, sanal makine ya da Service Fabric tanılama verilerini Application Insights'a gönderme
Bulut Hizmetleri, sanal makineler, sanal makine ölçek kümeleri ve Service Fabric tüm verileri toplamak için Azure tanılama uzantısını kullanın.  Azure Tanılama verileri Azure Storage tablolarının gönderir.  Ancak, ayrıca tüm kanal veya bir Azure tanılama uzantısı 1.5 veya üzeri kullanarak diğer konumlara veri alt kümesini kullanabilirsiniz.

Bu makalede, Application Insights'a Azure tanılama uzantısını veri gönderme açıklar.

## <a name="diagnostics-configuration-explained"></a>Açıklanan tanılama yapılandırması
Burada, Tanılama verileri gönderebilirsiniz ilave konum olduğu sunulan Azure tanılama uzantısı 1.5 başlatır.

Application Insights için bir havuz yapılandırması örneği:

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

- **Applicationınsights** öğesi burada Azure Tanılama verileri gönderilir. Application ınsights kaynağının izleme anahtarını belirtir.
    - Mevcut bir Application Insights kaynağına sahip değilseniz, bkz. [yeni bir Application Insights kaynağı oluşturun](../../azure-monitor/app/create-new-resource.md ) kaynak oluşturma ve izleme anahtarını alma hakkında daha fazla bilgi.
    - Bir bulut hizmeti Azure SDK 2.8 ve daha sonra geliştiriyorsanız bu izleme anahtarını otomatik olarak doldurulur. Dayanarak değeri **appınsıghts_ınstrumentatıonkey** bulut hizmeti proje paketlenirken hizmet yapılandırma ayarı. Bkz: [Cloud Services ile Application Insights kullanın](../../azure-monitor/app/cloudservices.md).

- **Kanalları** öğesi içeren bir veya daha fazla **kanal** öğeleri.
    - *Adı* özniteliği benzersiz olarak bu kanalda ifade eder.
    - *Loglevel* özniteliği kanalı sağlayan günlük düzeyini belirtmenize olanak sağlar. En az bilgi sırasına göre kullanılabilir günlük düzeyleri şunlardır:
        - Ayrıntılı
        - Bilgi
        - Uyarı
        - Hata
        - Kritik

Bir kanal gibi bir filtre işlevi görür ve hedef havuz için göndermek için özel günlük düzeyleri seçmenizi sağlar. Örneğin, ayrıntılı günlük toplama ve depolama alanına göndermeden ancak havuz yalnızca hataları gönderme.

Aşağıdaki grafikte, bu ilişkiyi gösterir.

![Genel tanılama yapılandırması](media/diagnostics-extension-to-application-insights/AzDiag_Channels_App_Insights.png)

Aşağıdaki grafikte, yapılandırma değerlerini ve nasıl çalıştıkları özetler. Hiyerarşideki farklı düzeylerde yapılandırmasında birden fazla havuz ekleyebilirsiniz. Havuz en üst düzeyde bir genel ayarı olarak davranır ve tek tek öğe belirtilen bu genel ayarı için bir geçersiz kılma gibi davranır.

![Tanılama, Application Insights ile yapılandırma iç havuzları](media/diagnostics-extension-to-application-insights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a>Tam havuzunu yapılandırma örneği
İşte genel yapılandırmasının tam bir örnek dosya
1. tüm hataları Application Insights'a gönderir (belirtilen **DiagnosticMonitorConfiguration** düğüm)
2. Ayrıca uygulama günlüklerini düzeyinde ayrıntılı günlükleri gönderir (belirtilen **günlükleri** düğümü).

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

### <a name="send-only-error-logs-to-the-application-insights-sink"></a>Application Insights havuz için hata günlüklerini Gönder

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-to-application-insights"></a>Ayrıntılı uygulama günlükleri Application Insights'a gönderme

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

- **Kanal türü ve değil performans sayaçları yalnızca oturum açın.** Bir performans sayacı öğesi ile bir kanalı belirtirseniz, göz ardı edilir.
- **Günlük düzeyi için bir kanal tarafından Azure tanılama toplanmakta olan günlük düzeyini aşamaz.** Örneğin, olamaz toplama günlükleri öğesinde bulunan uygulama günlüğüne hataları ve ayrıntılı göndermeye Application Insight havuz için günlükleri. *ScheduledTransferLogLevelFilter* özniteliği gerekir her zaman topla eşit veya havuza göndermeye çalıştığınız günlük daha fazla günlükleri.
- **Blob verilerini Application Insights'a Azure tanılama uzantısı tarafından toplanan gönderemez.** Örneğin, altında belirtilen hiçbir şey *dizinleri* düğümü. Kilitlenme dökümleri için gerçek kilitlenme BLOB depolamaya gönderilir ve Application ınsights'ı yalnızca kilitlenme oluşturulmuş bir bildirim gönderilir.

## <a name="next-steps"></a>Sonraki Adımlar
* Bilgi nasıl [Azure tanılama bilgilerinizi görüntülemek](https://docs.microsoft.com/azure/application-insights/app-insights-cloudservices) Application ınsights.
* Kullanım [PowerShell](../../cloud-services/cloud-services-diagnostics-powershell.md) uygulamanız için Azure tanılama uzantısını etkinleştirmek için.
* Kullanım [Visual Studio](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) uygulamanız için Azure tanılama uzantısını etkinleştirme

