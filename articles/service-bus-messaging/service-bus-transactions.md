---
title: Azure Service Bus işlem genel bakış | Microsoft Docs
description: Azure Service Bus atomik işlemler ve aracılığıyla Gönder'na genel bakış
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 64449247-1026-44ba-b15a-9610f9385ed8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: spelluru
ms.openlocfilehash: bc0e0bfb8d4dc8637c191785a98f5d15e086cbf5
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696440"
---
# <a name="overview-of-service-bus-transaction-processing"></a>Service Bus hareket işleme genel bakış

Bu makalede, Microsoft Azure Service Bus işlem yeteneklerini açıklar. Tartışma çoğunu gösterilmiştir [atomik işlemler ile hizmet veri yolu örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions). İşlem genel bir bakış için bu makalede sınırlıdır ve *aracılığıyla gönderme* Service Bus atomik işlemler örnek kapsamda daha geniş ve daha karmaşık olmakla birlikte, özelliği.

## <a name="transactions-in-service-bus"></a>Hizmet veri yolu işlemleri

A [ *işlem* ](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) iki veya daha fazla işlem içine gruplandıran bir *yürütme kapsam*. Doğası gereği bu tür bir işlem işlemlerinin belirli bir gruba ait tüm işlemleri başarılı veya başarısız ortaklaşa emin olmanız gerekir. Bu bakımdan, genellikle olarak adlandırılan tek bir birim olarak işlem hareket *kararlılık*. 

Service Bus, bir işlem iletisi aracısıdır ve kendi ileti depoları tüm iç işlemler için işlem tutarlılığı sağlar. İletileri taşıma gibi iletileri Service Bus içinde tüm aktarımları bir [eski ileti sırası](service-bus-dead-letter-queues.md) veya [otomatik iletme](service-bus-auto-forwarding.md) varlıklar arasında iletileri işlem olan. Bu nedenle, Service Bus iletiyi kabul ederse, zaten olduğundan depolanır ve bir sıra numarası ile etiketlenmiş. Daha sonra Service Bus içinde tüm ileti aktarımları varlıklar arasında Eşgüdümlü işlemlerdir ve bunların hiçbiri kaybına neden (kaynak başarılı ve başarısız hedef) veya çoğaltma (başarısız kaynak ve hedef başarılı) ileti.

Service Bus, bir hareketin kapsamı içindeki işlemlerin (kuyruk, konu başlığı, abonelik gibi) tek bir mesajlaşma varlığına göre gruplanmasını destekler. Örneğin, bir kuyruktan bir işlem kapsamı içindeki çeşitli iletiler gönderebilir ve işlem başarıyla tamamlandığında iletileri yalnızca sıranın günlüğüne kaydedilmiş olacaktır.

## <a name="operations-within-a-transaction-scope"></a>İşlem kapsamı içindeki işlemler

İşlem kapsamı içinde gerçekleştirilen işlem aşağıdaki gibidir:

* **[QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient), [MessageSender](/dotnet/api/microsoft.azure.servicebus.core.messagesender), [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient)**: SendBatchAsync SendAsync, SendBatch, Gönder 
* **[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: tam CompleteAsync gönderilemeyen, AbandonAsync teslim edilemeyen iletiler, DeadletterAsync, erteleme, DeferAsync, RenewLock RenewLockAsync 

Alma işlemleri dahil değildir, uygulamayı kullanarak iletileri alması varsayıldığından [ReceiveMode.PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modu, bazı iç döngü almak veya bir [Onmessageoptions](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) geri arama ve ancak bundan sonra iletiyi işlemek için bir işlem kapsamı açılır.

Değerlendirme iletisinin (tam gönderilemeyen, eski ileti erteleme,) üzerinde işlemin sonucunu genel kapsamdaki ve bağımlı sonra gerçekleşir.

## <a name="transfers-and-send-via"></a>Aktarımları ve "aracılığıyla Gönder"

Bir işlemci ve ardından için başka bir kuyruğa veri kuyruğu'ndan işlem devreden MultiPath etkinleştirmek için Service Bus destekler *aktarımları*. Bir aktarım işlemi, bir gönderici önce bir ileti gönderir. bir *aktarımı kuyruk*, ve aktarım sıranın aynı güçlü aktarım uygulamasını kullanarak istenen hedef sırasına hemen iletiyi taşır, otomatik iletme özelliği kullanır. İleti aktarımı sıranın günlük aktarımı sıranın Tüketiciler için görünür duruma gelir emin bir şekilde hiçbir zaman hassastır.

Aktarma sırası gönderenin giriş iletileri kaynağı olduğunda işlem bu özelliğin gücünü belirgin olur. Diğer bir deyişle, Service Bus ileti hedef sırasına aktarma sırası "ile" tam gerçekleştirilirken aktarabilirsiniz (veya erteleme, veya teslim edilemeyen) tek bir işlemde atomik giriş yapılacak işlem. 

### <a name="see-it-in-code"></a>Kodda bakın

Bu tür aktarımı ayarlamak için hedef sırası aktarımı kuyruk üzerinden hedefleyen bir iletiyi gönderenin oluşturun. Ayrıca, aynı kuyruktan iletileri çeken bir alıcı vardır. Örneğin:

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

Service Bus kuyrukları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Bus kuyruklarını kullanma](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus varlıklarına otomatik yönlendirme özellikli zincir oluşturma](service-bus-auto-forwarding.md)
* [Otomatik iletme örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Hizmet veri yolu örnek ile atomik işlemler](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Azure kuyrukları ve Service Bus kuyrukları karşılaştırma](service-bus-azure-and-service-bus-queues-compared-contrasted.md)


