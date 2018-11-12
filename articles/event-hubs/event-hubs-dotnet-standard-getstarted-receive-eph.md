---
title: .NET Standard kitaplığı kullanarak Azure Event Hubs’dan olay alma | Microsoft Belgeleri
description: .NET Standard'da EventProcessorHost ile iletiler almaya başlama
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 9adbd8b9e7934ebe454d14ac6e47fe96898c9184
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234400"
---
# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-standard"></a>.NET Standard'da Olay İşlemcisi Konağı ile iletiler almaya başlama
Event Hubs bağlı cihaz ve uygulamalardan büyük miktarlarda olay verileri (telemetri) işleyen bir hizmettir. Verileri Event Hubs’a topladıktan sonra bir depolama kümesi kullanarak depolayabilir veya gerçek zamanlı bir analiz sağlayıcısı kullanarak dönüştürebilirsiniz. Bu büyük ölçekli olay toplama ve işleme özelliği, Nesnelerin İnterneti (IoT) gibi modern uygulama mimarilerinin temel bir bileşenidir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğreticide, [Olay İşleyicisi Ana Bilgisayarı](event-hubs-event-processor-host.md)’nı kullanarak bir olay hub’ından iletiler alan .NET Core konsol uygulamasını yazma işlemi gösterilmektedir. [Olay İşleyicisi Ana Bilgisayarı](event-hubs-event-processor-host.md), olay hub’larına ait kalıcı denetim noktalarını ve paralel alımları yöneterek bu olay hub’larına ait alma olaylarını basitleştiren bir .NET sınıfıdır. Olay İşleyicisi Ana Bilgisayarı’nı kullanarak, farklı düğümlerde barındırıldığında bile birden çok alıcı arasında olayları bölebilirsiniz. Bu örnek, tek alıcı için Olay İşleyicisi Ana Bilgisayarı'nın nasıl kullanıldığını göstermektedir. [Olay işleme ölçeğini genişletme] [Olay Hub’ları içeren Olay İşleme Ölçeğini Genişletme] örneği, birden çok alıcıyla Olay İşleyicisi Ana Bilgisayarı'nın nasıl kullanılacağını göstermektedir.

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver)’dan örnek olarak indirebilir ve `EventHubConnectionString` ve `EventHubName`, `StorageAccountName`, `StorageAccountKey` ve `StorageContainerName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
* [Microsoft Visual Studio 2015 veya 2017](https://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılır, ama Visual Studio 2015 de desteklenir.
* [.NET Core Visual Studio 2015 veya 2017 araçları](https://www.microsoft.com/net/core).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma
İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için [bu makalede](event-hubs-create.md) verilen yordamı uygulayın, ardından bu öğreticide yer alan aşağıdaki adımlarla devam edin.

[!INCLUDE [event-hubs-create-storage](../../includes/event-hubs-create-storage.md)]

## <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Visual Studio’yu çalıştırın. **Dosya** menüsünde **Yeni**'ye ve ardından **Proje**'ye tıklayın. Bir .NET Core konsol uygulaması oluşturun.

![Yeni proje][2]

## <a name="add-the-event-hubs-nuget-package"></a>Event Hubs NuGet paketini ekleme

Aşağıdaki adımları izleyerek [**Microsoft.Azure.EventHubs**](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) ve [**Microsoft.Azure.EventHubs.Processor**](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard library NuGet paketlerini projenize ekleyin: 

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. **Gözat** sekmesine tıklayın, **Microsoft.Azure.EventHubs** için arama yapın ve **Microsoft.Azure.EventHubs** paketini seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.
3. 1. ve 2. adımları yineleyin, **Microsoft.Azure.EventHubs.Processor** paketini yükleyin.

## <a name="implement-the-ieventprocessor-interface"></a>IEventProcessor arabirimini gerçekleştirme

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

## <a name="update-the-main-method-to-use-simpleeventprocessor"></a>SimpleEventProcessor’ı kullanmak için Ana yöntemi güncelleştirme

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

Tebrikler! Olay İşlemcisi Konağı'nı kullanarak bir olay hub’ından iletiler aldınız.

> [!NOTE]
> Bu öğretici, [EventProcessorHost](event-hubs-event-processor-host.md)’un tek bir örneğini kullanır. Verimliliği artırmak için, birden çok [EventProcessorHost](event-hubs-event-processor-host.md) örneğinin [Ölçeği genişletilmiş olay işleme](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3) örneğinde gösterildiği gibi çalıştırmanızı öneririz. Bu gibi durumlarda, alınan olayların yükünü dengelemek üzere birden çok örnek otomatik olarak birbiriyle koordine olur. 

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta bir olay hub’ından iletiler alan .Net Standart uygulaması oluşturdunuz. .NET Standardı kullanarak olay hub’ına olaylar göndermeyi öğrenmek için bkz. [Olay hub’ından olaylar gönderme, .NET Standard](event-hubs-dotnet-standard-getstarted-send.md).

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcorercv.png
