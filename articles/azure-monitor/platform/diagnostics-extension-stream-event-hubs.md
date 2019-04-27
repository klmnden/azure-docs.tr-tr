---
title: Azure tanılama verilerinin Event hubs'a Stream
description: Sık karşılaşılan senaryolara yönelik yönergeler de dahil olmak üzere uca, Event Hubs ile Azure tanılamayı yapılandırma.
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 07/13/2017
ms.author: robb
ms.subservice: diagnostic-extension
ms.openlocfilehash: c2d577bd4c89046136a3465ff554e9662dd0ce19
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60396176"
---
# <a name="streaming-azure-diagnostics-data-in-the-hot-path-by-using-event-hubs"></a>Event Hubs kullanarak Azure Tanılama verileri etkin yolu akış
Azure Tanılama, bulut Hizmetleri sanal makinelerden (VM'ler) ölçümlerini ve günlüklerini toplamak ve sonuçları Azure depolama alanına aktarmak için esnek bir yol sağlar. Mart 2016 (SDK 2.9) zaman çerçevesinde başlayarak, özel veri kaynakları için Tanılama verileri gönderme ve sık kullanılan yol veri aktarma saniyeler içinde kullanarak [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).

Desteklenen veri türleri şunlardır:

* Windows için Olay İzleme (ETW) olayları
* Performans sayaçları
* Windows olay günlükleri
* Uygulama günlükleri
* Azure Tanılama altyapısı günlükleri

Bu makalede Azure tanılama Event Hubs ile uçtan uca yapılandırma gösterilmektedir. Aşağıdaki ortak senaryolar için yönergeler de verilmektedir:

* Event Hubs için gönderilen ölçüm ve günlükleri özelleştirme
* Her ortamda yapılandırmalarını değiştirme
* Event Hubs akış verilerini görüntüleme
* Bağlantı sorunlarını giderme  

## <a name="prerequisites"></a>Önkoşullar
Event Hubs'a, Azure tanılama verilerini alma, bulut Hizmetleri, VM'ler, sanal makine ölçek kümeleri ve Service Fabric karşılık gelen Azure Araçları ve Azure SDK 2.9 ile Visual Studio için başlangıç desteklenir.

* Azure tanılama uzantısını 1.6 ([veya daha sonra .NET 2.9 için Azure SDK](https://azure.microsoft.com/downloads/) varsayılan olarak bu hedefler)
* [Visual Studio 2013 veya üzeri](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* Mevcut Azure tanılama yapılandırmalarını kullanarak uygulamada bir *.wadcfgx* dosya ve aşağıdaki yöntemlerden biri:
  * Visual Studio: [Azure bulut Hizmetleri ve sanal makineler için tanılamayı yapılandırma](/visualstudio/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines)
  * Windows PowerShell: [PowerShell kullanarak Azure bulut hizmetlerinde tanılamayı etkinleştirme](../../cloud-services/cloud-services-diagnostics-powershell.md)
* Event Hubs ad alanı makale sağlanan [Event Hubs ile çalışmaya başlama](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

## <a name="connect-azure-diagnostics-to-event-hubs-sink"></a>Azure Tanılama Olay hub'ları havuz bağlanın
Varsayılan olarak, Azure tanılama her zaman günlükleri ve ölçümleri bir Azure depolama hesabına gönderir. Bir uygulama Ayrıca verileri olay hub'ları için yeni bir ekleyerek gönderebilir **havuzlarını** bölümüne **PublicConfig** / **WadCfg** öğesinin *. wadcfgx* dosya. Visual Studio'da *.wadcfgx* dosya şu yolda depolanır: **Bulut hizmeti projesi** > **rolleri** > **(RoleName)** > **diagnostics.wadcfgx** dosya.

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

Bu örnekte, olay hub'ı tam ad alanı için olay hub'ı URL'si ayarlanır: Event Hubs ad alanı + "/" + olay hub'ı adı.  

Olay hub'ı URL'si görüntülendiği [Azure portalında](https://go.microsoft.com/fwlink/?LinkID=213885) Event Hubs Panoda.  

**Havuz** ad yapılandırma dosyası boyunca aynı değere tutarlı bir şekilde kullanılan sürece herhangi bir geçerli dize ayarlanabilir.

> [!NOTE]
> Olabilir ek havuzlarını aşağıdaki gibi *Applicationınsights* Bu bölümde yapılandırılmış. Azure tanılama her havuz olarak da bildirilirse tanımlanacak bir veya daha fazla havuzlarını sağlar **PrivateConfig** bölümü.  
>
>

Event Hubs havuz gerekir ayrıca bildirilebileceği ve varsayılan tanımlanan **PrivateConfig** bölümünü *.wadcfgx* yapılandırma dosyası.

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

`SharedAccessKeyName` Değeri bir paylaşılan erişim imzası (SAS) anahtarı ve içinde tanımlanan ilke eşleşmesi gereken **Event Hubs** ad alanı. Event Hubs Panoda göz atın [Azure portalında](https://portal.azure.com), tıklayın **yapılandırma** sekmesini tıklatıp sahip bir adlandırılmış İlkesi (örneğin, "SendRule") ayarlama *Gönder* izinleri. **StorageAccount** içinde bildirildiği **PrivateConfig**. Çalışıyorsanız ve o değerleri burada değiştirmenize gerek yoktur. Bu örnekte biz değerlerin boş bir aşağı akış varlık değerlerini ayarlayacak bir oturum olduğu bırakın. Örneğin, *ServiceConfiguration.Cloud.cscfg* ortamı yapılandırma dosyası, ortam uygun adları ve anahtarları ayarlar.  

> [!WARNING]
> Event hubs'ı SAS anahtarı, düz metin halinde depolanır *.wadcfgx* dosya. Genellikle, bu anahtarı kaynak kodu denetimine iade edildiğinde veya uygun şekilde korur, böylece yapı sunucunuzu bir varlığı olarak kullanılabilir. SAS anahtarı ile burada kullanmanızı öneririz *yalnızca gönderme* izinleri böylece kötü niyetli bir kullanıcı olay hub'ına yazma ancak dinleyebilir veya yönetme.
>
>

## <a name="configure-azure-diagnostics-to-send-logs-and-metrics-to-event-hubs"></a>Azure tanılama günlükleri ve ölçümleri Event Hubs'a gönderme yapılandırın
Tüm varsayılan ve özel tanılama veri daha önce açıklandığı gibi diğer bir deyişle, ölçüm ve günlükleri, otomatik olarak gönderilen Azure Depolama'ya yapılandırılan aralıklarla. Event Hubs ve herhangi ek bir havuza olay hub'ına gönderilecek hiyerarşideki herhangi bir kök veya yaprak düğümü belirtebilirsiniz. Bu işlem, ETW olayları, performans sayaçları, Windows olay günlükleri ve uygulama günlükleri de içerir.   

Kaç veri noktaları için Event Hubs gerçekten aktarılması gerektiğini göz önünde bulundurmanız önemlidir. Genellikle, geliştiriciler tüketilen ve hızlı bir şekilde yorumlanır düşük gecikme süreli hot yol verileri aktarın. Uyarıları veya otomatik ölçeklendirme kurallarını izleme sistemleri verilebilir. Bir geliştirici ayrıca diğer analiz deposunu yapılandırmak veya mağazada--Azure Stream Analytics, Elasticsearch, özel bir izleme sistemine veya sık kullandığınız bir izleme sistemine diğerlerinden arayın.

Aşağıda bazı örnek yapılandırmalar verilmiştir.

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

Yukarıdaki örnekte, havuz üst öğeye uygulanan **PerformanceCounters** hiyerarşideki tüm alt anlamına gelir düğüm **PerformanceCounters** Event Hubs'a gönderilecek.  

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

Önceki örnekte, havuz için yalnızca üç sayaç uygulanır: **Sıraya alınan istek**, **reddedilen istekleri**, ve **% işlemci zamanı**.  

Aşağıdaki örnek, bir geliştirici bu hizmetin sistem durumu için kullanılan önemli ölçümler için gönderilen veri miktarını nasıl sınırlayabilirsiniz gösterir.  

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

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Dağıtma ve bulut Hizmetleri uygulama ve Tanılama yapılandırmasını güncelleştir
Visual Studio uygulama ve Event Hubs havuz yapılandırmasını dağıtmak için en kolay yolunu sağlar. Görüntüleme ve dosyayı düzenlemek için açın *.wadcfgx* dosyasını Visual Studio'da, düzenleyin ve kaydedin. Yol **bulut hizmeti projesi** > **rolleri** > **(RoleName)** > **diagnostics.wadcfgx**.  

Bu noktada, tüm dağıtım ve dağıtım eylemleri Visual Studio, Visual Studio Team System ve tüm komutları veya betikleri MSBuild ve kullanım tabanlı güncelleştirme **/t: yayımlama** hedef dahil *.wadcfgx* paketleme işleminde. Ayrıca, dağıtımları ve güncelleştirmelerinin dosyasını Azure'a Vm'lerinizde uygun Azure tanılama Aracısı uzantısını kullanarak dağıtın.

Azure tanılama yapılandırma ve uygulamayı dağıttıktan sonra hemen olay hub'ın panosunda etkinlik görürsünüz. Bu, sık erişimli yol verileri dinleyicisi istemci veya Analiz aracında, tercih ettiğiniz görüntülemeye devam etmeye hazır gösterir.  

Aşağıdaki resimde sağlıklı, olay hub'ına süre 23: 00 sonra Başlangıç Tanılama verileri gönderme Event Hubs Pano gösterir. Bu durumda uygulama dağıtıldı güncelleştirilmiş ile *.wadcfgx* dosya ve havuz yapılandırılmışsa düzgün şekilde.

![][0]  

> [!NOTE]
> Azure tanılama yapılandırma dosyası (.wadcfgx) için güncelleştirmeleri yaptığınızda, güncelleştirmeleri yapılandırmanın yanı sıra uygulamanın tamamı için Visual Studio yayımlama veya bir Windows PowerShell komut dosyasını kullanarak anında iletme, önerilir.  
>
>

## <a name="view-hot-path-data"></a>Sık erişimli-path verilerini görüntüleme
Daha önce açıklandığı gibi dinleme ve Event Hubs verilerini işlemeye yönelik birçok kullanım örnekleri vardır.

Basit bir yaklaşım, olay hub'ına dinleyin ve çıkış akışına yazdırmak için bir küçük test konsol uygulaması oluşturmaktır. Daha ayrıntılı olarak açıklanan aşağıdaki kodu koyabilirsiniz [Event Hubs ile çalışmaya başlama](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)), bir konsol uygulamasında.  

Konsol uygulaması içermelidir Not [olay işlemcisi konağı NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Köşeli ayraçlar içine değerleri değiştirmeyi unutmayın **ana** kaynaklarınız için değerlerle işlevi.   

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
            string eventHubConnectionString = "Endpoint= <your connection string>”;
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

## <a name="troubleshoot-event-hubs-sinks"></a>Event Hubs havuzlarını sorunlarını giderme
* Olay hub'ı beklendiği gibi gelen veya giden olay etkinlik göstermez.

    Olay hub'ınıza başarıyla hazırlanana denetleyin. Tüm bağlantı bilgilerini **PrivateConfig** bölümünü *.wadcfgx* portalda görüldüğü gibi kaynağınızın değerler eşleşmelidir. Tanımlı bir SAS İlkesi (örneğin, "SendRule") portalı ve sahip olduğunuzdan emin olun *Gönder* izin verilir.  
* Olay hub'ı, güncelleştirmeden sonra gelen veya giden olay etkinlikleri artık gösterir.

    İlk olarak, olay hub'ı ve yapılandırma bilgisi daha önce açıklandığı gibi doğru olduğundan emin olun. Bazen **PrivateConfig** dağıtım güncelleştirmede sıfırlanır. Tüm değişiklik yapmak için önerilen düzeltmesidir *.wadcfgx* proje ve sonra anında iletme uygulamanın güncelleştirme. Bu mümkün değilse, tanılama güncelleştirme eksiksiz bir gönderim emin **PrivateConfig** , SAS anahtarını içerir.  
* Öneriler çalıştım ve olay hub'ı hala çalışmıyorsa.

    Azure tanılama için kendisini günlükleri ve hataları içeren Azure depolama tablosundaki bakmayın deneyin: **WADDiagnosticInfrastructureLogsTable**. Bir seçenek olduğu gibi bir araç kullanmak için [Azure Depolama Gezgini](https://www.storageexplorer.com) bu depolama hesabına bağlanmak için bu tablo görüntülemek ve bir sorgu için zaman damgası, son 24 saat içindeki ekleyin. Microsoft Excel gibi bir uygulama açın ve bir .csv dosyasına dışarı aktarma Aracı'nı kullanabilirsiniz. Excel gibi arama kartı dizeleri için arama yapmayı kolaylaştırır **EventHubs**hangi hata bildirilir görmek için.  

## <a name="next-steps"></a>Sonraki adımlar
• [Event Hubs hakkında daha fazla bilgi edinin](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Ek: Tam Azure tanılama yapılandırma dosyası (.wadcfgx) örneği
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

Tamamlayıcı *ServiceConfiguration.Cloud.cscfg* için bu örnekte aşağıdaki gibi görünür.

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

Sanal makineler için eşdeğer JSON ayarları aşağıdaki gibidir:

Genel ayarlar:
```JSON
{
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

```

Korumalı ayarları:
```JSON
{
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

* [Event Hubs’a genel bakış](../../event-hubs/event-hubs-about.md)
* [Olay Hub’ı oluşturma](../../event-hubs/event-hubs-create.md)
* [Event Hubs ile ilgili SSS](../../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png

