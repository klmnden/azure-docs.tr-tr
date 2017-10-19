---
title: "Azure Service Bus konuları ve abonelikleri ile çalışmaya başlama | Microsoft Docs"
description: "Service Bus mesajlaşma konuları ve aboneliklerini kullanan bir C# konsol uygulaması yazın."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 10/10/2017
ms.author: sethm
ms.openlocfilehash: 3646d14be662af0fdf80790cb53ddc581b33a146
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-service-bus-topics"></a>Service Bus konuları ile çalışmaya başlama

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu öğreticide aşağıdaki adımlar yer almaktadır:

1. Azure portalı ile Service Bus ad alanı oluşturma.
2. Azure portalı ile Service Bus konusu oluşturma.
3. Azure portalı ile bu konu için bir Service Bus aboneliği oluşturma.
4. Konuya ileti göndermek için bir konsol uygulaması yazma.
5. Abonelikten bu iletiyi almak için bir konsol uygulaması yazma.

## <a name="prerequisites"></a>Ön koşullar

1. [Visual Studio 2015 veya üzeri](http://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılmaktadır.
2. Azure aboneliği.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Azure portalı kullanılarak ad alanı oluşturma

Daha önce bir Service Bus Mesajlaşma ad alanı oluşturduysanız [Azure portalını kullanarak konu oluşturma](#2-create-a-topic-using-the-azure-portal) bölümüne atlayın.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-the-azure-portal"></a>2. Azure portalını kullanarak konu oluşturma

1. [Azure portalında][azure-portal] oturum açın.
2. Portalın sol tarafındaki gezinme bölmesinde **Service Bus**'a tıklayın (**Service Bus** yoksa **Diğer hizmetler**'e tıklayın).
3. Konuyu oluşturmak istediğiniz ad alanına tıklayın. Ad alanına genel bakış dikey penceresi görünür:
   
    ![Konu başlığı oluşturma][createtopic1]
4. **Service Bus ad alanı** dikey penceresinde **Konular** ve ardından **Konu ekle** düğmesine tıklayın.
   
    ![Konu Seçme][createtopic2]
5. Konu için bir ad girin ve **Bölümlemeyi etkinleştir** seçeneğini kaldırın. Diğer seçenekleri varsayılan değerlerinde bırakın.
   
    ![Yeni Seçme][createtopic3]
6. Dikey pencerenin altında yer alan **Oluştur** düğmesine tıklayın.

## <a name="3-create-a-subscription-to-the-topic"></a>3. Konuya abonelik oluşturma

1. Portal kaynakları bölmesinde, 1. adımda oluşturduğunuz ad alanına tıklayın ve ardından 2. adımda oluşturduğunuz konu adına tıklayın.
2. Genel bakış bölmesinin üst kısmında **Abonelik** seçeneğinin yanındaki artı işaretine tıklayarak bu konuya bir abonelik ekleyin.

    ![Abonelik oluşturma][createtopic4]

3. Abonelik için bir ad girin. Diğer seçenekleri varsayılan değerlerinde bırakın.

## <a name="4-send-messages-to-the-topic"></a>4. Konuya ileti gönderme

Konuya ileti göndermek için Visual Studio kullanılarak bir C# konsol uygulaması yazılır.

### <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Visual Studio'yu başlatın ve yeni bir **Konsol uygulaması (.NET Framework)** projesi oluşturun.

### <a name="add-the-service-bus-nuget-package"></a>Service Bus NuGet paketi ekleme

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. **Gözat** sekmesine tıklayın, **WindowsAzure.ServiceBus** araması yapın ve **WindowsAzure.ServiceBus** öğesini seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.
   
    ![NuGet paketi seçme][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-topic"></a>Konuya ileti göndermek için kod yazma

1. Aşağıdaki `using` deyimini Program.cs dosyasının üst kısmına ekleyin.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. `Main` yöntemine aşağıdaki kodu ekleyin. `connectionString` değişkenini, ad alanını oluştururken elde ettiğiniz bağlantı dizesine, `topicName` değerini ise konuyu oluştururken kullandığınız ada ayarlayın.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    Program.cs dosyanız aşağıdaki gibi görünmelidir.
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace tsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var topicName = "<your topic name>";

                var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Programı çalıştırın ve Azure portalını denetleyin: Ad alanının **Genel Bakış** dikey penceresinde konunuzun adına tıklayın. Konunun **Temel Bileşenler** dikey penceresi gösterilir. Dikey pencerenin alt kısmının yanında listelenen aboneliklerde, her bir aboneliğin **İleti Sayısı** değerinin artık 1 olmasına dikkat edin. İletileri almadan gönderen uygulamasını her çalıştırdığınızda (sonraki bölümde açıklandığı gibi), bu değer 1 artar. Ayrıca, uygulamanın konuya/aboneliğe ileti eklediği her durumda konunun geçerli boyutu, **Temel Bileşenler** dikey penceresindeki **Geçerli** değerini artırır.
   
      ![İleti boyutu][topic-message]

## <a name="5-receive-messages-from-the-subscription"></a>5. Abonelikten ileti alma

1. Yeni gönderdiğiniz iletiyi veya iletileri almak için yeni bir konsol uygulaması oluşturun ve Service Bus NuGet paketine daha önceki gönderen uygulamasına benzer bir başvuru ekleyin.
2. Aşağıdaki `using` deyimini Program.cs dosyasının üst kısmına ekleyin.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. `Main` yöntemine aşağıdaki kodu ekleyin. `connectionString` değişkenini, ad alanını oluştururken elde ettiğiniz bağlantı dizesine, `topicName` değerini ise konuyu oluştururken kullandığınız ada ayarlayın. `<your subscription name>` yerine 3. adımda oluşturduğunuz aboneliğin adını yazmayı unutmayın. 
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
   
    namespace GettingStartedWithTopics
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var topicName = "<your topic name>";
   
          var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
4. Programı çalıştırın ve portalı tekrar denetleyin. **İleti Sayısı** ve **Geçerli** değerlerinin artık 0 olduğuna dikkat edin.
   
    ![Konu uzunluğu][topic-message-receive]

Tebrikler! Bir konu ve abonelik oluşturdunuz, ileti gönderdiniz ve bu iletiyi aldınız.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşmasının daha gelişmiş özelliklerini gösteren [örneklerin bulunduğu GitHub depomuza](https://github.com/Azure/azure-service-bus/tree/master/samples) göz atın.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com
