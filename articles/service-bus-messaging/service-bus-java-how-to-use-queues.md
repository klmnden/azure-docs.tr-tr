---
title: Java ile Azure Service Bus kuyruklarını kullanma | Microsoft Docs
description: Azure'da Service Bus kuyruklarını kullanmayı öğrenin. Java dilinde yazılan kod örneklerini.
services: service-bus-messaging
documentationcenter: java
author: spelluru
manager: timlt
ms.assetid: f701439c-553e-402c-94a7-64400f997d59
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: spelluru
ms.openlocfilehash: e4099c8228e9434276242a3c49ebcb4fc2e995b2
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696474"
---
# <a name="how-to-use-service-bus-queues-with-java"></a>Java ile Service Bus kuyruklarını kullanma
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu makalede, Service Bus kuyruklarının nasıl kullanılacağı açıklanır. Java ve kullanım örnekleri yazılır [Java için Azure SDK'sı][Azure SDK for Java]. Senaryoları ele alınmaktadır **kuyruk oluşturma**, **ileti gönderme ve alma**, ve **sıraları silme**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırma
Yüklediğinizden emin olun [Java için Azure SDK'sı] [ Azure SDK for Java] Bu örnek derlemeden önce. Eclipse kullanıyorsanız yükleyebileceğiniz [Eclipse için Azure Araç Seti] [ Azure Toolkit for Eclipse] , Java için Azure SDK'sı içerir. Daha sonra ekleyebilirsiniz **Microsoft Java için Azure kitaplıkları** projenize:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Aşağıdaki `import` Java dosyasının en üstüne ifadeleri:

```java
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
Service Bus kuyruklarına yönelik yönetim işlemlerini aracılığıyla gerçekleştirilebilir **ServiceBusContract** sınıfı. A **ServiceBusContract** nesnesi, yönetim izinleriyle SAS belirteci kapsülleyen uygun bir yapılandırma ile oluşturulur ve **ServiceBusContract** tek yakınlardaki bir sınıftır Azure ile iletişim.

**ServiceBusService** sınıfı oluşturmak, listeleme ve kuyruklarını silmek için yöntemler sağlar. Gösterir aşağıdaki örnekte nasıl bir **ServiceBusService** nesne adında bir kuyruk oluşturmak için kullanılabilir `TestQueue`, adlı bir ad alanı ile `HowToSample`:

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Temel yöntem vardır `QueueInfo` ayarlanmasına kuyruğun özelliklerini izin ver (örneğin: kuyruğa gönderilen iletilere uygulanacak varsayılan yaşam süresi (TTL) değerini ayarlamak için). Aşağıdaki örnekte adlı bir kuyruğun nasıl oluşturulacağını gösterir `TestQueue` en fazla 5 GB'lık:

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Kullanabileceğiniz Not `listQueues` metodunda **ServiceBusContract** nesneleri belirtilen adda bir kuyruk, bir hizmet ad alanı içinde zaten mevcut olup olmadığını denetleyin.

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Bir Service Bus kuyruğuna bir ileti göndermek için uygulamanızı alır bir **ServiceBusContract** nesne. Aşağıdaki kod bir ileti göndermek nasıl gösterir `TestQueue` daha önce oluşturulan kuyruk `HowToSample` ad alanı:

```java
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

İletileri gönderilen ve alınan Service Bus kuyrukları örnekleri olan [BrokeredMessage] [ BrokeredMessage] sınıfı. [BrokeredMessage] [ BrokeredMessage] nesneleri olan bir standart özellikler kümesi (gibi [etiket](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) ve [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), özel tutmak için kullanılan bir sözlüğü uygulamaya özgü özellikler ve rastgele uygulama verileri gövdesi. Uygulamanın herhangi bir seri hale getirilebilir nesnesi oluşturucusuna geçirerek ileti gövdesini ayarlayabilirsiniz [BrokeredMessage][BrokeredMessage], ve uygun seri hale getirici sonra nesneyi serileştirmek için kullanılır. Alternatif olarak, sağlayan bir **java. GÇ. InputStream** nesne.

Aşağıdaki örnek nasıl beş test iletisi göndereceğinizi gösterir `TestQueue` **MessageSender** önceki kod parçacığında elde ediyoruz:

```java
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="receive-messages-from-a-queue"></a>Bir kuyruktan ileti alma
Bir kuyruktan ileti almak için birincil yolu bir **ServiceBusContract** nesne. Alınan iletiler, iki farklı modda çalışabilir: **ReceiveAndDelete** ve **PeekLock**.

Kullanırken **ReceiveAndDelete** modunda almak bir tek işlem - diğer bir deyişle, Service Bus kuyruk iletiye yönelik Okuma isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. **ReceiveAndDelete** (varsayılan mod budur) mod en basit modeldir ve içinde bir uygulama tolere edebilen bir arıza olması durumunda bir iletiyi işlememeye izin senaryolarda en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın.
Service Bus iletiyi kullanılıyor olarak işaretleyeceğinden, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında ardından onu çökmenin öncesinde kullanılan iletiyi atlamış olur.

İçinde **PeekLock** modu, alma, atlanan iletilere veremeyen uygulamaları desteklemenin mümkün hale getiren bir iki aşamalıdır. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya güvenilir bir şekilde işlemek üzere depolar sonra) çağırarak alma işleminin ikinci aşamasını tamamlar **Sil** alınan iletide. Service Bus gördüğünde **Sil** çağrı, bu iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırın.

Aşağıdaki örnek nasıl ileti alındı ve işlenen kullanarak gösterir **PeekLock** modu (varsayılan mod değil). Aşağıdaki örnek bir sonsuz döngü yapar ve içine geldikçe iletileri işleyen bizim `TestQueue`:

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                 service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi işlemek için herhangi bir nedenle silemiyor sonra çağırabilirsiniz **unlockMessage** yöntemi alınan iletide (yerine **deleteMessage** yöntemi). Bu işlem Service Bus hizmetinin kuyruktaki iletinin kilidini açmasına ve iletiyi aynı veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale getirmesine neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı yoktur ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açar ve alınabilmesini kilit zaman aşımı dolmadan tekrar kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, **deleteMessage** isteği bildirilmeden, sonra yeniden başlatıldığında ileti uygulamaya yeniden teslim. Buna genellikle denir *en az bir kez işleme*; diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu genellikle kullanılmasıdır **getMessageId** yöntemi iletinin teslim denemeleri arasında sabit kalır.

## <a name="next-steps"></a>Sonraki Adımlar
Service Bus kuyruklarına ilişkin temel bilgileri öğrendiğinize göre artık bkz [kuyruklar, konular ve abonelikler] [ Queues, topics, and subscriptions] daha fazla bilgi için.

Daha fazla bilgi için bkz. [Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/).

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
