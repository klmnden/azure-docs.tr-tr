---
title: Standart .NET API'ları Azure Event Hubs'a genel bakış | Microsoft Docs
description: .NET standard API'sine genel bakış
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 08/13/2018
ms.author: shvija
ms.openlocfilehash: b09f39f45936a7c43dbc1ef109780315d62c768f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60821900"
---
# <a name="event-hubs-net-standard-api-overview"></a>Event Hubs .NET standart API'sine genel bakış

Bu makalede, Azure Event Hubs anahtar bazıları özetlenmektedir [.NET Standard istemci API'leri](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/). Şu anda Event Hubs için iki .NET Standard istemci kitaplıkları vardır:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs): Tüm temel çalışma zamanı işlemler sağlar.
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor): İşlenen olaylar izlemek sağlar ve en kolay yolu, bir olay hub'ından okumak için ek işlevler ekler.

## <a name="event-hubs-client"></a>Event Hubs istemcisi

[EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) olayları göndermek, alıcılar oluşturun ve çalışma zamanı bilgilerini almak için birincil nesnedir. Bu istemci, belirli bir olay hub'ına bağlanır ve Event hubs'ı uç noktasına yeni bir bağlantı oluşturur.

### <a name="create-an-event-hubs-client"></a>Event Hubs istemcisi oluşturma

Bir [EventHubClient](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) nesnesi, bir bağlantı dizesinden oluşturulur. Yeni bir istemci örneği oluşturmak için en basit yolu, aşağıdaki örnekte gösterilmiştir:

```csharp
var eventHubClient = EventHubClient.CreateFromConnectionString("Event Hubs connection string");
```

Program aracılığıyla bağlantı dizesini düzenlemek için kullanabileceğiniz [EventHubsConnectionStringBuilder](/dotnet/api/microsoft.azure.eventhubs.eventhubsconnectionstringbuilder) sınıfı ve bağlantı dizesini bir parametre olarak geçirmek [EventHubClient.CreateFromConnectionString](/dotnet/api/microsoft.azure.eventhubs.eventhubclient).

```csharp
var connectionStringBuilder = new EventHubsConnectionStringBuilder("Event Hubs connection string")
{
    EntityPath = EhEntityPath
};

var eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());
```

### <a name="send-events"></a>Olayları gönderme

Olay hub'ına olayları göndermek için [EventData](/dotnet/api/microsoft.azure.eventhubs.eventdata) sınıfı. Gövdesi olmalıdır bir `byte` diziye veya `byte` dizi kesimi.

```csharp
// Create a new EventData object by encoding a string as a byte array
var data = new EventData(Encoding.UTF8.GetBytes("This is my message..."));
// Set user properties if needed
data.Properties.Add("Type", "Informational");
// Send single message async
await eventHubClient.SendAsync(data);
```

### <a name="receive-events"></a>Olayları alma

Event Hubs'dan olay almak için önerilen yöntem kullanarak [Event Processor Host](#event-processor-host-apis), otomatik olarak olay hub'ı uzaklığı ve bölüm bilgileri izlemek için işlevsellik sağlar. Ancak, çekirdek Event Hubs kitaplığına esnekliği olaylarını almak için kullanmak isteyebilirsiniz bazı durumlar vardır.

#### <a name="create-a-receiver"></a>Alıcı oluşturma

Alıcılar belirli bölümlere, bu nedenle tüm olayları bir event hub'ında almak için bağlıdır, birden çok örnek oluşturma gerekir. ' % S'bölüm kimlikleri kodlamak yerine bölüm bilgileri programlı olarak almak için iyi bir uygulamadır. Bunu yapmak için kullanabileceğiniz [GetRuntimeInformationAsync](/dotnet/api/microsoft.azure.eventhubs.eventhubclient) yöntemi.

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

Olayları bir event hub'ından hiçbir zaman kaldırılır (ve yalnızca sona çünkü) uygun bir başlangıç noktası belirtmeniz gerekir. Aşağıdaki örnekte, olası eşleştirme birleşimlerini gösterilmektedir:

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

## <a name="event-processor-host-apis"></a>Olay işlemcisi konak API

Bu API'ler, bölümler arasında kullanılabilir çalışanlar dağıtarak, kullanılamayabilir çalışan işlemleri için dayanıklılık sağlar:

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

Aşağıdaki örnek uygulamasıdır [Ieventprocessor](/dotnet/api/microsoft.azure.eventhubs.processor.ieventprocessor) arabirimi:

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

.NET API başvuruları şunlardır:

* [Microsoft.Azure.EventHubs](/dotnet/api/microsoft.azure.eventhubs)
* [Microsoft.Azure.EventHubs.Processor](/dotnet/api/microsoft.azure.eventhubs.processor)
