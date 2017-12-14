---
title: "Olay hub'ları kullanarak etkin yolunuzda Azure Tanılama verileri akış | Microsoft Docs"
description: "Olay hub'ları yaygın senaryoları için yönergeler de dahil olmak üzere uca, ile Azure tanılama yapılandırılıyor."
services: event-hubs
documentationcenter: na
author: rboucher
manager: carmonm
editor: 
ms.assetid: edeebaac-1c47-4b43-9687-f28e7e1e446a
ms.service: monitoring-and-diagnostics
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: robb
ms.openlocfilehash: ca0dd96389a605ed8bf34af81eb4d75bef581338
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Olay hub'ları kullanarak Azure Tanılama verileri etkin yolunuzda akış
Azure tanılama bulut Hizmetleri sanal makinelerden (VM'ler) ölçümleri ve günlükleri toplamak ve sonuçları Azure depolama birimine aktarmak için esnek yöntemler sağlar. Mart 2016 (SDK 2.9) zaman çerçevesinde başlayarak, özel veri kaynaklarına tanılama gönderebilir ve etkin yolunuzda veri aktarımının saniye cinsinden kullanarak [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).

Desteklenen veri türleri şunlardır:

* Windows için Olay İzleme (ETW) olayları
* Performans sayaçları
* Windows olay günlükleri
* Uygulama günlükleri
* Azure tanılama altyapı günlükleri

Bu makalede Azure tanılama uçtan uca olay hub'larıyla yapılandırma gösterilmektedir. Yönergeler ayrıca aşağıdaki ortak senaryolar için sağlanır:

* Günlükleri ve Event Hubs'a gönderilen ölçümleri nasıl özelleştirileceği
* Her ortamda yapılandırmalarını değiştirme
* Olay hub'ları akış verileri görüntüleme
* Bağlantı ile ilgili sorunları giderme  

## <a name="prerequisites"></a>Ön koşullar
Olay hub'ları receieving Azure Tanılama verileri bulut Hizmetleri, sanal makineleri, sanal makine ölçek kümeleri ve Service Fabric Azure SDK 2.9 ve karşılık gelen Azure Araçları Visual Studio için başlangıç desteklenir.

* Azure tanılama uzantısını 1.6 ([veya daha sonra .NET 2.9 için Azure SDK](https://azure.microsoft.com/downloads/) bu varsayılan olarak hedefler)
* [Visual Studio 2013 veya üzeri](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* Var olan Azure tanılama yapılandırmalarında kullanarak bir uygulama bir *.wadcfgx* dosya ve aşağıdaki yöntemlerden birini:
  * Visual Studio: [tanılama Azure bulut Hizmetleri ve sanal makineler için yapılandırma](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
  * Windows PowerShell: [PowerShell kullanarak Azure Cloud Services tanılamayı etkinleştirme](../cloud-services/cloud-services-diagnostics-powershell.md)
* Olay hub'ları ad alanı makale sağlanan [Event Hubs ile çalışmaya başlama](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Azure Tanılama Olay hub'ları havuz Bağlan
Varsayılan olarak, Azure tanılama her zaman günlüklerini ve ölçümleri bir Azure depolama hesabı gönderir. Bir uygulama da verileri Event Hubs'a yeni ekleyerek gönderebilir **iç havuzlar** altında bölümünde **PublicConfig** / **WadCfg** öğesinin *. wadcfgx* dosya. Visual Studio'da *.wadcfgx* dosya şu yolda depolanır: **bulut hizmeti projesini** > **rolleri** > **() RoleName)** > **diagnostics.wadcfgx** dosya.

```xml
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "HotPath",
            "EventHub": {
                "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
                "SharedAccessKeyName": "SendRule"
            }
        }
    ]
}
```

Bu örnekte olay hub'ın tam ad alanına olay hub'ı URL'si ayarlanır: olay hub'ları ad alanı + "/" + olay hub'ı adı.  

Olay hub'ı URL görüntülenir [Azure portal](http://go.microsoft.com/fwlink/?LinkID=213885) olay hub'ları Panoda.  

**Havuzu** adı aynı değeri tutarlı bir şekilde yapılandırma dosyası kullanılan sürece için geçerli bir dize ayarlanabilir.

> [!NOTE]
> Olabilir ek havuzlarını aşağıdaki gibi *Applicationınsights* Bu bölümde yapılandırılmış. Azure tanılama sağlayan her havuz olarak da bildirilirse tanımlanması bir veya daha fazla havuzlarını **PrivateConfig** bölümü.  
>
>

Olay hub'ları havuz gerekir de bildirilen ve içinde tanımlanan **PrivateConfig** bölümünü *.wadcfgx* yapılandırma dosyası.

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="{account name}" key="{account key}" endpoint="{optional storage endpoint}" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
</PrivateConfig>
```
```JSON
{
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{optional storage endpoint}",
    "EventHub": {
        "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    }
}
```

`SharedAccessKeyName` Değeri bir paylaşılan erişim imzası (SAS) anahtarı ve içinde tanımlanan ilke eşleşmelidir **olay hub'ları** ad alanı. Olay hub'ları panoya göz atın [Azure portal](https://portal.azure.com), tıklatın **Yapılandır** sekmesini tıklatın ve sahip bir adlandırılmış ilkeyi kurmak (örneğin, "SendRule") *Gönder* izinleri. **StorageAccount** içinde bildirilmiş **PrivateConfig**. Bunlar çalışıyorsanız burada değerlerini değiştirmek için gerek yoktur. Bu örnekte, biz değerleri boş bir aşağı akış varlık değerleri ayarlayacaksınız oturum olduğu bırakın. Örneğin, *ServiceConfiguration.Cloud.cscfg* ortam yapılandırma dosyası ayarlar ortam uygun adları ve anahtarları.  

> [!WARNING]
> Olay hub'ları SAS anahtarı düz metin halinde depolanır *.wadcfgx* dosya. Genellikle, bu anahtarı kaynak kod denetimine iade veya uygun şekilde korumanız gerekir böylece yapı sunucunuz bir varlığı olarak kullanılabilir. Bir SAS anahtarıyla burada kullanmanızı öneririz *yalnızca gönderme* izinleri böylece kötü niyetli bir kullanıcı event hub'ına yazma ancak kendisine dinleme veya yönetme.
>
>

## <a name="configure-azure-diagnostics-to-send-logs-and-metrics-to-event-hubs"></a>Tanılama günlüklerini ve ölçümleri Event Hubs'a göndermek için Azure Yapılandır
Tüm varsayılan ve özel tanılama veri daha önce açıklandığı gibi diğer bir deyişle, ölçümleri ve günlükleri, otomatik olarak gönderilir Azure Storage yapılandırılmış işlevdeki. Olay hub'ları ve ek bir havuz, olay hub'ına gönderildiği hiyerarşideki herhangi bir kök veya yaprak düğümü belirtebilirsiniz. Bu, ETW olayları, performans sayaçları, Windows olay günlüklerini ve uygulama günlükleri içerir.   

Kaç tane veri noktaları gerçekte Event Hubs'a aktarılması gerektiğini dikkate almak önemlidir. Genellikle, geliştiriciler tüketilen ve hızlı bir şekilde yorumlanır düşük gecikme süreli hot yolu veri aktarın. Uyarıları veya otomatik ölçeklendirme kurallarını izleyen örnekler sistemleridir. Bir geliştirici ayrıca bir alternatif analiz deposu yapılandırma veya deposu--Azure akış analizi, Elasticsearch, özel bir izleme sistemi veya diğer sık kullanılan bir izleme sistemi arama.

Bazı örnek yapılandırmalar şunlardır.

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "HotPath",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        }
    ]
}
```

Yukarıdaki örnekte, havuz üst öğeye uygulanan **PerformanceCounters** düğümü hiyerarşideki tüm alt anlamına gelir **performans sayaçları** Event Hubs'a gönderilir.  

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Rejected",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Queued",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        }
    ]
}
```

Önceki örnekte, yalnızca üç sayaç havuz uygulanır: **sıraya alınan istek sayısı**, **reddedilen istekleri**, ve **% işlemci zamanı**.  

Aşağıdaki örnek, bir geliştirici bu hizmetin sistem durumu için kullanılan önemli ölçümleri olması için gönderilen veri miktarını nasıl sınırlayabilirsiniz gösterir.  

```XML
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```
```JSON
"Logs": {
    "scheduledTransferPeriod": "PT1M",
    "scheduledTransferLogLevelFilter": "Error",
    "sinks": "HotPath"
}
```

Bu örnekte, havuz için günlükleri uygulanır ve yalnızca hata düzeyi izleme filtrelenir.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Dağıtma ve bulut Hizmetleri uygulama ve tanılama config güncelleştir
Visual Studio uygulama ve Event Hubs havuz yapılandırma dağıtmak için en kolay yolu sağlar. Görüntülemek ve dosyayı düzenlemek için açın *.wadcfgx* dosya Visual Studio'da, düzenlemek ve dosyayı kaydedin. Yolun **bulut hizmeti projesini** > **rolleri** > **(RoleName)** > **diagnostics.wadcfgx**.  

Bu noktada, tüm dağıtım ve dağıtım eylemlerini Visual Studio, Visual Studio Team System ve tüm komutları veya MSBuild ve kullanım göre betikleri güncelleştirme **/t: yayımlama** hedef dahil *.wadcfgx* paketleme işleminde. Ayrıca, dağıtımları ve güncelleştirmeleri dosyayı Azure Vm'leriniz uygun Azure Tanılama Aracı uzantısını kullanarak dağıtın.

Uygulama ve Azure tanılama yapılandırması dağıttıktan sonra hemen panosunda olay hub'ın etkinliğini görür. Bu hot yol verileri dinleyicisi istemci ya da analiz aracı tercih ettiğiniz görüntülemeye devam etmeye hazır olduğunu gösterir.  

Aşağıdaki resimde sağlıklı olay hub'ına süre 23'sonra Başlangıç tanılama verilerini göndermeye olay hub'ları Panosu gösterir. Ne zaman olan uygulama dağıtıldıktan güncelleştirilmiş ile *.wadcfgx* dosya ve havuz düzgün yapılandırılmış.

![][0]  

> [!NOTE]
> Azure tanılama yapılandırma dosyasına (.wadcfgx) güncelleştirmeleri yaptığınızda, güncelleştirmeleri yapılandırmanın yanı sıra tüm uygulama için Visual Studio yayımlama veya bir Windows PowerShell komut dosyası kullanarak anında iletme olduğunu önerilir.  
>
>

## <a name="view-hot-path-data"></a>Hot yol verileri görüntüleme
Daha önce anlatıldığı gibi dinleme ve olay hub'ları veri işleme için birçok kullanım örnekleri vardır.

Basit bir yaklaşım, olay hub'ına dinle ve çıkış akışı yazdırmak için küçük test konsol uygulaması oluşturmaktır. Daha ayrıntılı olarak anlatılmıştır aşağıdaki kodu yerleştirebilirsiniz [Event Hubs ile çalışmaya başlama](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), bir konsol uygulamasında.  

Konsol uygulaması içermelidir Not [olay işlemcisi konağı NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Açılı ayraç değerleri değiştirin unutmayın **ana** kaynaklarınız için değerlerle işlevi.   

```csharp
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key to stop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sinks"></a>Olay hub'ları havuzlarını sorun giderme
* Olay hub'ı beklendiği gibi gelen veya giden olay etkinlik göstermez.

    Olay hub'ınızı başarıyla sağlandığından emin olun. Tüm bağlantı bilgilerini **PrivateConfig** bölümünü *.wadcfgx* kaynağınız değerlerini portalında göründüğü gibi eşleşmesi gerekir. Tanımlanan bir SAS İlkesi (örnekte "SendRule") portal ve sahip olduğunuzdan emin olun *Gönder* izin verilir.  
* Bir güncelleştirme sonrası olay hub'ı artık gelen veya giden olay etkinliğini gösterir.

    İlk olarak, olay hub'ı ve yapılandırma bilgileri daha önce açıklandığı gibi doğru olduğundan emin olun. Bazen **PrivateConfig** dağıtım güncelleştirmede sıfırlanır. Tüm değişiklik yapmak için önerilen düzeltme olan *.wadcfgx* proje ve ardından itme tam uygulama güncelleştirmesi. Bu mümkün değilse, tanılama güncelleştirme tamamı iter emin olun **PrivateConfig** SAS anahtarını içerir.  
* Öneriler denedi ve olay hub'ı hala çalışmıyor.

    Günlükleri ve hataları Azure tanılama için kendisini içeren Azure Storage tablo bakarak deneyin: **WADDiagnosticInfrastructureLogsTable**. Bir seçenektir bir aracı gibi kullanmayı [Azure Storage Gezgini](http://www.storageexplorer.com) bu depolama hesabına bağlanmak için bu tabloyu görüntülemek ve bir sorgu için zaman damgası son 24 saat içinde ekleyin. Microsoft Excel gibi bir uygulamada açma ve bir .csv dosyasına dışarı aktarma için aracını kullanabilirsiniz. Excel gibi için arama kartı dizeleri, arama yapmayı kolaylaştırır **EventHubs**, hangi hata bildirdi görmek için.  

## <a name="next-steps"></a>Sonraki adımlar
• [Event Hubs hakkında daha fazla bilgi edinin](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Ek: Azure tanılama yapılandırma dosyası (.wadcfgx) örneği tamamlayın
```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount>ACCOUNT_NAME</StorageAccount>
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="{account name}" key="{account key}" endpoint="{storage endpoint}" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

Tamamlayıcı *ServiceConfiguration.Cloud.cscfg* için bu örnek aşağıdaki gibi görünür.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

Sanal makineler için eşdeğer tabanlı Json ayarlar aşağıdaki gibidir:
```JSON
"settings": {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 4096,
            "sinks": "applicationInsights.errors",
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "Directories": {
                "scheduledTransferPeriod": "PT1M",
                "IISLogs": {
                    "containerName": "wad-iis-logfiles"
                },
                "FailedRequestLogs": {
                    "containerName": "wad-failedrequestlogs"
                }
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "sinks": "HotPath",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Memory\\Available MBytes",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\Bytes Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Queued",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Rejected",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
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
                "scheduledTransferLogLevelFilter": "Error",
                "sinks": "HotPath"
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "HotPath",
                    "EventHub": {
                        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
                        "SharedAccessKeyName": "SendRule"
                    }
                },
                {
                    "name": "applicationInsights",
                    "ApplicationInsights": "",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "errors"
                            }
                        ]
                    }
                }
            ]
        }
    },
    "StorageAccount": "{account name}"
}


"protectedSettings": {
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{storage endpoint}",
    "EventHub": {
        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "YOUR_KEY_HERE"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](../event-hubs/event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](../event-hubs/event-hubs-create.md)
* [Event Hubs ile ilgili SSS](../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png
