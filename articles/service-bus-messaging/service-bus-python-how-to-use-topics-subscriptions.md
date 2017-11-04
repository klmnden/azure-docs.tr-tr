---
title: "Python ile Azure Service Bus konu başlıklarını kullanma | Microsoft Docs"
description: "Azure Service Bus konuları ve abonelikleri python'dan kullanmayı öğrenin."
services: service-bus-messaging
documentationcenter: python
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 15269f9728e9dc45e6436e53b1859f76d4a7a0c9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-python"></a>Service Bus konuları ve abonelikleri Python ile kullanma

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makale, Service Bus konu başlıklarını ve aboneliklerini kullanmayı açıklar. Python ve kullanım örnekleri yazılır [Azure Python SDK paketi][Azure Python package]. Kapsamdaki senaryolar dahil **konuları ve abonelikleri oluşturma**, **abonelik filtreleri oluşturma**, **konu başlığına ileti gönderme**, **abonelikten ileti alma**, ve **konuları ve abonelikleri silmeyi**. Konu başlıkları ve abonelikler hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

> [!NOTE] 
> Python yüklemeniz gerekiyorsa veya [Azure Python paket][Azure Python package], bkz: [Python Yükleme Kılavuzu'na](../python-how-to-install.md).

## <a name="create-a-topic"></a>Konu başlığı oluşturma
**ServiceBusService** nesne konularda çalışmanıza olanak sağlar. Service Bus programlı olarak erişmek istediğiniz tüm Python dosyanın en üstüne yakın aşağıdakileri ekleyin:

```python
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Aşağıdaki kod oluşturur bir **ServiceBusService** nesnesi. Değiştir `mynamespace`, `sharedaccesskeyname`, ve `sharedaccesskey` ile gerçek ad alanınız, paylaşılan erişim imzası (SAS) anahtar adını ve anahtar değeri.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

SAS anahtar adı için değerleri elde edilir ve değeri [Azure portal][Azure portal].

```python
bus_service.create_topic('mytopic')
```

`create_topic` Yöntemi de ileti yaşam süresi veya en fazla konu başlığı boyutu gibi varsayılan konu ayarlarını geçersiz kılmak üzere etkinleştirmeniz ek seçenekleri destekler. Aşağıdaki örnek en büyük konu başlığı boyutu 5 GB ve bir süre için 1 dakika (TTL) değerini Canlı ayarlar:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Abonelikleri oluşturma
Konular için abonelikleri ile de oluşturulur **ServiceBusService** nesnesi. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna teslim edilen ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

> [!NOTE]
> Abonelikleri kalıcı ve bunlar ya da, bunlar abone olduğunuz, silinen konu kadar var olmaya devam eder.
> 
> 

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
**MatchAll** filtresi, yeni bir abonelik oluşturulurken filtre belirtilmeyen durumlarda kullanılan varsayılan filtredir. Zaman **MatchAll** filtre kullanıldığında, konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnek adlı bir abonelik oluşturur `AllMessages` ve varsayılan **MatchAll** Filtresi.

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
Ayrıca, hangi belirtmenize olanak verir filtreleri tanımlayabilirsiniz konu başlığına gönderilen iletilerin görünmesini içinde belirli konu aboneliği.

Filtre abonelikler tarafından desteklenen en esnek türünde bir **SqlFilter**, SQL92 alt kümesi uygular. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SQL filtresi ile kullanılabilen ifadeler hakkında daha fazla bilgi edinmek için [SqlFilter.SqlExpression][SqlFilter.SqlExpression] söz dizimine bakın.

Filtreleri kullanarak bir abonelik ekleyebilmeniz için **oluşturma\_kural** yöntemi **ServiceBusService** nesnesi. Bu yöntem, mevcut bir aboneliğe yeni filtreler eklemenizi sağlar.

> [!NOTE]
> Varsayılan filtre otomatik olarak tüm yeni abonelikleri uygulandığından, varsayılan filtre önce kaldırmanız gerekir veya **MatchAll** belirttiğiniz diğer filtreleri geçersiz kılar. Varsayılan kural kullanarak kaldırabilirsiniz `delete_rule` yöntemi **ServiceBusService** nesnesi.
> 
> 

Aşağıdaki örnek adlı bir abonelik oluşturur `HighMessages` ile bir **SqlFilter** özel iletileri yalnızca seçer `messagenumber` özelliği 3'ten büyük:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Benzer şekilde, aşağıdaki örnekte adlı bir abonelik oluşturulur `LowMessages` ile bir **SqlFilter** olan iletiler yalnızca seçer bir `messagenumber` özelliğine daha az veya bu değere eşit 3:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Şimdi, ne zaman bir ileti gönderilir `mytopic` her zaman alıcılara teslim edilir **AllMessages** konu aboneliği ve alıcılara teslim seçmeli olarak **HighMessages** ve **LowMessages** konu abonelikleri (bağlı olarak ileti içeriği).

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Service Bus konu başlığına bir ileti göndermek için uygulamanızı kullanmalısınız `send_topic_message` yöntemi **ServiceBusService** nesnesi.

Aşağıdaki örnekte nasıl beş test iletisi göndereceğinizi gösterir `mytopic`. Unutmayın `messagenumber` özellik değeri her iletinin tekrarına döngü üzerinde (Bu alacak abonelikleri belirler):

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Konu başlığında tutulan ileti sayısına ilişkin bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotalar hakkında daha fazla bilgi için bkz: [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
İletileri kullanarak bir abonelik alınan `receive_subscription_message` yöntemi **ServiceBusService** nesnesi:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

İletileri zaman okunduğu gibi abonelikten silindi parametresi `peek_lock` ayarlanır **False**. (Özet) okuma ve iletiyi sıradan parametresi ayarlanarak silmeden kilitlemek `peek_lock` için **doğru**.

Okuma ve ileti alma işleminin bir parçası olarak silme davranışı en basit modeldir ve uygulamanın hata oluştuğunda bir iletiyi işlemeyi değil dayanabileceği senaryoları için en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretlenmiş nedeniyle uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, sonra da çökmenin öncesinde kullanılan iletiyi atlamış olur.

Varsa `peek_lock` parametrenin ayarlanmış **doğru**, alma, iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya sonra işlemek için depoladıktan sonra), çağırarak alma işleminin ikinci aşamasını tamamlar `delete` yöntemi **ileti** nesnesi. `delete` Yöntemi iletiyi kullanılıyor olarak işaretler ve abonelikten kaldırır.

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi herhangi bir nedenden dolayı işleyemedi sonra işleyememesi `unlock` yöntemi **ileti** nesnesi. Bu iletinin kilidini açmak ve aynı kullanıcı uygulama tarafından veya başka bir kullanıcı uygulama tarafından tekrar alınabilir kullanılabilir hale getirmek Service Bus neden olur.

Ayrıca abonelikte kilitlenen iletiye ilişkin bir zaman aşımı vardır ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açar ve tekrar alınabilmesini sağlar kilit zaman aşımı dolmadan.

Uygulama iletiyi ancak önce çökmesi durumunda, `delete` yöntemi çağrıldıktan sonra yeniden başlatıldığında ileti uygulamaya tekrar teslim edilir. Bu genellikle adlandırılır *en az bir kez işleme*, diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu genellikle kullanılarak elde edilen **MessageID** özelliğini iletinin teslimat denemelerinde.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Konu başlıkları ve abonelikler kalıcıdır ve açıkça olmalıdır aracılığıyla ya da silinmiş [Azure portal] [ Azure portal] veya program aracılığıyla. Aşağıdaki örnekte nasıl adlı konu silineceğini gösterir `mytopic`:

```python
bus_service.delete_topic('mytopic')
```

Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir. Aşağıdaki kod adlı bir aboneliği silme gösterir `HighMessages` gelen `mytopic` konu:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## <a name="next-steps"></a>Sonraki adımlar
Service Bus konuları öğrendiğinize göre daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* Bkz: [kuyruklar, konu başlıkları ve abonelikler][Queues, topics, and subscriptions].
* İçin başvuru [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
