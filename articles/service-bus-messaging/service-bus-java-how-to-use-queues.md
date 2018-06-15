---
title: Java ile Azure Service Bus kuyruklarını kullanma | Microsoft Docs
description: Azure'da Service Bus kuyruklarını kullanmayı öğrenin. Java dilinde yazılan kod örnekleri.
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
ms.assetid: f701439c-553e-402c-94a7-64400f997d59
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 170f431525ffdc93a01fc085e48e69c3a774968e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23868434"
---
# <a name="how-to-use-service-bus-queues-with-java"></a>Java ile Service Bus kuyruklarını kullanma
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu makalede, Service Bus kuyruklarının nasıl kullanılacağı açıklanır. Java ve kullanım örnekleri yazılır [Java için Azure SDK][Azure SDK for Java]. Kapsamdaki senaryolar dahil **sıra oluşturma**, **ileti gönderme ve alma**, ve **sıraları silme**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırın
Yüklediğinizden emin olun [Java için Azure SDK] [ Azure SDK for Java] önce bu örnek oluşturma. Eclipse kullanıyorsanız, yükleyebileceğiniz [Eclipse için Azure Araç Seti] [ Azure Toolkit for Eclipse] Java için Azure SDK'sı içerir. Daha sonra ekleyebilirsiniz **Java için Microsoft Azure kitaplıkları** projenize:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Aşağıdakileri ekleyin `import` deyimlerini Java dosyanın en üstüne ekleyin:

```java
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
Service Bus kuyruklarına yönelik yönetim işlemlerini aracılığıyla gerçekleştirilebilecek **ServiceBusContract** sınıfı. A **ServiceBusContract** nesnesi, yönetmek için gerekli izinlere sahip SAS belirteci yalıtır uygun bir yapılandırma ile oluşturulur ve **ServiceBusContract** sınıfı Azure ile iletişimin tek noktasıdır.

**ServiceBusService** sınıfı oluşturmak, numaralandırır ve sırayı silmek için yöntemler sağlar. Gösterir aşağıdaki örnekte nasıl bir **ServiceBusService** nesne adında bir kuyruk oluşturmak için kullanılabilir `TestQueue`, adlı bir ad alanı ile `HowToSample`:

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

Yöntemleri vardır `QueueInfo` ayarlanmasına kuyruğun özelliklerini izin ver (örneğin: kuyruğa gönderilen iletilere uygulanacak varsayılan yaşam süresi (TTL) değerini ayarlamak için). Aşağıdaki örnek adlı bir kuyruk oluşturulacağını gösterir `TestQueue` maksimum 5 GB boyuta sahip:

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Kullanabileceğiniz Not `listQueues` yöntemi **ServiceBusContract** nesneleri belirtilen ada sahip bir sıra hizmet ad alanı içinde olup olmadığını denetle.

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Service Bus kuyruğuna bir ileti göndermek için uygulamanızı alacağı bir **ServiceBusContract** nesnesi. Aşağıdaki kod bir ileti göndermek gösterilmektedir `TestQueue` daha önce oluşturulan sıra `HowToSample` ad alanı:

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

Gönderilen iletileri ve Service Bus alınan örnekleridir [BrokeredMessage] [ BrokeredMessage] sınıfı. [BrokeredMessage] [ BrokeredMessage] nesneleri olan bir standart özellikler kümesi (gibi [etiket](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) ve [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), özel tutmak için kullanılan bir sözlük uygulamaya özgü özellikler ve rastgele uygulama verileri gövdesi. Bir uygulama herhangi bir seri hale getirilebilir nesne oluşturucusuna geçirerek ileti gövdesini ayarlayabilir [BrokeredMessage][BrokeredMessage], ve uygun seri hale getirici sonra nesneyi serileştirmek için kullanılır. Alternatif olarak, sağlayabilir bir **java. G/Ç. InputStream** nesnesi.

Aşağıdaki örnekte nasıl beş test iletisi göndereceğinizi gösterir `TestQueue` **MessageSender** biz önceki kod parçacığında elde:

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

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma
Kuyruktan iletileri almak için birincil yolu bir **ServiceBusContract** nesnesi. Alınan iletiler, iki farklı modda çalışabilir: **ReceiveAndDelete** ve **PeekLock**.

Kullanırken **ReceiveAndDelete** modu, alma bir tek işlemi - diğer bir deyişle, hizmet veri yolu bir kuyrukta İletiye yönelik Okuma isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. **ReceiveAndDelete** (varsayılan mod budur) modunda en basit modeldir ve senaryoları bir uygulama içinde tolerans bir arıza olması durumunda bir ileti işlenirken değil en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın.
Service Bus iletiyi kullanılıyor olarak işaretlenmiş nedeniyle uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, sonra da çökmenin öncesinde kullanılan iletiyi atlamış olur.

İçinde **PeekLock** modu, alma, iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi olur. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya sonra işlemek için depoladıktan sonra), çağırarak alma işleminin ikinci aşamasını tamamlar **silmek** alınan iletide. Hizmet veri yolu gördüğünde **silmek** çağrısı iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırın.

Aşağıdaki örnek nasıl ileti aldı ve işlenen kullanarak gösterir **PeekLock** modu (varsayılan mod değil). Aşağıdaki örnek sonsuz bir döngüde yapar ve içine geldikçe iletileri işleyen bizim `TestQueue`:

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
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi herhangi bir nedenden dolayı işleyemedi sonra işleyememesi **unlockMessage** alınan iletide yöntemi (yerine **deleteMessage** yöntemi). Bu işlem Service Bus hizmetinin kuyruktaki iletinin kilidini açmasına ve iletiyi aynı veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale getirmesine neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı vardır ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açar ve tekrar alınabilmesini sağlar kilit zaman aşımı dolmadan.

Uygulama iletiyi ancak önce çökmesi durumunda, **deleteMessage** isteği bildirilmeden, sonra yeniden başlatıldığında ileti uygulamaya tekrar teslim. Bu genellikle adlandırılır *en az bir kez işleme*; diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu genellikle kullanılarak elde edilen **getMessageId** iletinin teslimat denemelerinde yöntemi.

## <a name="next-steps"></a>Sonraki Adımlar
Service Bus kuyruklarına öğrendiğinize göre bkz: [kuyruklar, konu başlıkları ve abonelikler] [ Queues, topics, and subscriptions] daha fazla bilgi için.

Daha fazla bilgi için bkz. [Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/).

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
