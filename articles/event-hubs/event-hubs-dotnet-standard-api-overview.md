---
title: Standart .NET API'ları Azure Event Hubs'a genel bakış | Microsoft Docs
description: .NET standart API genel bakış
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: a173f8e4-556c-42b8-b856-838189f7e636
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 855f6e7f401621d7f923d68215ca880c05d38629
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
ms.locfileid: "26783008"
---
# <a name="event-hubs-net-standard-api-overview"></a>Event Hubs .NET standart API genel bakış

Bu makalede olay hub'ları .NET standart istemci API anahtarı özetlenmektedir. Şu anda iki .NET standart istemci kitaplıkları vardır:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs): tüm temel çalışma zamanı işlemler sağlar.
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor): işlenen olayları izlemek sağlar ve bir event hub'ından okuma yapmanın en kolay yolu olan ek işlevsellik ekler.

## <a name="event-hubs-client"></a>Olay hub'ları istemci

[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) olayları göndermek, alıcılar oluşturmak ve çalışma zamanı bilgileri almak için kullandığınız birincil nesnesidir. Bu istemci belirli bir olay hub'ına bağlanır ve Event Hubs uç noktası için yeni bir bağlantı oluşturur.

### <a name="create-an-event-hubs-client"></a>Event Hubs istemcisi oluşturma

Bir [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) nesnesi, bir bağlantı dizesinden oluşturulur. Yeni bir istemci örneği oluşturmak için en basit yolu, aşağıdaki örnekte gösterilmiştir:

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("{Event Hubs connection string}");
```

Program aracılığıyla bağlantı dizesini düzenlemek için kullanabileceğiniz [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) sınıfı ve bağlantı dizesi parametresi olarak geçirin [EventHubClient.CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_CreateFromConnectionString_System_String_).

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("{Event Hubs connection string}")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a>Olayları gönderme

Olay hub'ına olayları göndermek için kullanmak [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) sınıfı. Gövdesi olmalıdır bir `byte` diziye veya `byte` dizi kesimi.

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a>Olayları alma

Olay hub'larından olaylarını almak için önerilen yöntem kullanılarak [olay işleyicisi konağı](#event-processor-host-apis), otomatik olarak olay hub'ı uzaklık ve bölüm bilgileri izlemek için işlevsellik sağlar. Ancak, olaylarını almak için temel olay hub'ları kitaplığı esnekliğini kullanmak isteyebilirsiniz bazı durumlar vardır.

#### <a name="create-a-receiver"></a>Alıcı oluşturma

Alıcıları belirli bölümlere, böylece tüm olayları bir event hub'ında almak için bağlıdır, birden çok örneği oluşturmanız gerekir. Bölüm bilgileri programlı şekilde, bölüm kimlikleri kodlamak yerine almak için iyi bir uygulamadır. Bunu yapmak için kullanabileceğiniz [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient#Microsoft_Azure_EventHubs_EventHubClient_GetRuntimeInformationAsync) yöntemi.

```csharp
// Create a list to keep track of the receivers
var receivers = new List<PartitionReceiver>();
// Use the eventHubClient created above to get the runtime information
var runTimeInformation = await eventHubClient.GetRuntimeInformationAsync();
// Loop over the resulting partition IDs
foreach (var partitionId in runTimeInformation.PartitionIds)
{
    // Create the receiver
    var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);
    // Add the receiver to the list
    receivers.Add(receiver);
}
```

Olayları bir event hub'ından hiçbir zaman kaldırılır (ve yalnızca sona çünkü) uygun başlangıç noktası belirtmeniz gerekir. Aşağıdaki örnek, olası birleşimleri gösterir:

```csharp
// partitionId is assumed to come from GetRuntimeInformationAsync()

// Using the constant PartitionReceiver.EndOfStream only receives all messages from this point forward.
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, PartitionReceiver.EndOfStream);

// All messages available
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, "-1");

// From one day ago
var receiver = eventHubClient.CreateReceiver(PartitionReceiver.DefaultConsumerGroupName, partitionId, DateTime.Now.AddDays(-1));
```

#### <a name="consume-an-event"></a>Bir olay kullanma

```csharp
// Receive a maximum of 100 messages in this call to ReceiveAsync
var ehEvents = await receiver.ReceiveAsync(100);
// ReceiveAsync can return null if there are no messages
if (ehEvents != null)
{
    // Since ReceiveAsync can return more than a single event you will need a loop to process
    foreach (var ehEvent in ehEvents)
    {
        // Decode the byte array segment
        var message = UnicodeEncoding.UTF8.GetString(ehEvent.Body.Array);
        // Load the custom property that we set in the send example
        var customType = ehEvent.Properties["Type"];
        // Implement processing logic here
    }
}       
```

## <a name="event-processor-host-apis"></a>Olay işlemcisi konağı API'leri

Bu API'leri kullanılabilir çalışanları bölüm dağıtarak, kullanılamayabilir çalışan işlemleri için esneklik sağlar.

```csharp
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.

// Read these connection strings from a secure location
var ehConnectionString = "{Event Hubs connection string}";
var ehEntityPath = "{event hub path/name}";
var storageConnectionString = "{Storage connection string}";
var storageContainerName = "{Storage account container name}";

var eventProcessorHost = new EventProcessorHost(
    ehEntityPath,
    PartitionReceiver.DefaultConsumerGroupName,
    ehConnectionString,
    storageConnectionString,
    storageContainerName);

// Start/register an EventProcessorHost
await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

// Disposes the Event Processor Host
await eventProcessorHost.UnregisterEventProcessorAsync();
```

Aşağıdaki örnek uygulamasıdır [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor).

```csharp
public class SimpleEventProcessor : IEventProcessor
{
    public Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
        return Task.CompletedTask;
    }

    public Task OpenAsync(PartitionContext context)
    {
        Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
        return Task.CompletedTask;
    }

    public Task ProcessErrorAsync(PartitionContext context, Exception error)
    {
        Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
        return Task.CompletedTask;
    }

    public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (var eventData in messages)
        {
            var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
            Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
        }

        return context.CheckpointAsync();
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
Event Hubs senaryoları hakkında daha fazla bilgi almak için aşağıdaki bağlantıları ziyaret edin:

* [Azure Event Hubs nedir?](event-hubs-what-is-event-hubs.md)
* [Kullanılabilir olay hub'ları API'leri](event-hubs-api-overview.md)

.NET API başvuru şunlardır:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)