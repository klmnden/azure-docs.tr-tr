---
title: .NET Core - Azure Event Hubs'ı kullanarak olayları gönderme | Microsoft Docs
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
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 2bf5825d3f13044d1c9bde2b05d1c93ecd967a0f
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57782046"
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-core"></a>.NET Core, Azure Event hubs'a ileti göndermeye başlama
Event Hubs bağlı cihaz ve uygulamalardan büyük miktarlarda olay verileri (telemetri) işleyen bir hizmettir. Verileri Event Hubs’a topladıktan sonra bir depolama kümesi kullanarak depolayabilir veya gerçek zamanlı bir analiz sağlayıcısı kullanarak dönüştürebilirsiniz. Bu büyük ölçekli olay toplama ve işleme özelliği, Nesnelerin İnterneti (IoT) gibi modern uygulama mimarilerinin temel bir bileşenidir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğretici, .NET Core kullanılarak C# dilinde yazılmış bir konsol uygulaması ile bir olay hub’ına olay gönderme işlemini gösterir. 

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
* [Microsoft Visual Studio 2015 veya 2017](https://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılır, ama Visual Studio 2015 de desteklenir.
* [.NET Core Visual Studio 2015 veya 2017 araçları](https://www.microsoft.com/net/core). 

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma
İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md).

Bağlantı dizesi olay hub'ı ad alanı için makaledeki yönergeleri izleyerek alın: [Bağlantı dizesini alma](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu öğreticide daha sonra'de bağlantı dizesini kullanın. 

## <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Visual Studio’yu çalıştırın. **Dosya** menüsünde **Yeni**'ye ve ardından **Proje**'ye tıklayın. Bir .NET Core konsol uygulaması oluşturun.

![Yeni proje][1]

## <a name="add-the-event-hubs-nuget-package"></a>Event Hubs NuGet paketini ekleme

Ekleme [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Core kitaplığı NuGet paketini projenize aşağıdaki adımları izleyerek: 

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. **Gözat** sekmesine tıklayın, "Microsoft.Azure.EventHubs" için arama yapın ve **Microsoft.Azure.EventHubs** paketini seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.

## <a name="write-code-to-send-messages-to-the-event-hub"></a>Olay hub'ına ileti göndermek için kod yazma

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

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, .NET Core kullanarak olay hub'ına ileti gönderdiniz. .NET Core kullanarak bir olay hub'ından olay alma konusunda bilgi almak için bkz: [olayları event hub'dan - .NET Core alma](event-hubs-dotnet-standard-getstarted-receive-eph.md).

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcoresnd.png
