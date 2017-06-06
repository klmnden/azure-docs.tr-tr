---
title: "Azure Service Bus kuyruklarını kullanan bir program yazma | Microsoft Docs"
description: "Service Bus mesajlaşması için C# konsolu uygulaması yazma"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 03/23/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: f92909e0098a543f99baf3df3197a799bc9f1edc
ms.openlocfilehash: 83649bdad1d369cdfe4edf3c2bdaa67180db8668
ms.contentlocale: tr-tr
ms.lasthandoff: 02/28/2017


---
# <a name="get-started-with-service-bus-queues"></a>Service Bus kuyruklarını kullanmaya başlama
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Ne elde edilecek
Bu öğreticide aşağıdaki işlemler tamamlanacaktır:

1. Azure portalı ile Service Bus ad alanı oluşturma.
2. Azure portalı ile Service Bus Mesajlaşma kuyruğu oluşturma.
3. İleti göndermek için bir konsol uygulaması yazma.
4. İleti almak için bir konsol uygulaması yazma.

## <a name="prerequisites"></a>Ön koşullar
1. [Visual Studio 2015 veya üzeri](http://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2015 kullanılır.
2. Azure aboneliği.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Azure portalı kullanılarak ad alanı oluşturma
Daha önce oluşturduğunuz bir Service Bus ad alanı varsa [Azure portalını kullanarak kuyruk oluşturma](#2-create-a-queue-using-the-azure-portal) bölümüne atlayın.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a>2. Azure portalını kullanarak kuyruk oluşturma
Daha önce oluşturduğunuz bir Service Bus kuyruğu varsa [Kuyruğa ileti gönderme](#3-send-messages-to-the-queue) bölümüne atlayın.

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a>3. Kuyruğa ileti gönderme
Kuyruğa ileti göndermek için, Visual Studio’yu kullanarak bir C# konsolu uygulaması yazacağız.

### <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

- Visual Studio'yu başlatın ve yeni bir Konsol uygulaması oluşturun.

### <a name="add-the-service-bus-nuget-package"></a>Service Bus NuGet paketi ekleme
1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. **Gözat** sekmesine tıklayın, ardından “Microsoft Azure Service Bus” araması yapın ve **Microsoft Azure Service Bus**’ı seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.
   
    ![NuGet paketi seçme][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-queue"></a>Kuyruğa ileti göndermek için kod yazma
1. Aşağıdaki deyimi Program.cs dosyasının üst kısmına ekleyin.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Aşağıdaki kodu `Main` yöntemine ekleyin, **connectionString** değişkenini ad alanı oluşturulurken el edilen bağlantı dizesi olarak ayarlayın ve **queueName** değişkenini kuyruk oluşturulurken kullanılan kuyruk adı olarak ayarlayın.
   
    ```csharp
    var connectionString = "<Your connection string>";
    var queueName = "<Your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");
    client.Send(message);
    ```
   
    Program.cs dosyanız aşağıdaki gibi görünmelidir.
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "<Your connection string>";
                var queueName = "<Your queue name>";
   
                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");
   
                client.Send(message);
            }
        }
    }
    ```
3. Programı çalıştırın ve Azure portalı denetleyin. Ad alanı **Genel Bakış** dikey penceresinde kuyruğunuzun adına tıklayın. **Etkin ileti sayısı**’nın şimdi 1 olması gerektiğini fark edebilirsiniz.
   
      ![İleti sayısı][queue-message]

## <a name="4-receive-messages-from-the-queue"></a>4. Kuyruktan ileti alma
1. Yeni bir konsol uygulaması oluşturun ve Service Bus NuGet paketine, daha önceki gönderme uygulamasına benzer bir başvuru ekleyin.
2. Aşağıdaki `using` deyimini Program.cs dosyasının üst kısmına ekleyin.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Aşağıdaki kodu `Main` yöntemine ekleyin, **connectionString** değişkenini ad alanı oluşturulurken el edilen bağlantı dizesi olarak ayarlayın ve **queueName** değişkenini kuyruk oluşturulurken kullandığınız kuyruk adı olarak ayarlayın.
   
    ```csharp
    var connectionString = "";
    var queueName = "samplequeue";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.ReadLine();
    ```
   
    Program.cs dosyanız aşağıdaki gibi görünmelidir:
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "";
          var queueName = "samplequeue";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });
   
          Console.ReadLine();
        }
      }
    }
    ```
4. Programı çalıştırın ve portalı denetleyin. **Kuyruk Uzunluğu** değeri şu anda 0 olmalıdır.
   
    ![Kuyruk uzunluğu][queue-message-receive]

Tebrikler! Bir kuyruk oluşturdunuz, ileti gönderdiniz ve ileti aldınız.

## <a name="next-steps"></a>Sonraki adımlar
Azure Service Bus Mesajlaşması'nın daha gelişmiş özelliklerinden bazılarını gösteren [örnekleri içeren GitHub depomuza](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) göz atın.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

