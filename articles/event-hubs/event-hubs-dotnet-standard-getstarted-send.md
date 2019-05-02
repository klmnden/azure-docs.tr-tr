---
title: Gönder ve .NET Core - Azure Event Hubs kullanarak olay alma | Microsoft Docs
description: Bu makalede, Azure Event Hubs'a olayları gönderen bir .NET Core uygulaması oluşturmak için bir kılavuz sağlar.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: shvija
ms.openlocfilehash: e572a9e19da144be61faefacc370cf512f5079ac
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64725414"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-net-core"></a>Olayları göndermek veya .NET Core kullanarak Azure Event Hubs'tan gelen olayları alma
Event Hubs bağlı cihaz ve uygulamalardan büyük miktarlarda olay verileri (telemetri) işleyen bir hizmettir. Verileri Event Hubs’a topladıktan sonra bir depolama kümesi kullanarak depolayabilir veya gerçek zamanlı bir analiz sağlayıcısı kullanarak dönüştürebilirsiniz. Bu büyük ölçekli olay toplama ve işleme özelliği, Nesnelerin İnterneti (IoT) gibi modern uygulama mimarilerinin temel bir bileşenidir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğreticide, .NET Core uygulamaları oluşturma işlemi gösterilmektedir C# olayları göndermek veya bir olay hub'ından olay alma. 

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- [Microsoft Visual Studio 2015 veya 2017](https://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılır, ama Visual Studio 2015 de desteklenir.
- [.NET Core Visual Studio 2015 veya 2017 araçları](https://www.microsoft.com/net/core). 
- **Bir Event Hubs ad alanı ve bir olay hub'ı oluşturma**. İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md). Daha sonra **olay hub'ı ad alanı için bağlantı dizesi** makalesindeki yönergeleri izleyerek: [Bağlantı dizesini alma](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu öğreticide daha sonra'de bağlantı dizesini kullanın.

## <a name="send-events"></a>Olayları gönderme 
Bu bölüm bir olay hub'ına olayları göndermek için bir .NET Core konsol uygulaması oluşturma işlemini gösterir. 

### <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Visual Studio’yu çalıştırın. **Dosya** menüsünde **Yeni**'ye ve ardından **Proje**'ye tıklayın. Bir .NET Core konsol uygulaması oluşturun.

![Yeni proje](./media/event-hubs-dotnet-standard-getstarted-send/netcoresnd.png)

### <a name="add-the-event-hubs-nuget-package"></a>Event Hubs NuGet paketini ekleme

Ekleme [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Core kitaplığı NuGet paketini projenize aşağıdaki adımları izleyerek: 

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. **Gözat** sekmesine tıklayın, "Microsoft.Azure.EventHubs" için arama yapın ve **Microsoft.Azure.EventHubs** paketini seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Olay hub'ına ileti göndermek için kod yazma

1. Aşağıdaki `using` deyimlerini Program.cs dosyasının en üstüne ekleyin:

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. `Program` sınıfına Event Hubs bağlantı dizesi ve varlık yolu (bireysel olay hub'ı adı) için sabitleri ekleyin. Köşeli ayraçlar içindeki yer tutucuları olay hub'ı oluşturulurken edinilen uygun değerlerle değiştirin. `{Event Hubs connection string}` dizesinin olay hub'ı dizesi değil ad alanı düzeyi bağlantı dizesi olduğundan emin olun. 

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EventHubConnectionString = "{Event Hubs connection string}";
    private const string EventHubName = "{Event Hub path/name}";
    ```

3. `Program` sınıfına aşağıda gösterildiği gibi `MainAsync` adlı yeni bir yöntem ekleyin:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but this simple scenario
        // uses the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
        {
            EntityPath = EventHubName
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. `Program` sınıfına aşağıda gösterildiği gibi `SendMessagesToEventHub` adlı yeni bir yöntem ekleyin:

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. Aşağıdaki kodu `Program` sınıfındaki `Main` yöntemine ekleyin:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   Program.cs dosyanız aşağıdaki gibi görünmelidir.

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EventHubConnectionString = "{Event Hubs connection string}";
            private const string EventHubName = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EventHubConnectionString)
                {
                    EntityPath = EventHubName
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. Programı çalıştırın ve herhangi bir hata olmadığından emin olun.

## <a name="receive-events"></a>Olayları alma
Bu bölümde, bir olay hub'ı kullanarak'ından iletiler alan bir .NET Core konsol uygulamasını yazma işlemi gösterilmektedir [Event Processor Host](event-hubs-event-processor-host.md). [Olay İşleyicisi Ana Bilgisayarı](event-hubs-event-processor-host.md), olay hub’larına ait kalıcı denetim noktalarını ve paralel alımları yöneterek bu olay hub’larına ait alma olaylarını basitleştiren bir .NET sınıfıdır. Olay İşleyicisi Ana Bilgisayarı’nı kullanarak, farklı düğümlerde barındırıldığında bile birden çok alıcı arasında olayları bölebilirsiniz. Bu örnek, tek alıcı için Olay İşleyicisi Ana Bilgisayarı'nın nasıl kullanıldığını göstermektedir.
> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver)’dan örnek olarak indirebilir ve `EventHubConnectionString` ve `EventHubName`, `StorageAccountName`, `StorageAccountKey` ve `StorageContainerName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

[!INCLUDE [event-hubs-create-storage](../../includes/event-hubs-create-storage.md)]

### <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Visual Studio’yu çalıştırın. **Dosya** menüsünde **Yeni**'ye ve ardından **Proje**'ye tıklayın. Bir .NET Core konsol uygulaması oluşturun.

![Yeni proje](./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcorercv.png)

### <a name="add-the-event-hubs-nuget-package"></a>Event Hubs NuGet paketini ekleme

Aşağıdaki adımları izleyerek [**Microsoft.Azure.EventHubs**](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) ve [**Microsoft.Azure.EventHubs.Processor**](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard library NuGet paketlerini projenize ekleyin: 

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. **Gözat** sekmesine tıklayın, **Microsoft.Azure.EventHubs** için arama yapın ve **Microsoft.Azure.EventHubs** paketini seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.
3. 1. ve 2. adımları yineleyin, **Microsoft.Azure.EventHubs.Processor** paketini yükleyin.

### <a name="implement-the-ieventprocessor-interface"></a>IEventProcessor arabirimini gerçekleştirme

1. Çözüm Gezgini'nde projeye sağ tıklayın, **Ekle**'ye tıklayın ve ardından **Sınıf**'a tıklayın. Yeni sınıfa **SimpleEventProcessor** adını verin.

2. SimpleEventProcessor.cs dosyasını açın ve aşağıdaki `using` deyimlerini dosyanın en üstüne ekleyin.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. `IEventProcessor` arabirimini gerçekleştirin. `SimpleEventProcessor` sınıfının tüm içeriğini aşağıdaki kodla değiştirin:

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

### <a name="update-the-main-method-to-use-simpleeventprocessor"></a>SimpleEventProcessor’ı kullanmak için Ana yöntemi güncelleştirme

1. Aşağıdaki `using` deyimlerini Program.cs dosyasının üst kısmına ekleyin.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. `Program` sınıfına olay hub'ı bağlantı dizesi, olay hub'ı adı, depolama hesabı kapsayıcı adı, depolama hesabı adı ve depolama hesabı anahtarı için sabitleri ekleyin. Aşağıdaki kodu ekleyin (yer tutucular yerine ilgili değerleri kullanın):

    ```csharp
    private const string EventHubConnectionString = "{Event Hubs connection string}";
    private const string EventHubName = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. `Program` sınıfına aşağıda gösterildiği gibi `MainAsync` adlı yeni bir yöntem ekleyin:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EventHubName,
            PartitionReceiver.DefaultConsumerGroupName,
            EventHubConnectionString,
            StorageConnectionString,
            StorageContainerName);

        // Registers the Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER to stop worker.");
        Console.ReadLine();

        // Disposes of the Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. Aşağıdaki kod satırını `Main` yöntemine ekleyin:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Program.cs dosyanız aşağıdaki gibi görünmelidir:

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EventHubConnectionString = "{Event Hubs connection string}";
            private const string EventHubName = "{Event Hub path/name}";
            private const string StorageContainerName = "{Storage account container name}";
            private const string StorageAccountName = "{Storage account name}";
            private const string StorageAccountKey = "{Storage account key}";

            private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                Console.WriteLine("Registering EventProcessor...");

                var eventProcessorHost = new EventProcessorHost(
                    EventHubName,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EventHubConnectionString,
                    StorageConnectionString,
                    StorageContainerName);

                // Registers the Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER to stop worker.");
                Console.ReadLine();

                // Disposes of the Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. Programı çalıştırın ve herhangi bir hata olmadığından emin olun.


## <a name="next-steps"></a>Sonraki adımlar
Bu makaleleri okuyun:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Özellikler ve Azure Event Hubs terminolojisinde](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)


