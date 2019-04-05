---
title: Hızlı Başlangıç - Azure Service Bus kuyruğuna oluşturmak için PowerShell kullanma | Microsoft Docs
description: Bu hızlı başlangıçta, Service Bus kuyruğu oluşturma için Azure PowerShell kullanmayı öğrenin. Ardından, kuyruktan ileti alma ve ileti göndermek için örnek bir uygulama kullanın.
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.devlang: dotnet
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/12/2019
ms.author: spelluru
ms.openlocfilehash: ef510ca88f1b305125c7840932641c8a2359d8c9
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045250"
---
# <a name="quickstart-use-azure-powershell-to-create-a-service-bus-queue"></a>Hızlı Başlangıç: Azure PowerShell kullanarak bir Service Bus kuyruğu oluşturma
Microsoft Azure Service Bus, güvenli mesajlaşma ve son derece yüksek güvenilirlik sağlayan bir kurumsal tümleştirme ileti aracısıdır. Tipik bir Service Bus senaryosunda çoğunlukla iki veya daha çok uygulama, hizmet veya işlem birbirinden ayrılır ve durum veya veri değişiklikleri aktarılır. Bu tür senaryolar başka bir uygulama veya hizmetlerde birden çok toplu işin zamanlanmasını veya sipariş karşılama işleminin tetiklenmesini içerebilir. Örneğin, bir perakende şirketi satış noktası verilerini yenileme ve stok güncelleştirmeleri için bir arka ofise veya bölgesel dağıtım merkezine gönderebilir. Bu senaryoda, istemci uygulaması Service Bus kuyruğuna iletiler gönderir ve o kuyruktan ileti alır.

![kuyruk](./media/service-bus-quickstart-powershell/quick-start-queue.png)

Bu hızlı başlangıçta, mesajlaşma ad alanı ve o ad alanı içinde bir kuyruk oluşturmak ve söz konusu ad alanında yetkilendirme kimlik bilgilerini almak için PowerShell kullanarak bir Service Bus kuyruğuna nasıl ileti gönderileceği ve Service Bus kuyruğundan nasıl ileti alınacağı açıklanmaktadır. Daha sonra yordam, [.NET Standard kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) kullanılarak bu kuyruktan nasıl ileti gönderilip alınacağını gösterir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][] oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için şunları yüklediğinizden emin olun:

- [Visual Studio 2017 Güncelleştirme 3 (sürüm 15.3, 26730.01)](https://www.visualstudio.com/vs) veya sonraki sürümler.
- [NET Core SDK](https://www.microsoft.com/net/download/windows), sürüm 2.0 veya sonraki sürümler.

Bu hızlı başlangıç için Azure PowerShell'in en yeni sürümünü kullanmanız gerekir. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell'i Yükleme ve Yapılandırma][].

## <a name="log-in-to-azure"></a>Azure'da oturum açma

1. İlk olarak, henüz yapmadıysanız Service Bus PowerShell modülünü yükleyin:

   ```azurepowershell-interactive
   Install-Module Az.ServiceBus
   ```

2. Azure’da oturum açmak için aşağıdaki komutu çalıştırın:

   ```azurepowershell-interactive
   Login-AzAccount
   ```

3. Aşağıdaki komutları göndererek geçerli abonelik bağlamını ayarlayın veya şu anda etkin olan aboneliği görüntüleyin:

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionName "MyAzureSubName" 
   Get-AzContext
   ```

## <a name="provision-resources"></a>Kaynak sağlama

PowerShell komut isteminde, Service Bus kaynakları sağlamak için aşağıdaki komutları verin. Tüm yer tutucuları uygun değerlerle değiştirdiğinizden emin olun:

```azurepowershell-interactive
# Create a resource group 
New-AzResourceGroup –Name my-resourcegroup –Location eastus

# Create a Messaging namespace
New-AzServiceBusNamespace -ResourceGroupName my-resourcegroup -NamespaceName namespace-name -Location eastus

# Create a queue 
New-AzServiceBusQueue -ResourceGroupName my-resourcegroup -NamespaceName namespace-name -Name queue-name -EnablePartitioning $False

# Get primary connection string (required in next step)
Get-AzServiceBusKey -ResourceGroupName my-resourcegroup -Namespace namespace-name -Name RootManageSharedAccessKey
```

`Get-AzServiceBusKey` cmdlet’i çalıştırıldıktan sonra, bağlantı dizesini ve seçtiğiniz kuyruk adını kopyalayıp Not Defteri gibi geçici bir yere yapıştırın. Bu sonraki adımda gerekecektir.

## <a name="send-and-receive-messages"></a>İleti alma ve gönderme

Ad alanı ve kuyruğun oluşturulmasının ve gerekli kimlik bilgilerine sahip olmanızın ardından ileti gönderip almaya hazırsınız demektir. [Bu GitHub örnek klasöründeki](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/BasicSendReceiveQuickStart) kodu inceleyebilirsiniz.

Kodu çalıştırmak için aşağıdakileri yapın:

1. [Service Bus GitHub deposunu](https://github.com/Azure/azure-service-bus/) aşağıdaki komutu çalıştırarak kopyalayın:

   ```shell
   git clone https://github.com/Azure/azure-service-bus.git
   ```

3. Örnek `azure-service-bus\samples\DotNet\GettingStarted\BasicSendReceiveQuickStart\BasicSendReceiveQuickStart` klasörüne gidin.

4. Henüz yapmadıysanız, aşağıdaki PowerShell cmdlet’ini kullanarak bağlantı dizesini alın. Değiştirdiğinizden emin olun `my-resourcegroup` ve `namespace-name` , belirli değerlere sahip: 

   ```azurepowershell-interactive
   Get-AzServiceBusKey -ResourceGroupName my-resourcegroup -Namespace namespace-name -Name RootManageSharedAccessKey
   ```

5. PowerShell isteminde aşağıdaki komutu yazın:

   ```shell
   dotnet build
   ```

6. `bin\Debug\netcoreapp2.0` klasörüne gidin.

7. Programı çalıştırmak için aşağıdaki komutu yazın. `myConnectionString` yerine daha önce aldığınız değeri ve `myQueueName` yerine oluşturduğunuz kuyruğun adını koymayı unutmayın:

   ```shell
   dotnet BasicSendReceiveQuickStart.dll -ConnectionString "myConnectionString" -QueueName "myQueueName"
   ``` 

8. Kuyruğa 10 ileti gönderildiğini ve ardından bunların kuyruktan alındığını gözlemleyin:

   ![program çıktısı](./media/service-bus-quickstart-powershell/dotnet.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu, ad alanını ve tüm ilgili kaynakları kaldırmak için aşağıdaki komutu çalıştırın:

```powershell-interactive
Remove-AzResourceGroup -Name my-resourcegroup
```

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Bu bölümde örnek kodun işlevleri hakkında daha fazla ayrıntı bulunmaktadır. 

### <a name="get-connection-string-and-queue"></a>Bağlantı dizesini ve kuyruğu alma

Bağlantı dizesi ve kuyruk adı, `Main()` yöntemine komut satırı bağımsız değişkenleri olarak geçirilir. `Main()` Bu değerleri tutmak için iki dize değişkenleri bildirir:

```csharp
static void Main(string[] args)
{
    string ServiceBusConnectionString = "";
    string QueueName = "";

    for (int i = 0; i < args.Length; i++)
    {
        var p = new Program();
        if (args[i] == "-ConnectionString")
        {
            Console.WriteLine($"ConnectionString: {args[i+1]}");
            ServiceBusConnectionString = args[i + 1]; 
        }
        else if(args[i] == "-QueueName")
        {
            Console.WriteLine($"QueueName: {args[i+1]}");
            QueueName = args[i + 1];
        }                
    }

    if (ServiceBusConnectionString != "" && QueueName != "")
        MainAsync(ServiceBusConnectionString, QueueName).GetAwaiter().GetResult();
    else
    {
        Console.WriteLine("Specify -Connectionstring and -QueueName to execute the example.");
        Console.ReadKey();
    }                            
}
```
 
`Main()` yöntemi daha sonra zaman uyumsuz ileti döngüsünü (`MainAsync()`) başlatır.

### <a name="message-loop"></a>İleti döngüsü

MainAsync() yöntemi, komut satırı bağımsız değişkenleri ile bir kuyruk istemcisi oluşturur, `RegisterOnMessageHandlerAndReceiveMessages()` adlı bir ileti alma işleyicisi çağırır ve ileti kümesini gönderir:

```csharp
static async Task MainAsync(string ServiceBusConnectionString, string QueueName)
{
    const int numberOfMessages = 10;
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

    Console.WriteLine("======================================================");
    Console.WriteLine("Press any key to exit after receiving all the messages.");
    Console.WriteLine("======================================================");

    // Register QueueClient's MessageHandler and receive messages in a loop
    RegisterOnMessageHandlerAndReceiveMessages();

    // Send Messages
    await SendMessagesAsync(numberOfMessages);

    Console.ReadKey();

    await queueClient.CloseAsync();
}
```

`RegisterOnMessageHandlerAndReceiveMessages()` yöntemi, basit bir şekilde birkaç ileti işleyicisi seçeneği ayarlar, daha sonra kuyruk istemcisinin `RegisterMessageHandler()` yöntemini çağırarak alma işlemini başlatır:

```csharp
static void RegisterOnMessageHandlerAndReceiveMessages()
{
    // Configure the MessageHandler Options in terms of exception handling, number of concurrent messages to deliver etc.
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        // Maximum number of Concurrent calls to the callback `ProcessMessagesAsync`, set to 1 for simplicity.
        // Set it according to how many messages the application wants to process in parallel.
        MaxConcurrentCalls = 1,

        // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
        // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
        AutoComplete = false
    };

    // Register the function that will process messages
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
} 
```

### <a name="send-messages"></a>İleti gönderme

İleti oluşturma ve gönderme işlemleri, `SendMessagesAsync()` yönteminde gerçekleşir:

```csharp
static async Task SendMessagesAsync(int numberOfMessagesToSend)
{
    try
    {
        for (var i = 0; i < numberOfMessagesToSend; i++)
        {
            // Create a new message to send to the queue
            string messageBody = $"Message {i}";
            var message = new Message(Encoding.UTF8.GetBytes(messageBody));

            // Write the body of the message to the console
            Console.WriteLine($"Sending message: {messageBody}");

            // Send the message to the queue
            await queueClient.SendAsync(message);
        }
    }
    catch (Exception exception)
    {
        Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
    }
}
```

### <a name="process-messages"></a>İletileri işleme

`ProcessMessagesAsync()` yöntemi, iletilerin alınmasını onaylar, işler ve tamamlar:

```csharp
static async Task ProcessMessagesAsync(Message message, CancellationToken token)
{
    // Process the message
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

    // Complete the message so that it is not received again.
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Service Bus alan adı ve bir kuyruktan ileti gönderip almak için gereken diğer kaynakları oluşturdunuz. İleti göndermek ve almak için kod yazma hakkında daha fazla bilgi edinmek için, aşağıdaki Service Bus öğreticisine geçin:

> [!div class="nextstepaction"]
> [Azure PowerShell kullanarak Envanter güncelleştirme](./service-bus-tutorial-topics-subscriptions-powershell.md)

[kredi kazanın]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Azure PowerShell'i Yükleme ve Yapılandırma]: /powershell/azure/install-Az-ps
