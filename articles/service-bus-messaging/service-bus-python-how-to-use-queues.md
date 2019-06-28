---
title: Python ile Azure Service Bus kuyruklarını kullanma | Microsoft Docs
description: Python'dan Azure Service Bus kuyruklarını kullanmayı öğrenin.
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 04/10/2019
ms.author: aschhab
ms.openlocfilehash: b74238ee49fe0d96d218f1800a33a9d60badc6d5
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341706"
---
# <a name="how-to-use-service-bus-queues-with-python"></a>Python ile Service Bus kuyruklarını kullanma

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu öğreticide, bir Service Bus kuyruğundaki iletileri alıp ileti göndermek için Python uygulamalarının nasıl oluşturulacağını öğrenin. 

## <a name="prerequisites"></a>Önkoşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Etkinleştirebilir, [MSDN abone Avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) veya kaydolun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. İzleyeceğiniz adımlar [Service Bus kuyruğuna oluşturmak için Azure portalını kullanın](service-bus-quickstart-portal.md) makalesi.
    1. Hızlı Okuma **genel bakış** Service Bus **kuyrukları**. 
    2. Hizmet veri yolu oluşturma **ad alanı**. 
    3. Alma **bağlantı dizesi**. 

        > [!NOTE]
        > Oluşturacağınız bir **kuyruk** Bu öğreticide Python kullanarak Service Bus ad alanında. 
1. Yüklemeniz Python veya [Python Azure Service Bus paket][Python Azure Service Bus package], bakın [Python Yükleme Kılavuzu](../python-how-to-install.md). Service Bus Python SDK'ın tam belgelerine bakın [burada](/python/api/overview/azure/servicebus?view=azure-python).

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
**ServiceBusClient** nesnesi kuyrukları ile çalışmanıza olanak sağlar. Service Bus programlı olarak erişmek istiyorsanız, herhangi bir Python dosyasının en üstüne yakın aşağıdaki kodu ekleyin:

```python
from azure.servicebus import ServiceBusClient
```

Aşağıdaki kod oluşturur bir **ServiceBusClient** nesne. Değiştirin `<CONNECTION STRING>` , Service Bus bağlantı dizesi ile.

```python
sb_client = ServiceBusClient.from_connection_string('<CONNECTION STRING>')
```

SAS anahtar adını ve değerini değerleri bulunabilir [Azure portalında][Azure portal] bağlantı bilgilerini veya Visual Studio **özellikleri** Service Bus ad alanı (olarak sunucu Gezgini'nde seçerken bölmesi önceki bölümde gösterilmiştir).

```python
sb_client.create_queue("taskqueue")
```

`create_queue` Yöntemi de ileti süresi (TTL) Canlı ya da en büyük sıra boyutu gibi varsayılan kuyruk ayarlarını geçersiz kılmak etkinleştirdiğiniz ek seçenekler destekler. Aşağıdaki örnek, 5 GB ile 1 dakika TTL değeri için en büyük sıra boyutu ayarlar:

```python
sb_client.create_queue("taskqueue", max_size_in_megabytes=5120, default_message_time_to_live=datetime.timedelta(minutes=1))
```

Daha fazla bilgi için [Azure Service Bus Python belgeleri](/python/api/overview/azure/servicebus?view=azure-python).

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Uygulama çağrılarınızı, bir Service Bus kuyruğuna bir ileti göndermek için `send` metodunda `ServiceBusClient` nesne.

Aşağıdaki örnekte adlı kuyruk için bir test iletisi göndermek nasıl gösterir `taskqueue` kullanarak `send_queue_message`:

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient 
queue_client = QueueClient.from_connection_string("<CONNECTION STRING>", "<QUEUE NAME>")

# Send a test message to the queue
msg = Message(b'Test Message')
queue_client.send(msg)
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotaları hakkında daha fazla bilgi için bkz: [Service Bus kotaları][Service Bus quotas].

Daha fazla bilgi için [Azure Service Bus Python belgeleri](/python/api/overview/azure/servicebus?view=azure-python).

## <a name="receive-messages-from-a-queue"></a>Bir kuyruktan ileti alma
İletilerin kullanarak bir sıraya alındığı `get_receiver` metodunda `ServiceBusService` nesnesi:

```python
from azure.servicebus import QueueClient, Message

# Create the QueueClient 
queue_client = QueueClient.from_connection_string("<CONNECTION STRING>", "<QUEUE NAME>")

## Receive the message from the queue
with queue_client.get_receiver() as queue_receiver:
    messages = queue_receiver.fetch_next(timeout=3)
    for message in messages:
        print(message)
        message.complete()
```

Daha fazla bilgi için [Azure Service Bus Python belgeleri](/python/api/overview/azure/servicebus?view=azure-python).


İleti kuyruktan silinir, ne zaman okunurlar gibi parametre `peek_lock` ayarlanır **False**. (Özet) okuyun ve iletiyi kuyruktan parametresini ayarlayarak silmeden kilitleme `peek_lock` için **True**.

Okuma ve ileti alma işleminin bir parçası olarak silme davranışı en basit modeldir ve uygulamanın bir arıza olması durumunda bir iletiyi işlememeye izin dayanabilir senaryolar için en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretleyeceğinden, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında ardından onu çökmenin öncesinde kullanılan iletiyi atlamış olur.

Varsa `peek_lock` parametrenin ayarlanmış **True**, alma, atlanan iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya güvenilir bir şekilde işlemek üzere depolar sonra) çağırarak alma işleminin ikinci aşamasını tamamlar **Sil** metodunda **ileti** nesne. **Sil** yöntemi iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırın.

```python
msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi işlemek için herhangi bir nedenle silemiyor sonra çağırabilirsiniz **kilidini** metodunda **ileti** nesne. Bu, Service Bus'ın Kuyruktaki iletinin kilidini açmasına ve iletiyi aynı kullanıcı uygulama veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı yoktur ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açmasına ve hale kilit zaman aşımı dolmadan tekrar kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, **Sil** yöntemi çağrılır, ardından yeniden başlatıldığında ileti uygulamaya yeniden teslim edilebilir. Buna genellikle denir **en az bir kez işlenmesini**, diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu işlem, genellikle iletinin teslimat denemelerinde korunan **MessageId** özelliği kullanılarak gerçekleştirilir.

> [!NOTE]
> Service Bus kaynakları ile yönetebileceğiniz [hizmet veri yolu Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer/). Hizmet veri yolu Gezgini, bir Service Bus ad alanınıza bağlanın ve mesajlaşma varlıkları kolay bir şekilde yönetmek kullanıcıların sağlar. Araç, içeri/dışarı aktarma işlevleri veya konu, kuyruklar, abonelikler, geçiş hizmetleri, bildirim hub'ları ve olay hub'ları test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Service Bus kuyruklarına ilişkin temel bilgileri öğrendiniz, daha fazla bilgi için şu makalelere bakın.

* [Kuyruklar, konu başlıkları ve abonelikler][Queues, topics, and subscriptions]

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

