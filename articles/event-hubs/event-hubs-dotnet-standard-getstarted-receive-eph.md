---
title: ".NET standart kitaplığı kullanılarak Azure Event Hubs'tan gelen olayları alma | Microsoft Docs"
description: "EventProcessorHost bulunan iletiler alma standart .NET içinde kullanmaya başlama"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2017
ms.author: sethm
ms.openlocfilehash: a88b5da8fa504e0528caa7fa212d4cec26d1cf66
ms.sourcegitcommit: 651a6fa44431814a42407ef0df49ca0159db5b02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-standard"></a>Olay işleyicisi konağı iletilerle .NET standart alma kullanmaya başlama

> [!NOTE]
> Bu örnek edinilebilir [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).

Bu öğretici kullanarak bir event hub'ından iletileri alan bir .NET Core konsol uygulamasının nasıl yazılacağını gösterir **olay işleyicisi konağı** kitaplığı. Çalıştırabilirsiniz [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) çözümü olarak-olduğundan, olay hub'ı ve depolama hesabı değerleriniz ile dizeleri değiştirme. Veya, kendi oluşturmak için Bu öğreticide adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* [Microsoft Visual Studio 2015 veya 2017](http://www.visualstudio.com). Bu öğretici kullanım Visual Studio 2017 ancak Visual Studio 2015 örneklerde de desteklenir.
* [.NET core Visual Studio 2015 veya 2017 Araçları](http://www.microsoft.com/net/core).
* Azure aboneliği.
* Bir Azure Event Hubs ad alanı.
* Bir Azure depolama hesabı.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma  

İlk adım kullanmaktır [Azure portal](https://portal.azure.com) olay hub'ları türü için bir ad alanı oluşturmak ve event hub ile iletişim kurması için uygulamanız gereken yönetim kimlik bilgilerini elde etmek için. Bir ad alanı ve olay hub'ı oluşturmak için yordamı izleyin [bu makalede](event-hubs-create.md)ve ardından Bu öğretici ile devam edin.  

## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma  

1. [Azure Portal](https://portal.azure.com) oturum açın.  
2. Portalın sol gezinti bölmesinde tıklatın **yeni**, tıklatın **depolama**ve ardından **depolama hesabı**.  
3. Depolama hesabı penceresindeki alanları doldurun ve ardından **oluşturma**.

    ![Depolama hesabı oluştur][1]

4. Gördükten sonra **dağıtımları başarılı** ileti, yeni depolama hesabı adını tıklatın. İçinde **Essentials** penceresinde tıklatın **BLOB'lar**. Zaman **Blob hizmeti** iletişim kutusunu açar, tıklatın **+ kapsayıcı** üstünde. Kapsayıcı bir ad verin ve ardından kapatın **Blob hizmeti**.  
5. Tıklatın **erişim anahtarları** sol pencere kopyalayıp adını depolama kapsayıcısı, depolama hesabı ve değerini **key1**. Bu değerleri Not Defteri'nde veya başka bir geçici konuma kaydedin.  

## <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Visual Studio’yu çalıştırın. **Dosya** menüsünde **Yeni**'ye ve ardından **Proje**'ye tıklayın. .NET Core konsol uygulaması oluşturun.

![Yeni proje][2]

## <a name="add-the-event-hubs-nuget-package"></a>Olay hub'ları NuGet paketi ekleme

Ekleme [ **Microsoft.Azure.EventHubs** ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) ve [ **Microsoft.Azure.EventHubs.Processor** ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET standart kitaplığı NuGet paketleri için Aşağıdaki adımları izleyerek, projenizin: 

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. Tıklatın **Gözat** sekmesinde, arama **Microsoft.Azure.EventHubs**ve ardından **Microsoft.Azure.EventHubs** paket. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.
3. 1 ve 2. adımları yineleyin ve yükleme **Microsoft.Azure.EventHubs.Processor** paket.

## <a name="implement-the-ieventprocessor-interface"></a>Ieventprocessor arabirimini uygulama

1. Çözüm Gezgini'nde projeye sağ tıklayın, **Ekle**ve ardından **sınıfı**. Yeni sınıf **SimpleEventProcessor**.

2. SimpleEventProcessor.cs dosyasını açın ve aşağıdakileri ekleyin `using` deyimlerini dosyanın en üstüne ekleyin.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. Uygulama `IEventProcessor` arabirimi. Tüm içeriğini değiştirin `SimpleEventProcessor` aşağıdaki kodla sınıfı:

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

## <a name="write-a-main-console-method-that-uses-the-simpleeventprocessor-class-to-receive-messages"></a>SimpleEventProcessor sınıfı iletileri almak için kullandığı bir ana Konsolu yöntemi yazma

1. Aşağıdaki `using` deyimlerini Program.cs dosyasının üst kısmına ekleyin.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. Sabitlere eklemek `Program` olay hub bağlantı dizesine, olay hub'ı adı, depolama hesabı kapsayıcı adı, depolama hesabı adı ve depolama hesabı anahtarı için sınıf. Yer tutucuları bunlara karşılık gelen değerler ile değiştirerek aşağıdaki kodu ekleyin.

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Adlı yeni bir yöntem ekleyin `MainAsync` için `Program` sınıfı, şu şekilde:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EhEntityPath,
            PartitionReceiver.DefaultConsumerGroupName,
            EhConnectionString,
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

3. Aşağıdaki kod satırını ekleyin `Main` yöntemi:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Program.cs dosyanız aşağıdaki gibi görünmelidir:

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";
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
                    EhEntityPath,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EhConnectionString,
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

Tebrikler! İletilerin bir event hub'ından olay işleyicisi konağı kullanarak şimdi aldınız.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
