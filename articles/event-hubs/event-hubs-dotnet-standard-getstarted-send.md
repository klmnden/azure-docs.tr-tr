---
title: "Azure Event Hubs'a .NET standart kullanarak olayları göndermek | Microsoft Docs"
description: "Event Hubs .NET standart, olayları göndermek kullanmaya başlama"
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
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: 8af9d70965c1c9ad8c49b7d2bb04244fc207058d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>Azure Event Hubs .NET standart içinde ileti göndermek kullanmaya başlama

> [!NOTE]
> Bu örnek edinilebilir [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).

Bu öğretici, bir dizi ileti olay hub'ına gönderir .NET Core konsol uygulamasının nasıl yazılacağını gösterir. Çalıştırabilirsiniz [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) çözümü olarak-yerini alacak olan `EhConnectionString` ve `EhEntityPath` dizeleri, olay hub'ı değerlerle. Veya, kendi oluşturmak için Bu öğreticide adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* [Microsoft Visual Studio 2015 veya 2017](http://www.visualstudio.com). Bu öğretici kullanım Visual Studio 2017 ancak Visual Studio 2015 örneklerde de desteklenir.
* [.NET core Visual Studio 2015 veya 2017 Araçları](http://www.microsoft.com/net/core).
* Azure aboneliği.
* Bir olay hub'ad alanı.

Olay hub'ına iletileri göndermek için Visual Studio bir C# konsol uygulaması yazmak için kullanacağız.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma

İlk adım kullanmaktır [Azure portal](https://portal.azure.com) olay hub'türü için bir ad alanı oluşturmak ve event hub ile iletişim kurması için uygulamanız gereken yönetim kimlik bilgilerini elde etmek için. Bir ad ve bir olay hub'ı oluşturmak için yordamı izleyin [bu makalede](event-hubs-create.md)ve ardından aşağıdaki adımlarla devam edin.

## <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Visual Studio’yu çalıştırın. **Dosya** menüsünde **Yeni**'ye ve ardından **Proje**'ye tıklayın. .NET Core konsol uygulaması oluşturun.

![Yeni Proje][1]

## <a name="add-the-event-hubs-nuget-package"></a>Olay hub'ları NuGet paketi ekleme

Ekleme [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) aşağıdaki adımları izleyerek projeniz .NET standart kitaplığı NuGet paketi: 

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. Tıklatın **Gözat** sekmesini ve ardından "Microsoft.Azure.EventHubs" ve select arama **Microsoft.Azure.EventHubs** paket. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a>Olay hub'ına iletileri göndermek için bazı kod yazma

1. Aşağıdaki `using` deyimlerini Program.cs dosyasının üst kısmına ekleyin.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Sabitlere eklemek `Program` sınıfı Event Hubs bağlantı dizesini ve varlık yolu için (tek olay hub'ı adı). Köşeli ayraçlar içindeki yer tutucuları olay hub'ı oluştururken edinilen uygun değerlerle değiştirin.

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Adlı yeni bir yöntem ekleyin `MainAsync` için `Program` sınıfı, şu şekilde:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
        // we are using the connection string from the namespace.
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

4. Adlı yeni bir yöntem ekleyin `SendMessagesToEventHub` için `Program` sınıfı, şu şekilde:

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

5. Aşağıdaki kodu ekleyin `Main` yönteminde `Program` sınıfı.

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
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
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
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Olay hub'larından olayları alma](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
