<properties
    pageTitle="Service Bus kuyruklarını kullanmaya başlama | Microsoft Azure"
    description="Service Bus mesajlaşması için C# konsolu uygulaması yazma"
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/23/2016"
    ms.author="jotaub;sethm"/>

# Service Bus Kuyruklarını kullanmaya başlama

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## Ne elde edilecek

Bu öğreticide aşağıdaki işlemler tamamlanacaktır:

1. Azure portalı ile Service Bus ad alanı oluşturma.

2. Azure portalı ile Service Bus Mesajlaşma kuyruğu oluşturma.

3. İleti göndermek için bir konsol uygulaması yazma.

4. İleti almak için bir konsol uygulaması yazma.

## Ön koşullar

1. [Visual Studio 2013 veya Visual Studio 2015](http://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2015 kullanılır.

2. Azure aboneliği.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## 1. Azure portalı kullanılarak ad alanı oluşturma

Daha önce oluşturduğunuz bir Service Bus ad alanı varsa [Azure portalını kullanarak kuyruk oluşturma](#2-create-a-queue-using-the-azure-portal) bölümüne atlayın.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## 2. Azure portalını kullanarak kuyruk oluşturma

Daha önce oluşturduğunuz bir Service Bus kuyruğu varsa [Kuyruğa ileti gönderme](#3-send-messages-to-the-queue) bölümüne atlayın.

[AZURE.INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## 3. Kuyruğa ileti gönderme

Kuyruğa ileti göndermek için, Visual Studio’yu kullanarak bir C# konsolu uygulaması yazacağız.

### Konsol uygulaması oluşturma

1. Visual Studio'yu başlatın ve yeni bir Konsol uygulaması oluşturun.

### Service Bus NuGet paketi ekleme

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.

2. **Gözat** sekmesine tıklayın, ardından “Microsoft Azure Service Bus” araması yapın ve **Microsoft Azure Service Bus**’ı seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.

    ![NuGet paketi seçme][nuget-pkg]

### Kuyruğa ileti göndermek için kod yazma

1. Aşağıdaki deyimi Program.cs dosyasının üst kısmına ekleyin.

    ```
    using Microsoft.ServiceBus.Messaging;
    ```
    
2. Aşağıdaki kodu `Main` yöntemine ekleyin, **connectionString** değişkenini ad alanı oluşturulurken el edilen bağlantı dizesi olarak ayarlayın ve **queueName** değişkenini kuyruk oluşturulurken kullanılan kuyruk adı olarak ayarlayın.

    ```
    var connectionString = "<Your connection string>";
    var queueName = "<Your queue name>";
  
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");
    client.Send(message);
    ```

    Program.cs dosyanız aşağıdaki gibi görünmelidir.

    ```
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
    
## 4. Kuyruktan ileti alma

1. Yeni bir konsol uygulaması oluşturun ve Service Bus NuGet paketine, daha önceki gönderme uygulamasına benzer bir başvuru ekleyin.

2. Aşağıdaki `using` deyimini Program.cs dosyasının üst kısmına ekleyin.
  
    ```
    using Microsoft.ServiceBus.Messaging;
    ```
  
3. Aşağıdaki kodu `Main` yöntemine ekleyin, **connectionString** değişkenini ad alanı oluşturulurken el edilen bağlantı dizesi olarak ayarlayın ve **queueName** değişkenini kuyruk oluşturulurken kullandığınız kuyruk adı olarak ayarlayın.

    ```
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

    ```
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

## Sonraki adımlar

Azure Service Bus mesajlaşmasının daha gelişmiş özelliklerinden bazılarını gösteren [örnekleri içeren GitHub depomuza](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) göz atın.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png


<!--Reference style links - using these makes the source content way more readable than using inline links-->

[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples


<!--HONumber=sep16_HO1-->


