---
title: Azure Service Bus işlem genel bakış | Microsoft Docs
description: Azure Service Bus atomik işlemler ve aracılığıyla Gönder'na genel bakış
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 64449247-1026-44ba-b15a-9610f9385ed8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/22/2018
ms.author: aschhab
ms.openlocfilehash: a839a4cad824a74bde388317cf3aaddf9c5bd47f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60332355"
---
# <a name="overview-of-service-bus-transaction-processing"></a>Service Bus hareket işleme genel bakış

Bu makalede, Microsoft Azure Service Bus işlem yeteneklerini açıklar. Tartışma çoğunu gösterilmiştir [hizmet veri yolu örnek ile AMQP işlemleri](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TransactionsAndSendVia/TransactionsAndSendVia/AMQPTransactionsSendVia). İşlem genel bir bakış için bu makalede sınırlıdır ve *aracılığıyla gönderme* Service Bus atomik işlemler örnek kapsamda daha geniş ve daha karmaşık olmakla birlikte, özelliği.

## <a name="transactions-in-service-bus"></a>Hizmet veri yolu işlemleri

A *işlem* iki veya daha fazla işlem içine gruplandıran bir *yürütme kapsam*. Doğası gereği bu tür bir işlem işlemlerinin belirli bir gruba ait tüm işlemleri başarılı veya başarısız ortaklaşa emin olmanız gerekir. Bu bakımdan, genellikle olarak adlandırılan tek bir birim olarak işlem hareket *kararlılık*.

Service Bus, bir işlem iletisi aracısıdır ve kendi ileti depoları tüm iç işlemler için işlem tutarlılığı sağlar. İletileri taşıma gibi iletileri Service Bus içinde tüm aktarımları bir [eski ileti sırası](service-bus-dead-letter-queues.md) veya [otomatik iletme](service-bus-auto-forwarding.md) varlıklar arasında iletileri işlem olan. Bu nedenle, Service Bus iletiyi kabul ederse, zaten olduğundan depolanır ve bir sıra numarası ile etiketlenmiş. Daha sonra Service Bus içinde tüm ileti aktarımları varlıklar arasında Eşgüdümlü işlemlerdir ve bunların hiçbiri kaybına neden (kaynak başarılı ve başarısız hedef) veya çoğaltma (başarısız kaynak ve hedef başarılı) ileti.

Service Bus, bir hareketin kapsamı içindeki işlemlerin (kuyruk, konu başlığı, abonelik gibi) tek bir mesajlaşma varlığına göre gruplanmasını destekler. Örneğin, bir kuyruktan bir işlem kapsamı içindeki çeşitli iletiler gönderebilir ve işlem başarıyla tamamlandığında iletileri yalnızca sıranın günlüğüne kaydedilmiş olacaktır.

## <a name="operations-within-a-transaction-scope"></a>İşlem kapsamı içindeki işlemler

İşlem kapsamı içinde gerçekleştirilen işlem aşağıdaki gibidir:

* **[QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient), [MessageSender](/dotnet/api/microsoft.azure.servicebus.core.messagesender), [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient)**: SendBatch, SendBatchAsync SendAsync, Gönder 
* **[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: Tamamlandı, CompleteAsync gönderilemeyen, AbandonAsync teslim edilemeyen iletiler, DeadletterAsync, DeferAsync, RenewLock RenewLockAsync ertele 

Alma işlemleri dahil değildir, uygulamayı kullanarak iletileri alması varsayıldığından [ReceiveMode.PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modu, bazı iç döngü almak veya bir [Onmessageoptions](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) geri arama ve ancak bundan sonra iletiyi işlemek için bir işlem kapsamı açılır.

Değerlendirme iletisinin (tam gönderilemeyen, eski ileti erteleme,) üzerinde işlemin sonucunu genel kapsamdaki ve bağımlı sonra gerçekleşir.

## <a name="transfers-and-send-via"></a>Aktarımları ve "aracılığıyla Gönder"

Bir işlemci ve ardından için başka bir kuyruğa veri kuyruğu'ndan işlem devreden MultiPath etkinleştirmek için Service Bus destekler *aktarımları*. Bir aktarım işlemi, bir gönderici önce bir ileti gönderir. bir *aktarımı kuyruk*, ve aktarım sıranın aynı güçlü aktarım uygulamasını kullanarak istenen hedef sırasına hemen iletiyi taşır, otomatik iletme özelliği kullanır. İleti aktarımı sıranın günlük aktarımı sıranın Tüketiciler için görünür duruma gelir emin bir şekilde hiçbir zaman hassastır.

Aktarma sırası gönderenin giriş iletileri kaynağı olduğunda işlem bu özelliğin gücünü belirgin olur. Diğer bir deyişle, Service Bus ileti hedef sırasına aktarma sırası "ile" tam gerçekleştirilirken aktarabilirsiniz (veya erteleme, veya teslim edilemeyen) tek bir işlemde atomik giriş yapılacak işlem. 

### <a name="see-it-in-code"></a>Kodda bakın

Bu tür aktarımı ayarlamak için hedef sırası aktarımı kuyruk üzerinden hedefleyen bir iletiyi gönderenin oluşturun. Ayrıca, aynı kuyruktan iletileri çeken bir alıcı vardır. Örneğin:

```csharp
var connection = new ServiceBusConnection(connectionString);

var sender = new MessageSender(connection, QueueName);
var receiver = new MessageReceiver(connection, QueueName);
```

Aşağıdaki örnekte olduğu gibi bu öğelerden sonra basit bir işlem kullanır. Tam örnek başvurmak için başvuru [kaynak kodu github'da](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TransactionsAndSendVia/TransactionsAndSendVia/AMQPTransactionsSendVia):

```csharp
var receivedMessage = await receiver.ReceiveAsync();

using (var ts = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
{
    try
    {
        // do some processing
        if (receivedMessage != null)
            await receiver.CompleteAsync(receivedMessage.SystemProperties.LockToken);

        var myMsgBody = new MyMessage
        {
            Name = "Some name",
            Address = "Some street address",
            ZipCode = "Some zip code"
        };

        // send message
        var message = myMsgBody.AsMessage();
        await sender.SendAsync(message).ConfigureAwait(false);
        Console.WriteLine("Message has been sent");

        // complete the transaction
        ts.Complete();
    }
    catch (Exception ex)
    {
        // This rolls back send and complete in case an exception happens
        ts.Dispose();
        Console.WriteLine(ex.ToString());
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Service Bus kuyrukları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Service Bus kuyruklarını kullanma](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus varlıklarına otomatik yönlendirme özellikli zincir oluşturma](service-bus-auto-forwarding.md)
* [Otomatik iletme örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Hizmet veri yolu örnek ile atomik işlemler](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Azure kuyrukları ve Service Bus kuyrukları karşılaştırma](service-bus-azure-and-service-bus-queues-compared-contrasted.md)


