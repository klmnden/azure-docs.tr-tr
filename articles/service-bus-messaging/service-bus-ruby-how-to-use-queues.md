---
title: Ruby ile Azure Service Bus kuyruklarını kullanma | Microsoft Docs
description: Azure'da Service Bus kuyruklarını kullanmayı öğrenin. Ruby'de yazılan kod örneklerini içerir.
services: service-bus-messaging
documentationcenter: ruby
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 0a11eab2-823f-4cc7-842b-fbbe0f953751
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 04/10/2019
ms.author: aschhab
ms.openlocfilehash: 48f60b7c07cc16b4d9994d5644069fdcb4881e0a
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991870"
---
# <a name="how-to-use-service-bus-queues-with-ruby"></a>Ruby ile Service Bus kuyruklarını kullanma

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu öğreticide, bir Service Bus kuyruğundaki iletileri alıp ileti göndermek için Ruby uygulamalarının nasıl oluşturulacağını öğrenin. Örnekler, Ruby'de yazılan ve Azure gem kullanır.

## <a name="prerequisites"></a>Önkoşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Etkinleştirebilir, [MSDN abone Avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) veya kaydolun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. İzleyeceğiniz adımlar [Service Bus kuyruğuna oluşturmak için Azure portalını kullanın](service-bus-quickstart-portal.md) makalesi.
    1. Hızlı Okuma **genel bakış** Service Bus **kuyrukları**. 
    2. Hizmet veri yolu oluşturma **ad alanı**. 
    3. Alma **bağlantı dizesi**. 

        > [!NOTE]
        > Oluşturacağınız bir **kuyruk** Bu öğreticide Ruby kullanarak Service Bus ad alanında. 

[!INCLUDE [service-bus-ruby-setup](../../includes/service-bus-ruby-setup.md)]

## <a name="how-to-create-a-queue"></a>Bir kuyruk oluşturma
**Azure::ServiceBusService** nesnesi kuyrukları ile çalışmanıza olanak sağlar. Bir kuyruk oluşturmak için kullanın `create_queue()` yöntemi. Aşağıdaki örnek, bir sıra oluşturur veya tüm hataları yazdırır.

```ruby
azure_service_bus_service = Azure::ServiceBus::ServiceBusService.new(sb_host, { signer: signer})
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

De geçirebilirsiniz bir **Azure::ServiceBus::Queue** ek seçeneklere nesne, Canlı ya da en büyük sıra boyutu ileti süresi gibi varsayılan kuyruk ayarlarını geçersiz kılmanıza olanak sağlar. Aşağıdaki örnek, en büyük sıra boyutu 5 GB ve saat 1 dakika için dinamik olarak ayarlamak gösterilmektedir:

```ruby
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Bir kuyruğa ileti göndermek nasıl
Uygulama çağrılarınızı, bir Service Bus kuyruğuna bir ileti göndermek için `send_queue_message()` metodunda **Azure::ServiceBusService** nesne. Gönderilen (ve öğesinden alınan) hizmet sıraları Bus iletileri **Azure::ServiceBus::BrokeredMessage** nesneleri ve yüklü bir standart özellikler kümesi (gibi `label` ve `time_to_live`), özel tutmak için kullanılan bir sözlüğü uygulamaya özgü özellikler ve rastgele uygulama verileri gövdesi. Bir uygulama iletisi olarak bir dize değeri geçirerek ileti gövdesini ayarlayabilirsiniz ve gerekli tüm standart özellikleri varsayılan değerlerle doldurulur.

Aşağıdaki örnekte adlı kuyruk için bir test iletisi göndermek nasıl gösterir `test-queue` kullanarak `send_queue_message()`:

```ruby
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="how-to-receive-messages-from-a-queue"></a>Kuyruktan ileti alma
İletilerin kullanarak bir sıraya alındığı `receive_queue_message()` metodunda **Azure::ServiceBusService** nesne. Varsayılan olarak, iletileri okuyun ve sıradan silinir olmadan kilitli. Ayarlayarak okunurlar ancak, iletileri kuyruktan silebilirsiniz `:peek_lock` seçeneğini **false**.

Varsayılan davranışı, okuma ve silme ayrıca, atlanan iletilere uygulamaları desteklemek mümkün kılar bir iki aşamalı işlemi yapar. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya güvenilir bir şekilde işlemek üzere depolar sonra) çağırarak alma işleminin ikinci aşamasını tamamlar `delete_queue_message()` yöntemi ve iletiyi bir parametre olarak silinmesini sağlar. `delete_queue_message()` Yöntemi iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırın.

Varsa `:peek_lock` parametrenin ayarlanmış **false**, okuma ve ileti silme basit model haline gelir ve içinde bir uygulama tolere edebilen bir arıza olması durumunda bir iletiyi işlememeye izin senaryolarda en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletisi, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, Tüketilmekte olan olarak işaretlediğinden çökmenin öncesinde kullanılan iletiyi atlamış olur.

Aşağıdaki örneği kullanarak iletileri almak ve işlemek nasıl gösterir `receive_queue_message()`. Örneğin ilk alır ve bir iletiyi kullanarak siler `:peek_lock` kümesine **false**, başka bir iletiyi alır ve ardından iletiyi kullanarak siler `delete_queue_message()`:

```ruby
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi işlemek için herhangi bir nedenle silemiyor sonra çağırabilirsiniz `unlock_queue_message()` metodunda **Azure::ServiceBusService** nesne. Bu çağrı Bus hizmetinin Kuyruktaki iletinin kilidini açmasına ve iletiyi aynı kullanıcı uygulama veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı yoktur ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açar ve alınabilmesini kilit zaman aşımı dolmadan tekrar kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, `delete_queue_message()` yöntemi çağrılır, ardından yeniden başlatıldığında ileti uygulamaya yeniden teslim. Bu işlem genellikle çağrılırken *en az bir kez işleme*; diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu genellikle kullanılmasıdır `message_id` özelliğini iletinin teslim denemeleri arasında sabit kalır.

> [!NOTE]
> Service Bus kaynakları ile yönetebileceğiniz [hizmet veri yolu Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer/). Hizmet veri yolu Gezgini, bir Service Bus ad alanınıza bağlanın ve mesajlaşma varlıkları kolay bir şekilde yönetmek kullanıcıların sağlar. Araç, içeri/dışarı aktarma işlevleri veya konu, kuyruklar, abonelikler, geçiş hizmetleri, bildirim hub'ları ve olay hub'ları test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Artık Service Bus kuyruklarına ilişkin temel bilgileri öğrendiğinize göre, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* Genel Bakış [kuyruklar, konular ve abonelikler](service-bus-queues-topics-subscriptions.md).
* Ziyaret [Ruby için Azure SDK'sı](https://github.com/Azure/azure-sdk-for-ruby) github deposu.

Bu makalede ele alınan Azure Service Bus kuyrukları ve Azure kuyrukları ele arasında bir karşılaştırma için [ruby'den kuyruk depolama kullanma](../storage/queues/storage-ruby-how-to-use-queue-storage.md) makale için bkz: [Azure kuyrukları ve Azure hizmet veri yolu kuyrukları - karşılaştırıldığında ve Karşıtlıklar](service-bus-azure-and-service-bus-queues-compared-contrasted.md)

