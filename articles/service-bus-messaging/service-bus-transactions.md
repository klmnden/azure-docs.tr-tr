---
title: Azure hizmet veri yolundaki işlem genel bakış | Microsoft Docs
description: Azure Service Bus atomik işlemleri ve aracılığıyla gönderme genel bakış
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 64449247-1026-44ba-b15a-9610f9385ed8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: sethm
ms.openlocfilehash: d2e3fc7c59e0b57e77d2239ff73368f96426ef39
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
ms.locfileid: "29558950"
---
# <a name="overview-of-service-bus-transaction-processing"></a>Hizmet veri yolu işlem genel bakış

Bu makalede, Microsoft Azure Service Bus işlem özelliklerini açıklar. Tartışma çoğunu tarafından gösterilen [hizmet veri yolu örnek ile atomik işlemleri](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions). Bu makalede işlem genel bir bakış için sınırlıdır ve *aracılığıyla gönderme* Service Bus atomik işlemleri örnek kapsamında daha geniş ve daha karmaşık olsa da, özelliği.

## <a name="transactions-in-service-bus"></a>Hizmet veri yolu işlemleri

A [ *işlem* ](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) iki veya daha fazla işlem içine gruplandıran bir *yürütme kapsam*. Doğası gereği, böyle bir işlem işlemlerinin belirli bir gruba ait tüm işlemlerin başarılı veya başarısız ortaklaşa emin olmalısınız. Bu bakımdan genellikle olarak adlandırılır tek bir birim olarak hareketleri hareket *kararlılık*. 

Hizmet veri yolu işlem ileti Aracısı olduğunu ve kendi ileti depoları karşı tüm iç işlemleri için işlem bütünlüğünü sağlar. Aktarımlar iletileri taşıma gibi iletilerinin Service Bus içinde bir [sahipsiz sırayı](service-bus-dead-letter-queues.md) veya [otomatik iletme](service-bus-auto-forwarding.md) varlıklar arasındaki iletileri işlem şunlardır. Bu nedenle, hizmet veri yolu ileti kabul ediyorsa, zaten olduğundan depolanan ve bir sıra numarasıyla etiketli. Daha sonra Service Bus içinde ileti aktarımlar Eşgüdümlü işlemleri arasında varlıklardır ve hiçbiri kaybına neden (başarılı kaynak ve hedef başarısız) veya çoğaltma için (kaynak başarısız olur ve hedef başarılı) iletisi.

Hizmet veri yolu bir işlem tek bir Mesajlaşma varlık (kuyruk, konu, abonelik) kapsamındaki karşı gruplandırma işlemleri destekler. Örneğin, bir işlem kapsamı içinde bir kuyruktan çeşitli iletiler gönderebilir ve işlem başarıyla tamamlandığında iletileri yalnızca sıra günlüğe kaydedilmiş olacaktır.

## <a name="operations-within-a-transaction-scope"></a>Bir işlem kapsamı içinde işlemleri

Bir işlem kapsamı içinde gerçekleştirilen işlem aşağıdaki gibidir:

* **[QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient), [MessageSender](/dotnet/api/microsoft.azure.servicebus.core.messagesender), [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient)**: Send, SendAsync, SendBatch, SendBatchAsync 
* **[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: tam, CompleteAsync, durdurma, AbandonAsync, sahipsiz, DeadletterAsync, erteleme, DeferAsync, RenewLock, RenewLockAsync 

Alma işlemleri olmayan dahil, uygulamayı kullanarak iletileri alması varsayıldığından [ReceiveMode.PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) bazı içinde mod döngü almak veya ile bir [Onmessageoptions](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) geri ve yalnızca ardından iletiyi işlemek için bir işlem kapsamı açar.

(Tam abandon, teslim edilemeyen erteleneceği,) ileti değerlendirme kapsamında ve bağımlı üzerinde genel işlem sonucunu sonra oluşur.

## <a name="transfers-and-send-via"></a>Aktarımları ve "aracılığıyla Gönder"

Bir kuyruk verilerini bir işlemci ve ardından üzere başka bir sıra işlem handover etkinleştirmek için Service Bus destekler *aktarımları*. Bir aktarım işlemi bir gönderici önce bir ileti gönderir. bir *aktarımı sıra*, ve aktarım sıranın ileti aynı güçlü aktarım uygulamasını kullanarak hedeflenen hedef sırasına hemen taşır., otomatik iletme Yetenek kullanır. İleti hiçbir zaman bu aktarımı sıranın Tüketiciler için görünür olacağı şekilde transfer sıranın günlüğüne taahhüt eder.

Aktarma sırası gönderenin giriş iletileri kaynağı olduğunda bu işlem özelliği gücünü belirgin olur. Diğer bir deyişle, hizmet veri yolu ileti hedef sırasına aktarımı sıranın "ile" tam gerçekleştirilirken aktarabilirsiniz (veya erteleneceği, ya da teslim edilemeyen) giriş ileti, tüm tek bir atomik işlemle işlemi. 

### <a name="see-it-in-code"></a>Kodda bakın

Bu tür aktarımları ayarlamak için hedef sıra aktarımı kuyruk üzerinden hedefleyen bir ileti gönderen oluşturun. Bu aynı sıraya alınan iletileri çeken bir alıcı da vardır. Örneğin:

```csharp
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Aşağıdaki örnekte olduğu gibi bu öğelerden sonra basit bir işlem kullanır:

```csharp
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Sonraki adımlar

Service Bus kuyruklarını hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Bus kuyruklarını kullanma](service-bus-dotnet-get-started-with-queues.md)
* [Hizmet veri yolu varlıklarını otomatik iletme ile zincirleme](service-bus-auto-forwarding.md)
* [Otomatik iletme örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Hizmet veri yolu örnek ile atomik işlemleri](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Azure kuyruklar ve hizmet veri yolu sıraları karşılaştırılan](service-bus-azure-and-service-bus-queues-compared-contrasted.md)


