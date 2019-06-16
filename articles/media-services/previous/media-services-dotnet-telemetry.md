---
title: .NET ile Azure Media Services telemetri yapılandırma | Microsoft Docs
description: Bu makalede .NET SDK kullanarak Azure Media Services telemetri kullanma gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: f8f55e37-0714-49ea-bf4a-e6c1319bec44
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 1ffaefc51121aeb7421d6e49a3c0e58c76d4391e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61464955"
---
# <a name="configuring-azure-media-services-telemetry-with-net"></a>.NET ile Azure Media Services telemetri yapılandırma 

Bu makalede .NET SDK kullanarak Azure Media Services (AMS) telemetri yapılandırırken sürebilir genel adımlar açıklanmaktadır. 

>[!NOTE]
>Ayrıntılı bir açıklama ne AMS telemetri ve nasıl tüketicilerimizin kullanmak için bkz. [genel bakış](media-services-telemetry-overview.md) makalesi.

Telemetri verileri aşağıdaki yollardan birini kullanabilir:

- Verileri doğrudan Azure tablo depolama (örneğin, depolama SDK'sını kullanarak) okuyabilir. Telemetri depolama tabloları açıklaması için bkz: **telemetri bilgilerini kullanan** içinde [bu](https://msdn.microsoft.com/library/mt742089.aspx) makalesi.

Or

- Destek, Media Services .NET SDK depolama veri okumak için kullanın. Bu makalede, belirtilen bir AMS hesabının için telemetri etkinleştirme ve nasıl Azure Media Services .NET SDK kullanarak ölçümlerin sorgulanacağı gösterilmektedir.  

## <a name="configuring-telemetry-for-a-media-services-account"></a>Media Services hesabı için telemetri yapılandırma

Telemetri etkinleştirmek için aşağıdaki adımları gerekir:

- Media Services hesabına bağlı depolama hesabının kimlik bilgilerini alın. 
- Bir bildirim uç noktası ile **EndPointType** kümesine **AzureTable** ve depolama tablosuna işaret eden endPointAddress.

```csharp
        INotificationEndPoint notificationEndPoint = 
                      _context.NotificationEndPoints.Create("monitoring", 
                      NotificationEndPointType.AzureTable,
                      "https://" + _mediaServicesStorageAccountName + ".table.core.windows.net/");
```

- İzlemek istediğiniz hizmetleri için izleme yapılandırmasını oluşturun. İzleme birden fazla yapılandırma ayarı izin verilir. 

```csharp
        IMonitoringConfiguration monitoringConfiguration = _context.MonitoringConfigurations.Create(notificationEndPoint.Id,
            new List<ComponentMonitoringSetting>()
            {
                new ComponentMonitoringSetting(MonitoringComponent.Channel, MonitoringLevel.Normal),
                new ComponentMonitoringSetting(MonitoringComponent.StreamingEndpoint, MonitoringLevel.Normal)
            });
```

## <a name="consuming-telemetry-information"></a>Telemetri bilgilerini kullanma

Alıcı telemetri bilgileri hakkında daha fazla bilgi için bkz. [bu](media-services-telemetry-overview.md) makalesi.

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

1. Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

2. Şu öğeye eklemek **appSettings** app.config dosyanızda tanımlanan:

    ```xml
        <add key="StorageAccountName" value="storage_name" />
    ```
 
## <a name="example"></a>Örnek  
    
Aşağıdaki örnek belirtilen bir AMS hesabının için telemetri etkinleştirme ve Azure Media Services .NET SDK kullanarak ölçümleri sorgulama işlemini gösterir.  

```csharp
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;

namespace AMSMetrics
{
    class Program
    {
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];

        private static readonly string _mediaServicesStorageAccountName =
            ConfigurationManager.AppSettings["StorageAccountName"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        private static IStreamingEndpoint _streamingEndpoint = null;
        private static IChannel _channel = null;

        static void Main(string[] args)
        {
            AzureAdTokenCredentials tokenCredentials =
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            _streamingEndpoint = _context.StreamingEndpoints.FirstOrDefault();
            _channel = _context.Channels.FirstOrDefault();

            var monitoringConfigurations = _context.MonitoringConfigurations;
            IMonitoringConfiguration monitoringConfiguration = null;

            // No more than one monitoring configuration settings is allowed.
            if (monitoringConfigurations.ToArray().Length != 0)
            {
                monitoringConfiguration = _context.MonitoringConfigurations.FirstOrDefault();
            }
            else
            {
                INotificationEndPoint notificationEndPoint =
                          _context.NotificationEndPoints.Create("monitoring",
                          NotificationEndPointType.AzureTable, GetTableEndPoint());

                monitoringConfiguration = _context.MonitoringConfigurations.Create(notificationEndPoint.Id,
                    new List<ComponentMonitoringSetting>()
                    {
                    new ComponentMonitoringSetting(MonitoringComponent.Channel, MonitoringLevel.Normal),
                    new ComponentMonitoringSetting(MonitoringComponent.StreamingEndpoint, MonitoringLevel.Normal)

                    });
            }

            //Print metrics for a Streaming Endpoint.
            PrintStreamingEndpointMetrics();

            Console.ReadLine();
        }

        private static string GetTableEndPoint()
        {
            return "https://" + _mediaServicesStorageAccountName + ".table.core.windows.net/";
        }

        private static void PrintStreamingEndpointMetrics()
        {
            Console.WriteLine(string.Format("Telemetry for streaming endpoint '{0}'", _streamingEndpoint.Name));

            DateTime timerangeEnd = DateTime.UtcNow;
            DateTime timerangeStart = DateTime.UtcNow.AddHours(-5);

            // Get some streaming endpoint metrics.
            var telemetry = _streamingEndpoint.GetTelemetry();

            var res = telemetry.GetStreamingEndpointRequestLogs(timerangeStart, timerangeEnd);

            Console.Title = "Streaming endpoint metrics:";

            foreach (var log in res)
            {
                Console.WriteLine("AccountId: {0}", log.AccountId);
                Console.WriteLine("BytesSent: {0}", log.BytesSent);
                Console.WriteLine("EndToEndLatency: {0}", log.EndToEndLatency);
                Console.WriteLine("HostName: {0}", log.HostName);
                Console.WriteLine("ObservedTime: {0}", log.ObservedTime);
                Console.WriteLine("PartitionKey: {0}", log.PartitionKey);
                Console.WriteLine("RequestCount: {0}", log.RequestCount);
                Console.WriteLine("ResultCode: {0}", log.ResultCode);
                Console.WriteLine("RowKey: {0}", log.RowKey);
                Console.WriteLine("ServerLatency: {0}", log.ServerLatency);
                Console.WriteLine("StatusCode: {0}", log.StatusCode);
                Console.WriteLine("StreamingEndpointId: {0}", log.StreamingEndpointId);
                Console.WriteLine();
            }

            Console.WriteLine();
        }

        private static void PrintChannelMetrics()
        {
            if (_channel == null)
            {
                Console.WriteLine("There are no channels in this AMS account");
                return;
            }

            Console.WriteLine(string.Format("Telemetry for channel '{0}'", _channel.Name));

            DateTime timerangeEnd = DateTime.UtcNow;
            DateTime timerangeStart = DateTime.UtcNow.AddHours(-5);

            // Get some channel metrics.
            var telemetry = _channel.GetTelemetry();

            var channelMetrics = telemetry.GetChannelHeartbeats(timerangeStart, timerangeEnd);

            // Print the channel metrics.
            Console.WriteLine("Channel metrics:");

            foreach (var channelHeartbeat in channelMetrics.OrderBy(x => x.ObservedTime))
            {
                Console.WriteLine(
                    "    Observed time: {0}, Last timestamp: {1}, Incoming bitrate: {2}",
                    channelHeartbeat.ObservedTime,
                    channelHeartbeat.LastTimestamp,
                    channelHeartbeat.IncomingBitrate);
            }

            Console.WriteLine();
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
