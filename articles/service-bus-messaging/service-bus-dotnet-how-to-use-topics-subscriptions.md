---
title: "Azure Service Bus konuları ve abonelikleri ile çalışmaya başlama | Microsoft Docs"
description: "Service Bus mesajlaşma konuları ve aboneliklerini kullanan bir C# .NET Core konsol uygulaması yazın."
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
ms.date: 12/6/2017
ms.author: sethm
ms.openlocfilehash: aa75ac48d650f28d4aaeb612f2900d705cf71b5b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="get-started-with-service-bus-topics"></a>Service Bus konuları ile çalışmaya başlama

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu öğreticide aşağıdaki adımlar yer almaktadır:

1. Azure portalı ile Service Bus ad alanı oluşturma.
2. Azure portalı ile Service Bus konusu oluşturma.
3. Azure portalı ile bu konu için bir Service Bus aboneliği oluşturma.
4. Konuya bir dizi ileti göndermek için bir .NET Core konsol uygulaması yazın.
5. Bu iletileri abonelikten almak için bir .NET Core konsol uygulaması yazın.

## <a name="prerequisites"></a>Ön koşullar

1. [Visual Studio 2017 Güncelleştirme 3 (sürüm 15.3, 26730.01)](http://www.visualstudio.com/vs) veya sonraki sürümler.
2. [NET Core SDK](https://www.microsoft.com/net/download/windows), sürüm 2.0 veya sonraki sürümler.
2. Azure aboneliği.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Azure portalı kullanılarak ad alanı oluşturma

> [!NOTE] 
> Dilerseniz [PowerShell](/powershell/azure/get-started-azureps) kullanarak Service Bus ad alanı ve mesajlaşma varlıkları da oluşturabilirsiniz. Daha fazla bilgi için bkz. [Service Bus kaynaklarını yönetmek için PowerShell’i kullanma](service-bus-manage-with-ps.md).

Daha önce bir Service Bus Mesajlaşma ad alanı oluşturduysanız [Azure portalını kullanarak konu oluşturma](#2-create-a-topic-using-the-azure-portal) bölümüne atlayın.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-the-azure-portal"></a>2. Azure portalını kullanarak konu oluşturma

1. [Azure portalında][azure-portal] oturum açın.
2. Portalın sol tarafındaki gezinme bölmesinde **Service Bus**'a tıklayın (**Service Bus** yoksa **Tüm hizmetler**'e veya **Tüm kaynaklar**'a tıklayın). Konuyu oluşturmak istediğiniz ad alanına tıklayın. 
3. Ad alanına genel bakış penceresi görünür. **Konular**’a tıklayın:
   
    ![Konu başlığı oluşturma][createtopic1]
4. **Konu ekle**’ye tıklayın.
   
    ![Konu Seçme][createtopic2]
5. Konu için bir ad girin. Diğer seçenekleri varsayılan değerlerinde bırakın.
   
    ![Yeni Seçme][createtopic3]
6. İletişim kutusunun altında yer alan **Oluştur**’a tıklayın.

## <a name="3-create-a-subscription-to-the-topic"></a>3. Konuya abonelik oluşturma

1. Portal kaynakları bölmesinde, 1. adımda oluşturduğunuz ad alanına, **Konular**’a ve ardından 2. adımda oluşturduğunuz konu adına tıklayın.
2. Genel bakış bölmesinin üst kısmında **Abonelik** seçeneğinin yanındaki + işaretine tıklayarak bu konuya bir abonelik ekleyin.

    ![Abonelik oluşturma][createtopic4]

3. Abonelik için bir ad girin. Diğer seçenekleri varsayılan değerlerinde bırakın.

## <a name="4-send-messages-to-the-topic"></a>4. Konuya ileti gönderme

Konuya ileti göndermek için Visual Studio kullanılarak bir C# konsol uygulaması yazın.

### <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

Visual Studio'yu başlatın ve yeni bir **Konsol Uygulaması (.NET Core)** projesi oluşturun.

### <a name="add-the-service-bus-nuget-package"></a>Service Bus NuGet paketi ekleme

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
2. **Gözat** sekmesine tıklayın, **[Microsoft.Azure.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/)** araması yapın ve ardından **Microsoft.Azure.ServiceBus** öğesini seçin. Yüklemeyi tamamlamak için **Yükle**'ye tıklayın, ardından bu iletişim kutusunu kapatın.
   
    ![NuGet paketi seçme][nuget-pkg]

### <a name="write-code-to-send-messages-to-the-topic"></a>Konuya ileti göndermek için kod yazma

1. Program.cs’de, ad alanı tanımının üst kısmına, sınıf bildiriminden önce aşağıdaki `using` deyimlerini ekleyin:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. `Program` sınıfında, aşağıdaki değişkenleri bildirin. `ServiceBusConnectionString` değişkenini, ad alanını oluştururken elde ettiğiniz bağlantı dizesine, `TopicName` değerini ise konuyu oluştururken kullandığınız ada ayarlayın:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string TopicName = "<your_topic_name>";
    static ITopicClient topicClient;
    ``` 

3. `Main()` öğesinin varsayılan içeriklerini aşağıdaki kod satırıyla değiştirin:

    ```csharp
    MainAsync().GetAwaiter().GetResult();
    ```
   
4. `Main()` öğesinden hemen sonra, gönderme iletilerini metoda çağıran aşağıdaki zaman uyumsuz `MainAsync()` metodunu ekleyin:

    ```csharp
    static async Task MainAsync()
    {
        const int numberOfMessages = 10;
        topicClient = new TopicClient(ServiceBusConnectionString, TopicName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after sending all the messages.");
        Console.WriteLine("======================================================");

        // Send messages.
        await SendMessagesAsync(numberOfMessages);

        Console.ReadKey();

        await topicClient.CloseAsync();
    }
    ```

5. `MainAsync()` metodundan hemen sonra, `numberOfMessagesToSend` tarafından belirlenen sayıda mesajı (geçerli değer: 10) gönderen aşağıdaki `SendMessagesAsync()` metodunu ekleyin:

    ```csharp
    static async Task SendMessagesAsync(int numberOfMessagesToSend)
    {
        try
        {
            for (var i = 0; i < numberOfMessagesToSend; i++)
            {
                // Create a new message to send to the topic.
                string messageBody = $"Message {i}";
                var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                // Write the body of the message to the console.
                Console.WriteLine($"Sending message: {messageBody}");

                // Send the message to the topic.
                await topicClient.SendAsync(message);
            }
        }
        catch (Exception exception)
        {
            Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
        }
    }
    ```
  
6. Gönderen Program.cs dosyanız aşağıdaki gibi görünmelidir.
   
    ```csharp
    namespace CoreSenderApp
    {
        using System;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.ServiceBus;

        class Program
        {
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string TopicName = "<your_topic_name>";
            static ITopicClient topicClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                const int numberOfMessages = 10;
                topicClient = new TopicClient(ServiceBusConnectionString, TopicName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after sending all the messages.");
                Console.WriteLine("======================================================");

                // Send messages.
                await SendMessagesAsync(numberOfMessages);

                Console.ReadKey();

                await topicClient.CloseAsync();
            }

            static async Task SendMessagesAsync(int numberOfMessagesToSend)
            {
                try
                {
                    for (var i = 0; i < numberOfMessagesToSend; i++)
                    {
                        // Create a new message to send to the topic
                        string messageBody = $"Message {i}";
                        var message = new Message(Encoding.UTF8.GetBytes(messageBody));

                        // Write the body of the message to the console
                        Console.WriteLine($"Sending message: {messageBody}");

                        // Send the message to the topic
                        await topicClient.SendAsync(message);
                    }
                }
                catch (Exception exception)
                {
                    Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
                }
            }
        }
    }
    ```

3. Programı çalıştırın ve Azure portalını denetleyin: Ad alanının **Genel Bakış** penceresinde konunuzun adına tıklayın. Konunun **Temel Bileşenler** penceresi gösterilir. Pencerenin alt kısmının yanında listelenen aboneliklerde, aboneliğin **İleti Sayısı** değerinin artık **10** olmasına dikkat edin. İletileri almadan gönderen uygulamasını her çalıştırdığınızda (sonraki bölümde açıklandığı gibi), bu değer 10 artar. Ayrıca, uygulamanın konuya ileti eklediği her durumda konunun geçerli boyutu, **Temel Bileşenler** penceresindeki **Geçerli** değerini artırır.
   
      ![İleti boyutu][topic-message]

## <a name="5-receive-messages-from-the-subscription"></a>5. Abonelikten ileti alma

Gönderdiğiniz iletileri almak için farklı bir .NET Core konsol uygulaması oluşturun ve önceki gönderen uygulamaya benzer şekilde, **Microsoft.Azure.ServiceBus** NuGet paketine başvuru ekleyin.

### <a name="write-code-to-receive-messages-from-the-subscription"></a>Abonelikten ileti almak için kod yazın

1. Program.cs’de, ad alanı tanımının üst kısmına, sınıf bildiriminden önce aşağıdaki `using` deyimlerini ekleyin:
   
    ```csharp
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

2. `Program` sınıfında, aşağıdaki değişkenleri bildirin. `ServiceBusConnectionString` değişkenini ad alanını oluştururken kullandığınız bağlantı dizesine, `TopicName` değişkenini konuyu oluştururken kullandığınız ada ve `SubscriptionName` değişkenini konu için abonelik oluştururken kullandığınız ada ayarlayın:
   
    ```csharp
    const string ServiceBusConnectionString = "<your_connection_string>";
    const string TopicName = "<your_topic_name>";
    const string SubscriptionName = "<your_subscription_name>";
    static ISubscriptionClient subscriptionClient;
    ```

3. `Main()` öğesinin varsayılan içeriklerini aşağıdaki kod satırıyla değiştirin:

    ```csharp
    MainAsync().GetAwaiter().GetResult();
    ```

4. `Main()` öğesinden hemen sonra, `RegisterOnMessageHandlerAndReceiveMessages()` metoduna çağrı yapan aşağıdaki zaman uyumsuz `MainAsync()` metodunu ekleyin:

    ```csharp
    static async Task MainAsync()
    {
        subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
        Console.WriteLine("======================================================");

        // Register subscription message handler and receive messages in a loop
        RegisterOnMessageHandlerAndReceiveMessages();

        Console.ReadKey();

        await subscriptionClient.CloseAsync();
    }
    ```

5. `MainAsync()` metodundan hemen sonra, ileti işleyiciyi kaydeden ve gönderen uygulama tarafından gönderilen iletileri alan aşağıdaki metodu ekleyin:

    ```csharp
    static void RegisterOnMessageHandlerAndReceiveMessages()
    {
        // Configure the message handler options in terms of exception handling, number of concurrent messages to deliver, etc.
        var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
        {
            // Maximum number of concurrent calls to the callback ProcessMessagesAsync(), set to 1 for simplicity.
            // Set it according to how many messages the application wants to process in parallel.
            MaxConcurrentCalls = 1,

            // Indicates whether the message pump should automatically complete the messages after returning from user callback.
            // False below indicates the complete operation is handled by the user callback as in ProcessMessagesAsync().
            AutoComplete = false
        };

        // Register the function that processes messages.
        subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    }
    ```    

6. Önceki metottan hemen sonra, alınan mesajları işlemek için aşağıdaki `ProcessMessagesAsync()` metodunu ekleyin:
 
    ```csharp
    static async Task ProcessMessagesAsync(Message message, CancellationToken token)
    {
        // Process the message.
        Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

        // Complete the message so that it is not received again.
        // This can be done only if the subscriptionClient is created in ReceiveMode.PeekLock mode (which is the default).
        await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);

        // Note: Use the cancellationToken passed as necessary to determine if the subscriptionClient has already been closed.
        // If subscriptionClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
        // to avoid unnecessary exceptions.
    }
    ```

7. Son olarak, olası özel durumları işlemek için aşağıdaki metodu ekleyin:
 
    ```csharp
    // Use this handler to examine the exceptions received on the message pump.
    static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
    {
        Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
        var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
        Console.WriteLine("Exception context for troubleshooting:");
        Console.WriteLine($"- Endpoint: {context.Endpoint}");
        Console.WriteLine($"- Entity Path: {context.EntityPath}");
        Console.WriteLine($"- Executing Action: {context.Action}");
        return Task.CompletedTask;
    }    
    ```    

8. Alıcı Program.cs dosyanız aşağıdaki gibi görünmelidir:
   
    ```csharp
    namespace CoreReceiverApp
    {
        using System;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.ServiceBus;

        class Program
        {
            const string ServiceBusConnectionString = "<your_connection_string>";
            const string TopicName = "<your_topic_name>";
            const string SubscriptionName = "<your_subscription_name>";
            static ISubscriptionClient subscriptionClient;

            static void Main(string[] args)
            {
                MainAsync().GetAwaiter().GetResult();
            }

            static async Task MainAsync()
            {
                subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);

                Console.WriteLine("======================================================");
                Console.WriteLine("Press ENTER key to exit after receiving all the messages.");
                Console.WriteLine("======================================================");

                // Register subscription message handler and receive messages in a loop.
                RegisterOnMessageHandlerAndReceiveMessages();

                Console.ReadKey();

                await subscriptionClient.CloseAsync();
            }

            static void RegisterOnMessageHandlerAndReceiveMessages()
            {
                // Configure the message handler options in terms of exception handling, number of concurrent messages to deliver, etc.
                var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
                {
                    // Maximum number of concurrent calls to the callback ProcessMessagesAsync(), set to 1 for simplicity.
                    // Set it according to how many messages the application wants to process in parallel.
                    MaxConcurrentCalls = 1,

                    // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
                    // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
                    AutoComplete = false
                };

                // Register the function that processes messages.
                subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
            }

            static async Task ProcessMessagesAsync(Message message, CancellationToken token)
            {
                // Process the message.
                Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

                // Complete the message so that it is not received again.
                // This can be done only if the subscriptionClient is created in ReceiveMode.PeekLock mode (which is the default).
                await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);

                // Note: Use the cancellationToken passed as necessary to determine if the subscriptionClient has already been closed.
                // If subscriptionClient has already been closed, you can choose to not call CompleteAsync() or AbandonAsync() etc.
                // to avoid unnecessary exceptions.
            }

            static Task ExceptionReceivedHandler(ExceptionReceivedEventArgs exceptionReceivedEventArgs)
            {
                Console.WriteLine($"Message handler encountered an exception {exceptionReceivedEventArgs.Exception}.");
                var context = exceptionReceivedEventArgs.ExceptionReceivedContext;
                Console.WriteLine("Exception context for troubleshooting:");
                Console.WriteLine($"- Endpoint: {context.Endpoint}");
                Console.WriteLine($"- Entity Path: {context.EntityPath}");
                Console.WriteLine($"- Executing Action: {context.Action}");
                return Task.CompletedTask;
            }
        }
    }
    ```
9. Programı çalıştırın ve portalı tekrar denetleyin. **İleti Sayısı** ve **Geçerli** değerlerinin artık **0** olduğuna dikkat edin.
   
    ![Konu uzunluğu][topic-message-receive]

Tebrikler! .NET Standard kitaplığını kullanarak bir konu ve abonelik oluşturdunuz, 10 ileti gönderdiniz ve bu iletileri aldınız.

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
