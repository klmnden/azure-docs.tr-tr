---
title: "Azure Service Bus kuyrukları ile çalışmaya başlama | Microsoft Docs"
description: "Service Bus mesajlaşması kuyruklarını kullanan bir C# konsol uygulaması yazın."
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
ms.date: 06/26/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 02d0ce093bc42cffa4f3993826c61c8aeca4d033
ms.contentlocale: tr-tr
ms.lasthandoff: 07/01/2017

---
# <a name="get-started-with-service-bus-queues"></a>Service Bus kuyrukları ile çalışmaya başlama
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Ne elde edilecek
Bu öğreticide aşağıdaki adımlar yer almaktadır:

1. Azure portalı ile Service Bus ad alanı oluşturma.
2. Azure portalını kullanarak Service Bus kuyruğu oluşturma.
3. İleti göndermek için bir konsol uygulaması yazma.
4. Önceki adımda gönderilen iletileri almak için bir konsol uygulaması yazma.

## <a name="prerequisites"></a>Ön koşullar
1. [Visual Studio 2015 veya üzeri](http://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılmaktadır.
2. Azure aboneliği.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Azure portalı kullanılarak ad alanı oluşturma
Daha önce bir Service Bus Mesajlaşması ad alanı oluşturduysanız [Azure portalını kullanarak kuyruk oluşturma](#2-create-a-queue-using-the-azure-portal) bölümüne atlayın.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a>2. Azure portalını kullanarak kuyruk oluşturma
Daha önce bir Service Bus kuyruğu oluşturduysanız [Kuyruğa ileti gönderme](#3-send-messages-to-the-queue) bölümüne atlayın.

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a>3. Kuyruğa ileti gönderme
Kuyruğa ileti göndermek için, Visual Studio'yu kullanarak bir C# konsol uygulaması yazacağız.

### <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Visual Studio'yu başlatın ve yeni bir **Konsol uygulaması (.NET Framework)** projesi oluşturun.

### <a name="add-the-service-bus-nuget-package"></a>Service Bus NuGet paketi ekleme
1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. **Gözat** sekmesine tıklayın, **Microsoft Azure Service Bus**'ı bulun ve ardından **WindowsAzure.ServiceBus** öğesini seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.
   
    ![NuGet paketi seçme][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-queue"></a>Kuyruğa ileti göndermek için kod yazma
1. Aşağıdaki `using` deyimini Program.cs dosyasının üst kısmına ekleyin.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Aşağıdaki kodu `Main` yöntemine ekleyin. `connectionString` değişkenini, ad alanını oluştururken edindiğiniz bağlantı dizesi olarak; `queueName` değişkenini ise kuyruğu oluştururken kullandığınız kuyruk adı olarak belirleyin.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    Program.cs dosyanızın şu biçimde olması gerekir:
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace qsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var queueName = "<your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Programı çalıştırın ve Azure portalını denetleyin: Ad alanına ilişkin **Genel Bakış** dikey penceresinde kuyruğunuzun adına tıklayın. Kuyruğa ilişkin **Temel Parçalar** dikey penceresi görüntülenir. **Etkin Mesaj Sayısı** değerinin 1 olduğuna dikkat edin. Gönderen uygulamayı iletileri almadan her çalıştırdığınızda bu değer 1 artar. Ayrıca uygulama kuyruğa her ileti eklediğinde kuyruğun geçerli boyutunun artış gösterdiğine dikkat edin.
   
      ![İleti boyutu][queue-message]

## <a name="4-receive-messages-from-the-queue"></a>4. Kuyruktan ileti alma

1. Gönderdiğiniz iletileri almak için yeni bir konsol uygulaması oluşturun ve önceki gönderen uygulamaya benzer şekilde, Service Bus NuGet paketine başvuru ekleyin.
2. Aşağıdaki `using` deyimini Program.cs dosyasının üst kısmına ekleyin.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Aşağıdaki kodu `Main` yöntemine ekleyin. `connectionString` değişkenini, ad alanı oluşturulurken edinilen bağlantı dizesi olarak; `queueName` değişkenini ise kuyruğu oluştururken kullandığınız kuyruk adı olarak belirleyin.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER to exit program");
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
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var queueName = "<your queue name>";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.WriteLine("Press ENTER to exit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. Programı çalıştırın ve portalı tekrar denetleyin. Bu işlemden sonra **Etkin Mesaj Sayısı** ve **Geçerli** değerlerinin 0 olduğuna dikkat edin.
   
    ![Kuyruk uzunluğu][queue-message-receive]

Tebrikler! Bir kuyruk oluşturdunuz, ileti gönderdiniz ve ileti aldınız.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşmasının daha gelişmiş özelliklerini gösteren [örneklerin bulunduğu GitHub depomuza](https://github.com/Azure/azure-service-bus/tree/master/samples) göz atın.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

