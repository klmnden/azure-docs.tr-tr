---
title: "Ruby ile Azure Service Bus kuyruklarını kullanma | Microsoft Docs"
description: "Azure'da Service Bus kuyruklarını kullanmayı öğrenin. Ruby içinde yazılan kod örnekleri."
services: service-bus-messaging
documentationcenter: ruby
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0a11eab2-823f-4cc7-842b-fbbe0f953751
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 357a7277dd42b6973cf35a9f642b8eec36a745e3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-queues-with-ruby"></a>Ruby ile Service Bus kuyruklarını kullanma

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu kılavuz, Service Bus kuyruklarını kullanmayı açıklar. Örnekler Ruby yazılır ve Azure gem kullanır. Kapsamdaki senaryolar dahil **ileti gönderme ve alma sıra oluşturma**, ve **sıraları silme**. Service Bus kuyruklarını hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]
   
[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-to-create-a-queue"></a>Bir sıra oluşturma
**Azure::ServiceBusService** nesne kuyruklarla çalışmanıza olanak sağlar. Bir kuyruk oluşturmak için kullanmak `create_queue()` yöntemi. Aşağıdaki örnekte bir kuyruk oluşturur veya hataları yazdırır.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Ayrıca iletebilirsiniz bir **Azure::ServiceBus::Queue** nesne ek seçenekleri, Canlı veya en büyük sıra boyutu ileti süresi gibi varsayılan sırası ayarlarını geçersiz kılmanıza olanak sağlar. Aşağıdaki örnekte, en büyük sıra boyutu 5 GB ve saat 1 dakika Canlı ayarlamak gösterilmektedir:

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Kuyruğa ileti göndermek nasıl
Uygulama çağrılarınızı bir Service Bus kuyruğuna bir ileti göndermek için `send_queue_message()` yöntemi **Azure::ServiceBusService** nesnesi. İletileri gönderilen (ve öğesinden alınan) hizmet kuyruklar veri yolu **Azure::ServiceBus::BrokeredMessage** nesneleri ve bir standart özellikler kümesi sahip (gibi `label` ve `time_to_live`), bir uygulamaya özgü özel özellikleri tutmak için kullanılan bir sözlük ve rastgele uygulama verileri gövdesi içerir. Uygulamanın iletiyi olarak bir dize değeri geçirerek ileti gövdesini ayarlayabilir ve gerekli tüm standart özellikleri varsayılan değerlerle doldurulur.

Aşağıdaki örnek adlı sırasına sınama iletisi göndermek nasıl gösterir `test-queue` kullanarak `send_queue_message()`:

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="how-to-receive-messages-from-a-queue"></a>Kuyruktan ileti alma
İletileri kullanarak bir Sıraya alınan `receive_queue_message()` yöntemi **Azure::ServiceBusService** nesnesi. Varsayılan olarak, iletileri okumak ve sıradan silinir olmadan kilitli. Ayarlayarak okunduğu gibi ancak, ileti kuyruktan silebilirsiniz `:peek_lock` için seçenek **false**.

Varsayılan davranış, okuma ve ayrıca, iletilere uygulamaları desteklemek mümkün kılar bir iki aşamalı işlemi silme yapar. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya sonra işlemek için depoladıktan sonra), çağırarak alma işleminin ikinci aşamasını tamamlar `delete_queue_message()` yöntemi ve parametre olarak silinecek ileti sağlama. `delete_queue_message()` Yöntemi iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırır.

Varsa `:peek_lock` parametrenin ayarlanmış **yanlış**, okuma ve iletisi siliniyor basit model olur ve senaryoları bir uygulama içinde tolerans bir arıza olması durumunda bir ileti işlenirken değil en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Hizmet veri yolu ileti uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, kullanılan olarak işaretlenmiş olduğundan, çökmenin öncesinde kullanılan iletiyi atlamış olur.

Aşağıdaki örnek, kullanarak iletileri almak ve işlemek gösterilmiştir `receive_queue_message()`. Örnek ilk alır ve bir iletiyi kullanarak siler `:peek_lock` kümesine **false**, başka bir ileti alır ve kullanarak iletiyi siler `delete_queue_message()`:

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi herhangi bir nedenden dolayı işleyemedi sonra işleyememesi `unlock_queue_message()` yöntemi **Azure::ServiceBusService** nesnesi. Bu çağrı, Service Bus hizmetinin Kuyruktaki iletinin kilidini açmasına ve iletiyi aynı veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale getirmesine neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı vardır ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açar ve tekrar alınabilmesini sağlar kilit zaman aşımı dolmadan.

Uygulama iletiyi ancak önce çökmesi durumunda, `delete_queue_message()` yöntemi çağrıldıktan sonra yeniden başlatıldığında ileti uygulamaya tekrar teslim. Bu işlem genellikle adlı *en az bir kez işleme*; diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu genellikle kullanılarak elde edilen `message_id` iletinin teslimat denemelerinde özelliği.

## <a name="next-steps"></a>Sonraki adımlar
Artık Service Bus kuyruklarına ilişkin temel bilgileri öğrendiğinize göre, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* Genel Bakış [kuyruklar, konu başlıkları ve abonelikler](service-bus-queues-topics-subscriptions.md).
* Ziyaret [Ruby için Azure SDK](https://github.com/Azure/azure-sdk-for-ruby) github'daki.

Bu makalede ele alınan Azure Service Bus kuyrukları ve Azure ele sıraları arasında bir karşılaştırma için [ruby'den kuyruk depolama kullanma](../storage/queues/storage-ruby-how-to-use-queue-storage.md) makale için bkz: [Azure kuyrukları ve Karşılaştırılan ve Contrasted Azure hizmet veri yolu kuyrukları -](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

