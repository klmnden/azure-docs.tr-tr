---
title: .NET Standard kullanarak olayları Azure Event Hubs’a gönderme | Microsoft Belgeleri
description: .NET Standard'da Event Hubs'a olay göndermeye başlama
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
ms.openlocfilehash: 4cd2fdb2bd8b6a15bc8dc3e4594971a61e1889e7
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "41920383"
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>.NET Standard'da Azure Event Hubs'a ileti göndermeye başlama

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender)'da sağlanır.

Bu öğreticide, olay hub'ına bir dizi ileti gönderen bir .NET Core konsol uygulamasının nasıl yazılacağı gösterilir. [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) çözümünü olduğu gibi, `EhConnectionString` ve `EhEntityPath` dizelerini kendi olay hub'ı değerlerinizle değiştirerek kullanabilirsiniz. Öte yandan, bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* [Microsoft Visual Studio 2015 veya 2017](http://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılır, ama Visual Studio 2015 de desteklenir.
* [.NET Core Visual Studio 2015 veya 2017 araçları](http://www.microsoft.com/net/core).
* Azure aboneliği.
* [Bir olay hub'ı ad alanı ve olay hub'ı](event-hubs-quickstart-portal.md).

Olay hub'ına ileti göndermek için, bu öğreticide Visual Studio kullanılarak bir C# konsol uygulaması yazılır.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma

Ad alanı ve olay hub'ı oluşturmak için [bu makalede](event-hubs-quickstart-portal.md) verilen yordamı izleyin ve ardından bu öğreticiyle devam edin.

## <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Visual Studio’yu çalıştırın. **Dosya** menüsünde **Yeni**'ye ve ardından **Proje**'ye tıklayın. Bir .NET Core konsol uygulaması oluşturun.

![Yeni proje][1]

## <a name="add-the-event-hubs-nuget-package"></a>Event Hubs NuGet paketini ekleme

Aşağıdaki adımları izleyerek [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard kitaplığı NuGet paketini projenize ekleyin: 

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. **Gözat** sekmesine tıklayın, "Microsoft.Azure.EventHubs" için arama yapın ve **Microsoft.Azure.EventHubs** paketini seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a>Olay hub'ına ileti göndermek için bazı kodlar yazma

1. Aşağıdaki `using` deyimlerini Program.cs dosyasının en üstüne ekleyin:

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. `Program` sınıfına Event Hubs bağlantı dizesi ve varlık yolu (bireysel olay hub'ı adı) için sabitleri ekleyin. Köşeli ayraçlar içindeki yer tutucuları olay hub'ı oluşturulurken edinilen uygun değerlerle değiştirin. `{Event Hubs connection string}` dizesinin olay hub'ı dizesi değil ad alanı düzeyi bağlantı dizesi olduğundan emin olun. 

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. `Program` sınıfına aşağıda gösterildiği gibi `MainAsync` adlı yeni bir yöntem ekleyin:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but this simple scenario
        // uses the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
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

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantılarda Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs'dan olayları alma](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcoresnd.png
