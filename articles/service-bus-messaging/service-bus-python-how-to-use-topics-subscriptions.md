---
title: Python ile Azure Service Bus konu başlıklarını kullanma | Microsoft Docs
description: Azure Service Bus konuları ve abonelikleri python'dan kullanmayı öğrenin.
services: service-bus-messaging
documentationcenter: python
author: axisc
manager: timlt
editor: spelluru
ms.assetid: c4f1d76c-7567-4b33-9193-3788f82934e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 04/15/2019
ms.author: aschhab
ms.openlocfilehash: cd75ba9d407399703a382596019d5f370808b20a
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543674"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-python"></a>Service Bus konuları ve abonelikleri ile Python kullanma

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makale, Service Bus konu başlıklarını ve aboneliklerini kullanmayı açıklar. Python ve kullanım örnekleri yazılır [Azure Python SDK'sı paketinin][Azure Python package]. Kapsanan senaryolar şunlardır:

- Konuları ve abonelikleri oluşturma 
- Abonelik filtreleri oluşturma 
- Bir konu başlığına ileti gönderme 
- Abonelikten ileti alma
- Konuları ve abonelikleri silme

## <a name="prerequisites"></a>Önkoşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Etkinleştirebilir, [Visual Studio veya MSDN abone Avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) veya kaydolun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. İzleyeceğiniz adımlar [hızlı başlangıç: Bir Service Bus konusu ve konu için Abonelik oluşturmak için Azure portal'ı kullanmanızı](service-bus-quickstart-topics-subscriptions-portal.md) bir Service Bus'ı oluşturmak için **ad alanı** ve **bağlantı dizesi**.

    > [!NOTE]
    > Oluşturacağınız bir **konu** ve **abonelik** kullanarak konuya **Python** Bu hızlı başlangıçta. 
3. Yükleme [Azure Python paketini][Azure Python package]. Bkz: [Python Yükleme Kılavuzu](../python-how-to-install.md).

## <a name="create-a-topic"></a>Konu başlığı oluşturma

**ServiceBusService** nesnesi konuları ile çalışmanıza olanak sağlar. Service Bus programlı olarak erişmek istiyorsanız, herhangi bir Python dosyasının en üstüne yakın aşağıdaki kodu ekleyin:

```python
from azure.servicebus.control_client import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

Aşağıdaki kod oluşturur bir **ServiceBusService** nesne. Değiştirin `mynamespace`, `sharedaccesskeyname`, ve `sharedaccesskey` gerçek, ad alanı ile paylaşılan erişim imzası (SAS) anahtar adını ve anahtar değeri.

```python
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

SAS anahtar adı değerlerini almak ve değerini [Azure portalında][Azure portal].

```python
bus_service.create_topic('mytopic')
```

`create_topic` Yöntemi, varsayılan ileti yaşam süresi veya en büyük konu boyutu gibi konu başlığı ayarlarını geçersiz kılmak etkinleştirdiğiniz ek seçenekler de destekler. Aşağıdaki örnekte, en büyük konu boyutu 5 GB ve bir süre için Canlı bir dakika (TTL) değerini ayarlar:

```python
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## <a name="create-subscriptions"></a>Abonelik oluşturma

Konu abonelikleri ile de oluşturulur **ServiceBusService** nesne. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna teslim ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

> [!NOTE]
> Varsayılan olarak, abonelikleri kalıcıdır ve bunlar ya da, bunlar abonesiniz, silinen konu kadar var olmaya devam eder.
> 
> Otomatik olarak ayarlayarak silinmiş abonelikleri olabilir [auto_delete_on_idle özelliği](https://docs.microsoft.com/python/api/azure-mgmt-servicebus/azure.mgmt.servicebus.models.sbsubscription?view=azure-python).

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma

Yeni bir abonelik oluşturulurken filtre belirtilmezse **MatchAll** filtre (varsayılan) kullanılır. Zaman **MatchAll** filtresinin kullanılacağının, konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnekte adlı bir abonelik oluşturur `AllMessages` ve varsayılan **MatchAll** filtre.

```python
bus_service.create_subscription('mytopic', 'AllMessages')
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma

Hangi belirtmenize olanak sağlayan filtreler de tanımlayabilirsiniz konu başlığına gönderilen iletilerin görünmesi bir özel konu başlığı aboneliğinde.

En esnek filtre türü, abonelikler tarafından desteklenen bir **SqlFilter**, SQL92 kümesini uygular. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SQL filtresi ile kullanılabilen ifadeler hakkında daha fazla bilgi edinmek için [SqlFilter.SqlExpression][SqlFilter.SqlExpression] söz dizimine bakın.

Filtreleri kullanarak için bir abonelik ekleyebilmeniz için **oluşturma\_kural** yöntemi **ServiceBusService** nesne. Bu yöntem, mevcut bir aboneliğe yeni filtreler eklemenize olanak tanır.

> [!NOTE]
> Varsayılan filtre tüm yeni abonelikler için otomatik olarak uygulandığından, önce varsayılan filtreyi kaldırmalısınız veya **MatchAll** , belirttiğiniz diğer filtreleri geçersiz kılar. Varsayılan kural kullanarak kaldırabilirsiniz `delete_rule` yöntemi **ServiceBusService** nesne.
> 
> 

Aşağıdaki örnekte adlı bir abonelik oluşturur `HighMessages` ile bir **SqlFilter** , yalnızca özel bir bulunduran iletileri seçen `messagenumber` özelliği 3'ten büyük:

```python
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

Benzer şekilde, aşağıdaki örnekte adlı bir abonelik oluşturur `LowMessages` ile bir **SqlFilter** , yalnızca bulunduran iletileri seçen bir `messagenumber` özelliğini daha az veya eşit 3:

```python
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

Şimdi ne zaman bir ileti gönderilir `mytopic` abone alıcılar için her zaman teslim **AllMessages** konu aboneliği ve seçmeli olarak teslim edilen alıcılar abone için **HighMessages**ve **MessageNumber** konu abonelikleri (ileti içeriğine göre) bağlı olarak.

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme

Bir Service Bus konusuna bir ileti göndermek için uygulamanızı kullanmalısınız `send_topic_message` yöntemi **ServiceBusService** nesne.

Aşağıdaki örnek nasıl beş test iletisi göndereceğinizi gösterir `mytopic`. `messagenumber` Her ileti özelliği değerinin değişen döngü yinelemeyi (Bu alacak abonelikleri belirler):

```python
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Konu başlığında tutulan ileti sayısına ilişkin bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotaları hakkında daha fazla bilgi için bkz: [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma

Kullanarak bir abonelik iletilerin alındığı `receive_subscription_message` metodunda **ServiceBusService** nesnesi:

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

İletileri abonelikten silinir, ne zaman okunurlar gibi parametre `peek_lock` ayarlanır **False**. (Özet) okuyun ve iletiyi kuyruktan parametresini ayarlayarak silmeden kilitleme `peek_lock` için **True**.

Okuma ve ileti alma işleminin bir parçası olarak silme davranışı en basit modeldir ve bir uygulama başarısız olduğunda bir iletiyi işlememeye izin dayanabilir senaryolar için en iyi şekilde çalışır. Bu davranışı anlamak için hangi tüketici alma isteği bildirdiğini ve ardından işlenmeden önce kilitleniyor bir senaryo düşünün. Service Bus iletiyi kullanılıyor olarak işaretlediğinden, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında ardından onu çökmenin öncesinde kullanılan iletiyi eksik.

Varsa `peek_lock` parametrenin ayarlanmış **True**, alma, atlanan iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya güvenilir bir şekilde işlemek üzere depolar sonra) çağırarak alma işleminin ikinci aşamasını tamamlar `delete` metodunda **ileti** nesne. `delete` Yöntemi iletiyi kullanılıyor olarak işaretler ve abonelikten kaldırır.

```python
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
if msg.body is not None:
print(msg.body)
msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme

Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi işlemek için herhangi bir nedenle silemiyor sonra çağırabilirsiniz `unlock` metodunda **ileti** nesne. Bu yöntem Bus hizmetinin Abonelikteki iletinin kilidini açmasına ve iletiyi aynı kullanıcı uygulama veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale neden olur.

Ayrıca abonelikte kilitlenen iletiye ilişkin bir zaman aşımı yoktur ve uygulamanın iletiyi işleyemezse (örneğin, uygulama çökerse) önce kilit zaman aşımı süresi dolduktan sonra Service Bus ileti otomatik olarak kilidini açar ve kullanılabilir tekrar alınabilir hale getirir.

Uygulama iletiyi ancak önce çökmesi durumunda, `delete` yöntemi çağrılır, ardından yeniden başlatıldığında ileti uygulamaya yeniden teslim edilebilir. Bu davranış, genellikle olarak adlandırılır. En az bir kez işlenmesini\*; diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bunu yapmak için kullanabileceğiniz **MessageID** özelliğini iletinin teslim denemeleri arasında sabit kalır.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme

Konuları ve abonelikleri kalıcı sürece [auto_delete_on_idle özelliği](https://docs.microsoft.com/python/api/azure-mgmt-servicebus/azure.mgmt.servicebus.models.sbsubscription?view=azure-python) ayarlanır. Olabilirler aracılığıyla silindi [Azure portalında][Azure portal] veya programlama yoluyla. Aşağıdaki örnekte adlı konu gösterilmektedir `mytopic`:

```python
bus_service.delete_topic('mytopic')
```

Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir. Aşağıdaki kod, adlı bir abonelik silme işlemi gösterilmektedir `HighMessages` gelen `mytopic` konu:

```python
bus_service.delete_subscription('mytopic', 'HighMessages')
```

> [!NOTE]
> Service Bus kaynakları ile yönetebileceğiniz [hizmet veri yolu Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer/). Hizmet veri yolu Gezgini, bir Service Bus ad alanınıza bağlanın ve mesajlaşma varlıkları kolay bir şekilde yönetmek kullanıcıların sağlar. Araç, içeri/dışarı aktarma işlevleri veya konu, kuyruklar, abonelikler, geçiş hizmetleri, bildirim hub'ları ve olay hub'ları test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

Hizmet veri yolu konuları hakkındaki temel bilgileri öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin.

* Bkz: [kuyruklar, konular ve abonelikler][Queues, topics, and subscriptions].
* İçin başvuru [SqlFilter.SqlExpression][SqlFilter.SqlExpression].

[Azure portal]: https://portal.azure.com
[Azure Python package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Service Bus quotas]: service-bus-quotas.md 
