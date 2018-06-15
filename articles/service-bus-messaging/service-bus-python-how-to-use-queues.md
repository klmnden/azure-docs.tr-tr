---
title: Python ile Azure Service Bus kuyruklarını kullanma | Microsoft Docs
description: Python'dan Azure Service Bus kuyruklarını kullanmayı öğrenin.
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 04/30/2018
ms.author: sethm
ms.openlocfilehash: aa0f243f4a5bc3d84c580b950bcf0ed7a78362e7
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
ms.locfileid: "32312493"
---
# <a name="how-to-use-service-bus-queues-with-python"></a>Python ile Service Bus kuyruklarını kullanma

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu makalede, Service Bus kuyruklarının nasıl kullanılacağı açıklanır. Python ve kullanım örnekleri yazılır [Python Azure Service Bus paket][Python Azure Service Bus package]. Kapsamdaki senaryolar dahil **ileti gönderme ve alma sıra oluşturma**, ve **sıraları silme**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> Python yüklemek için veya [Python Azure Service Bus paket][Python Azure Service Bus package], bkz: [Python Yükleme Kılavuzu'na](../python-how-to-install.md).
> 
> 

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
**ServiceBusService** nesne kuyruklarla çalışmanıza olanak sağlar. Service Bus programlı olarak erişmek istediğiniz tüm Python dosyanın en üstüne yakın aşağıdaki kodu ekleyin:

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

Aşağıdaki kod oluşturur bir **ServiceBusService** nesnesi. Değiştir `mynamespace`, `sharedaccesskeyname`, ve `sharedaccesskey` ad alanı, paylaşılan erişim imzası (SAS) anahtar adını ve değeri.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

SAS anahtar adını ve değerini değerlerini bulunabilir [Azure portal] [ Azure portal] bağlantı bilgilerini veya Visual Studio'da **özellikleri** hizmet seçerken bölmesi Veri yolu ad alanı Sunucu Gezgininde (önceki bölümde gösterildiği gibi).

```python
bus_service.create_queue('taskqueue')
```

`create_queue` Yöntemi de ileti zamanı dinamik (TTL) veya en büyük sıra boyutu gibi varsayılan sırası ayarlarını geçersiz kılmanıza olanak sağlayan ek seçenekleri destekler. Aşağıdaki örnek en büyük sıra boyutu 5 GB ve TTL değeri 1 dakika olarak ayarlar:

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Uygulama çağrılarınızı bir Service Bus kuyruğuna bir ileti göndermek için `send_queue_message` yöntemi **ServiceBusService** nesnesi.

Aşağıdaki örnek adlı sırasına sınama iletisi göndermek nasıl gösterir `taskqueue` kullanarak `send_queue_message`:

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotalar hakkında daha fazla bilgi için bkz: [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma
İletileri kullanarak bir Sıraya alınan `receive_queue_message` yöntemi **ServiceBusService** nesnesi:

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

İletileri zaman okunduğu gibi sıradan silinir parametresi `peek_lock` ayarlanır **False**. (Özet) okuma ve iletiyi sıradan parametresi ayarlanarak silmeden kilitlemek `peek_lock` için **doğru**.

Okuma ve ileti alma işleminin bir parçası olarak silme davranışı en basit modeldir ve uygulamanın hata oluştuğunda bir iletiyi işlemeyi değil dayanabileceği senaryoları için en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretlenmiş nedeniyle uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, sonra da çökmenin öncesinde kullanılan iletiyi atlamış olur.

Varsa `peek_lock` parametrenin ayarlanmış **doğru**, alma, iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya sonra işlemek için depoladıktan sonra), çağırarak alma işleminin ikinci aşamasını tamamlar **silmek** yöntemi **ileti** nesne. **Silmek** yöntemi iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırır.

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi herhangi bir nedenden dolayı işleyemedi sonra işleyememesi **kilidini** yöntemi **ileti** nesnesi. Bu, Service Bus hizmetinin Kuyruktaki iletinin kilidini açmasına ve iletiyi aynı veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale getirmesine neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı vardır ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açmak ve onu kilit zaman aşımı dolmadan yeniden alınabilmesi kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, **silmek** yöntemi çağrıldıktan sonra yeniden başlatıldığında ileti uygulamaya tekrar teslim edilir. Bu genellikle adlandırılır **en az bir kez işleme**, diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu genellikle kullanılarak elde edilen **MessageID** özelliğini iletinin teslimat denemelerinde.

## <a name="next-steps"></a>Sonraki adımlar
Service Bus kuyruklarını temel bilgileri öğrendiniz, daha fazla bilgi için aşağıdaki makalelere bakın.

* [Kuyruklar, konu başlıkları ve abonelikler][Queues, topics, and subscriptions]

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

