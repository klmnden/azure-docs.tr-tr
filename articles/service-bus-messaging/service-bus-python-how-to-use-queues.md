---
title: Python ile Azure Service Bus kuyruklarını kullanma | Microsoft Docs
description: Python'dan Azure Service Bus kuyruklarını kullanmayı öğrenin.
services: service-bus-messaging
documentationcenter: python
author: spelluru
manager: timlt
editor: ''
ms.assetid: b95ee5cd-3b31-459c-a7f3-cf8bcf77858b
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 04/30/2018
ms.author: spelluru
ms.openlocfilehash: afc310ce4dd373b632f183245ab427a3a65a0af6
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696777"
---
# <a name="how-to-use-service-bus-queues-with-python"></a>Python ile Service Bus kuyruklarını kullanma

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu makalede, Service Bus kuyruklarının nasıl kullanılacağı açıklanır. Python ve kullanım örnekleri yazılır [Python Azure Service Bus paket][Python Azure Service Bus package]. Senaryoları ele alınmaktadır **ileti gönderme ve alma sıra oluşturma**, ve **sıraları silme**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

> [!NOTE]
> Python'ı yüklemek için veya [Python Azure Service Bus paket][Python Azure Service Bus package], bkz: [Python Yükleme Kılavuzu](../python-how-to-install.md).
> 
> 

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
**ServiceBusService** nesnesi kuyrukları ile çalışmanıza olanak sağlar. Service Bus programlı olarak erişmek istiyorsanız, herhangi bir Python dosyasının en üstüne yakın aşağıdaki kodu ekleyin:

```python
from azure.servicebus import ServiceBusService, Message, Queue
```

Aşağıdaki kod oluşturur bir **ServiceBusService** nesne. Değiştirin `mynamespace`, `sharedaccesskeyname`, ve `sharedaccesskey` ad alanı, paylaşılan erişim imzası (SAS) anahtar adını ve değeri.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

SAS anahtar adını ve değerini değerleri bulunabilir [Azure portalında] [ Azure portal] bağlantı bilgilerini veya Visual Studio **özellikleri** hizmet seçerken bölmesi Sunucu Gezgini'ndeki (önceki bölümde gösterildiği gibi) veri yolu ad alanı.

```python
bus_service.create_queue('taskqueue')
```

`create_queue` Yöntemi de ileti süresi (TTL) Canlı ya da en büyük sıra boyutu gibi varsayılan kuyruk ayarlarını geçersiz kılmak etkinleştirdiğiniz ek seçenekler destekler. Aşağıdaki örnek, 5 GB ile 1 dakika TTL değeri için en büyük sıra boyutu ayarlar:

```python
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Uygulama çağrılarınızı, bir Service Bus kuyruğuna bir ileti göndermek için `send_queue_message` metodunda **ServiceBusService** nesne.

Aşağıdaki örnekte adlı kuyruk için bir test iletisi göndermek nasıl gösterir `taskqueue` kullanarak `send_queue_message`:

```python
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotaları hakkında daha fazla bilgi için bkz: [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Bir kuyruktan ileti alma
İletilerin kullanarak bir sıraya alındığı `receive_queue_message` metodunda **ServiceBusService** nesnesi:

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

İleti kuyruktan silinir, ne zaman okunurlar gibi parametre `peek_lock` ayarlanır **False**. (Özet) okuyun ve iletiyi kuyruktan parametresini ayarlayarak silmeden kilitleme `peek_lock` için **True**.

Okuma ve ileti alma işleminin bir parçası olarak silme davranışı en basit modeldir ve uygulamanın bir arıza olması durumunda bir iletiyi işlememeye izin dayanabilir senaryolar için en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretleyeceğinden, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında ardından onu çökmenin öncesinde kullanılan iletiyi atlamış olur.

Varsa `peek_lock` parametrenin ayarlanmış **True**, alma, atlanan iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya güvenilir bir şekilde işlemek üzere depolar sonra) çağırarak alma işleminin ikinci aşamasını tamamlar **Sil** metodunda **ileti** nesne. **Sil** yöntemi iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırın.

```python
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi işlemek için herhangi bir nedenle silemiyor sonra çağırabilirsiniz **kilidini** metodunda **ileti** nesne. Bu, Service Bus'ın Kuyruktaki iletinin kilidini açmasına ve iletiyi aynı kullanıcı uygulama veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı yoktur ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açmasına ve hale kilit zaman aşımı dolmadan tekrar kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, **Sil** yöntemi çağrılır, ardından yeniden başlatıldığında ileti uygulamaya yeniden teslim edilebilir. Buna genellikle denir **en az bir kez işleme**, diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu genellikle kullanılmasıdır **MessageID** özelliğini iletinin teslim denemeleri arasında sabit kalır.

## <a name="next-steps"></a>Sonraki adımlar
Service Bus kuyruklarına ilişkin temel bilgileri öğrendiniz, daha fazla bilgi için şu makalelere bakın.

* [Kuyruklar, konular ve abonelikler][Queues, topics, and subscriptions]

[Azure portal]: https://portal.azure.com
[Python Azure Service Bus package]: https://pypi.python.org/pypi/azure-servicebus  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Service Bus quotas]: service-bus-quotas.md

