---
title: Service Bus konu başlıklarını (Ruby) kullanma | Microsoft Docs
description: Azure'da Service Bus konu başlıklarını ve aboneliklerini kullanmayı öğrenin. Kod örnekleri Söyleniş uygulamalarına yönelik yazılır.
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 3ef2295e-7c5f-4c54-a13b-a69c8045d4b6
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 4a4c9949843b16ae6be2f516de4fd1e3f7415959
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23926798"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-ruby"></a>Service Bus konuları ve abonelikleri Ruby ile kullanma
 
[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makale, Service Bus konuları ve abonelikleri Söyleniş uygulamalardan kullanmayı açıklar. Kapsamdaki senaryolar dahil **konuları ve Abonelikleri, ileti gönderme, abonelik filtreleri oluşturma oluşturma** bir konuya **abonelikten ileti alma**, ve **silme konu başlıkları ve abonelikler**. Konu başlıkları ve abonelikler hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="create-a-topic"></a>Konu başlığı oluşturma
**Azure::ServiceBusService** nesne konularda çalışmanıza olanak sağlar. Aşağıdaki kod oluşturur bir **Azure::ServiceBusService** nesnesi. Bir konu başlığı oluşturmak için kullanmak `create_topic()` yöntemi. Aşağıdaki örnek, bir konu oluşturur veya varsa hataları yazdırır.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Ayrıca iletebilirsiniz bir **Azure::ServiceBus::Topic** Canlı veya en büyük sıra boyutu ileti süresi gibi varsayılan konu ayarlarını geçersiz kılmanıza olanak sağlayan ek seçeneklere sahip nesne. Aşağıdaki örnek, en büyük sıra boyutu 5 GB ve saat 1 dakika ile canlı olarak ayarlanması gösterir:

```ruby
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Abonelikleri oluşturma
Konu aboneliklerini içeren de oluşturulur **Azure::ServiceBusService** nesnesi. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna teslim edilen ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

Abonelikleri kalıcı ve ya da bunlar kadar var olmaya devam eder ya da ilişkili konu birlikte silinir. Uygulamanız bir aboneliği oluşturmak için mantığı içeriyorsa, ilk abonelik zaten getSubscription yöntemini kullanarak olup olmadığını denetlemelisiniz.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
**MatchAll** filtresi, yeni bir abonelik oluşturulurken filtre belirtilmeyen durumlarda kullanılan varsayılan filtredir. Zaman **MatchAll** filtre kullanıldığında, konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnekte "tüm iletileri" adlı bir abonelik oluşturulur ve varsayılan **MatchAll** Filtresi.

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
Ayrıca, hangi belirtmenize olanak verir filtreleri tanımlayabilirsiniz konu başlığına gönderilen iletilerin görünmesini içinde belirli bir aboneliği.

Filtre abonelikler tarafından desteklenen en esnek türü **Azure::ServiceBus::SqlFilter**, SQL92 alt kümesi uygular. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SQL Filtresi ile kullanılabilen ifadeler hakkında daha fazla ayrıntı için gözden [SqlFilter](service-bus-messaging-sql-filter.md) sözdizimi.

Filtreleri kullanarak bir abonelik ekleyebilmeniz için `create_rule()` yöntemi **Azure::ServiceBusService** nesnesi. Bu yöntem, mevcut bir aboneliğe yeni filtreler eklemenize olanak sağlar.

Varsayılan filtre tüm yeni abonelikler için otomatik olarak uygulanan olduğundan, varsayılan filtre önce kaldırmanız gerekir veya **MatchAll** belirttiğiniz diğer filtreleri geçersiz kılar. Varsayılan kural kullanarak kaldırabilirsiniz `delete_rule()` yöntemi **Azure::ServiceBusService** nesnesi.

Aşağıdaki örnekte "iletileri yüksek" adlı bir abonelik oluşturur bir **Azure::ServiceBus::SqlFilter** özel iletileri yalnızca seçer `message_number` özelliği 3'ten büyük:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Benzer şekilde, aşağıdaki örnekte adlı bir abonelik oluşturulur `low-messages` ile bir **Azure::ServiceBus::SqlFilter** olan iletiler yalnızca seçer bir `message_number` özelliğine daha az veya bu değere eşit 3:

```ruby
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Ne zaman bir ileti artık gönderildi `test-topic`, onu olduğundan her zaman teslim edilemiyor alıcılara `all-messages` konu aboneliği ve alıcılara teslim seçmeli olarak `high-messages` ve `low-messages` (bağlı olarak konu abonelikleri ileti içeriği).

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Service Bus konu başlığına bir ileti göndermek için uygulamanızı kullanmalısınız `send_topic_message()` yöntemi **Azure::ServiceBusService** nesnesi. Service Bus konu başlıklarına gönderilen iletiler örnekleridir **Azure::ServiceBus::BrokeredMessage** nesneleri. **Azure::ServiceBus::BrokeredMessage** nesneleri olan bir standart özellikler kümesi (gibi `label` ve `time_to_live`), bir uygulamaya özgü özel özellikleri tutmak için kullanılan sözlüğü ve dize verileri gövdesi içerir. Bir uygulama için bir dize değeri geçirerek ileti gövdesini ayarlayabilir `send_topic_message()` yöntemi ve tüm gerekli standart özellikleri varsayılan değerlerle doldurulmuş.

Aşağıdaki örnekte nasıl beş test iletisi göndereceğinizi gösterir `test-topic`. Unutmayın `message_number` özel özellik değerini her iletinin tekrarına döngü üzerinde (Bu abonelik aldığı belirler):

```ruby
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Konu başlığında tutulan ileti sayısına ilişkin bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
İletileri kullanarak bir abonelik alınan `receive_subscription_message()` yöntemi **Azure::ServiceBusService** nesnesi. Varsayılan olarak, iletiler read(peak) ve abonelikten silmeden kilitli. Okuma ve ayarlayarak, abonelikten ileti silmek `peek_lock` için seçenek **false**.

Varsayılan davranış, okuma ve ayrıca, iletilere uygulamaları desteklemek mümkün kılar bir iki aşamalı işlemi silme yapar. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya sonra işlemek için depoladıktan sonra), çağırarak alma işleminin ikinci aşamasını tamamlar `delete_subscription_message()` yöntemi ve parametre olarak silinecek ileti sağlama. `delete_subscription_message()` Yöntemi iletiyi kullanılıyor olarak işaretler ve abonelikten kaldırır.

Varsa `:peek_lock` parametrenin ayarlanmış **yanlış**, okuma ve iletisi siliniyor basit model olur ve senaryoları bir uygulama içinde tolerans bir arıza olması durumunda bir ileti işlenirken değil en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretlenmiş nedeniyle uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, sonra da çökmenin öncesinde kullanılan iletiyi atlamış olur.

Aşağıdaki örnek nasıl ileti aldı ve işlenen kullanarak gösterir `receive_subscription_message()`. Örnek ilk alır ve bir iletiden siler `low-messages` kullanarak abonelik `:peek_lock` kümesine **false**, başka bir iletiden aldıktan sonra `high-messages` ve iletiyi kullanaraksiler`delete_subscription_message()`:

```ruby
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi herhangi bir nedenden dolayı işleyemedi sonra işleyememesi `unlock_subscription_message()` yöntemi **Azure::ServiceBusService** nesnesi. Bu işlem, Service Bus hizmetinin abonelikteki iletinin kilidini açmasına ve iletiyi aynı veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale getirmesine neden olur.

Ayrıca abonelikte kilitlenen iletiye ilişkin bir zaman aşımı vardır ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini kilit zaman aşımı dolmadan ve tekrar alınabilmesini kullanılabilir yapın.

Uygulama iletiyi ancak önce çökmesi durumunda, `delete_subscription_message()` yöntemi çağrıldıktan sonra yeniden başlatıldığında ileti uygulamaya tekrar teslim. Bu genellikle adlandırılır *en az bir kez işleme*; diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu mantık, genellikle kullanılarak gerçekleştirilir `message_id` özelliğini iletinin teslimat denemelerinde.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Konu başlıkları ve abonelikler kalıcıdır ve açıkça olmalıdır aracılığıyla ya da silinmiş [Azure portal] [ Azure portal] veya program aracılığıyla. Aşağıdaki örnek adlı konuyu silmek gösterilmiştir `test-topic`.

```ruby
azure_service_bus_service.delete_topic("test-topic")
```

Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir. Aşağıdaki kodda adlı abonelik silmek gösterilmiştir `high-messages` gelen `test-topic` konu:

```ruby
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Sonraki adımlar
Service Bus konuları öğrendiğinize göre daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* Bkz: [kuyruklar, konu başlıkları ve abonelikler](service-bus-queues-topics-subscriptions.md).
* [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter#microsoft_servicebus_messaging_sqlfilter) için API başvurusu
* Ziyaret [Ruby için Azure SDK](https://github.com/Azure/azure-sdk-for-ruby) github'daki.

[Azure portal]: https://portal.azure.com
