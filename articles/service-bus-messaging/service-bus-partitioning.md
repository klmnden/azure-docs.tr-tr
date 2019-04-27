---
title: Bölümlenmiş bir Azure Service Bus kuyrukları ve konuları oluşturun | Microsoft Docs
description: Birden çok ileti aracıları kullanarak Service Bus kuyrukları ve konuları bölüm açıklar.
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 02/06/2019
ms.author: aschhab
ms.openlocfilehash: 699581c7ccd3f36da0cd0c1def623607b7c0a13b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60589672"
---
# <a name="partitioned-queues-and-topics"></a>Bölümlenmiş kuyruklar ve konular

Azure Service Bus iletileri işlemek üzere birden çok ileti aracıları ve iletileri depolamak için birden çok Mesajlaşma deposu kullanır. Geleneksel bir kuyruk veya konuda tek ileti aracısı tarafından işlenmesini ve bir Mesajlaşma deposunda depolanır. Service Bus *bölümleri* kuyrukları ve konuları etkinleştirmek veya *Mesajlaşma*birden çok ileti aracıları ve ileti depoları arasında yeniden bölümlenmesi için. Bölümleme bölümlenen bir varlığın genel verimini tek ileti aracısı veya ileti deposu performansını tarafından artık sınırlı olduğu anlamına gelir. Ayrıca, bir Mesajlaşma deposunun geçici bir kesinti bölümlenmiş bir kuyruk veya konuda kullanılamaz işlemez. Bölümlenmiş kuyruklar ve konular işlemleri ve oturumları desteği gibi tüm Gelişmiş Service Bus özellikleri içerebilir.

> [!NOTE]
> Bölümleme tüm kuyruklar ve konular temel veya standart SKU'lar için varlık oluşturulurken kullanılabilir. Premium Mesajlaşma SKU'su kullanılamıyor ancak daha önce mevcut tüm bölümlenen varlıklar Premium ad alanlarında beklendiği gibi çalışmaya devam.
 
Herhangi bir mevcut kuyruk veya konuda bölümleme seçeneğini değiştirmek mümkün değildir; yalnızca varlık oluşturma seçeneğini de ayarlayabilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Her bölümlenmiş bir kuyruk veya konu başlığı birden çok bölüm içerir. Her bölüm farklı bir Mesajlaşma deposunda depolanır ve farklı ileti aracısı tarafından işlenir. Bir bölümlenmiş kuyruğa veya konuya ileti gönderildiğinde, Service Bus ileti bölümlerden birine atar. Seçimi rastgele Service Bus veya gönderen belirtebilirsiniz bir bölüm anahtarı kullanılarak gerçekleştirilir.

Ardından tüm bölümleri iletiler için bir ileti bölümlenmiş bir kuyruk veya Service Bus sorguları bölümlenmiş bir konu için bir abonelik almak bir istemci istediği zaman, tüm Mesajlaşma depoları, alıcıya elde edilen ilk iletiyi döndürür. Diğer iletiler ve aldığında ek döndürür Service Bus önbellekler isteklerini alır. Bir alıcı istemci bölümleme uyumlu değil; bölümlenmiş bir kuyruk veya konuda istemciye yönelik davranışını (örneğin, okuma, tamamlamak, erteleme, teslim edilemeyen iletiler, önceden getiriliyor) normal bir varlık davranışını aynıdır.

İleti gönderme veya bir bölümlenmiş bir kuyruk veya konuda, bir iletiyi alan hiçbir ek ücret yoktur.

## <a name="enable-partitioning"></a>Bölümlemeyi etkinleştir

Bölümlenmiş kuyruklar ve konular, Azure Service Bus ile kullanmak için Azure SDK'sını 2.2 veya sonraki bir sürümü belirtin veya `api-version=2013-10` veya daha sonra HTTP istekleri.

### <a name="standard"></a>Standart

Standart Mesajlaşma katmanına, Service Bus kuyrukları ve konuları 1, 2, 3, 4 veya 5 GB'lık boyutları (varsayılan 1 GB'tır) oluşturabilirsiniz. Etkin bölümleme ile Service Bus varlık her biri belirtilen aynı boyutu 16 kopyalarını (16 bölümler) oluşturur. Bu nedenle, 5 GB boyutundadır bir kuyruk oluşturmak, en büyük sıra boyutu 16 bölümlerle olur (5 \* 16) = 80 GB. Bölümlenmiş bir kuyruk veya konuda en büyük boyutunu, girdi bakarak gördüğünüz [Azure portalında][Azure portal], **genel bakış** dikey penceresinde söz konusu varlığa ilişkin.

### <a name="premium"></a>Premium

Premium katman ad alanında bölümleme varlıkları desteklenmez. Bununla birlikte, yine de Service Bus kuyrukları ve konuları 1, 2, 3, 4, 5, 10, 20, 40 veya 80 GB boyutları (varsayılan 1 GB'tır) oluşturabilirsiniz. Kuyruk veya konu başlığı boyutu, girdi bakarak gördüğünüz [Azure portalında][Azure portal], **genel bakış** dikey penceresinde söz konusu varlığa ilişkin.

### <a name="create-a-partitioned-entity"></a>Bölümlenmiş bir varlık oluşturma

Bölümlenmiş bir kuyruk veya konuda oluşturmanın birkaç yolu vardır. Uygulamanızdan bir kuyruk veya konu oluşturduğunuzda, kuyruk veya konu başlığı için sırasıyla ayarlayarak bölümleme etkinleştirebilirsiniz [QueueDescription.EnablePartitioning] [ QueueDescription.EnablePartitioning] veya [ TopicDescription.EnablePartitioning] [ TopicDescription.EnablePartitioning] özelliğini **true**. Bu özellikler kuyruk veya konuda oluşturulduğu zaman ve yalnızca eski kullanılabilir ayarlanmalıdır [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) kitaplığı. Daha önce belirtildiği gibi bu özellikleri bir var olan bir kuyruk veya konuda değiştirmek mümkün değildir. Örneğin:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Alternatif olarak, bölümlenmiş bir kuyruk veya konuda oluşturabilirsiniz [Azure portalında][Azure portal]. Portalda bir kuyruk veya konuda oluşturduğunuzda **bölümlemeyi etkinleştir** kuyruğun veya konunun seçeneğinde **Oluştur** iletişim kutusu varsayılan olarak işaretli. Yalnızca standart katman varlığındaki bu seçeneği devre dışı bırakabilirsiniz; Premium katmanda bölümleme desteklenmiyor ve onay kutusunu etkisi yoktur. 

## <a name="use-of-partition-keys"></a>Bölüm anahtarları kullanma

Bölümlenmiş bir kuyruk veya konuda sıraya bir ileti olduğunda, hizmet veri yolu bir bölüm anahtarı olup olmadığını denetler. Bulursa, bu anahtarı temel bölüm seçer. Bir bölüm anahtarı bulamazsa, bir iç algoritmaya bağlı bağlı bölüm seçer.

### <a name="using-a-partition-key"></a>Bir bölüm anahtarının kullanılması

Oturumlar veya işlemler, gibi bazı senaryolarda belirli bir bölümünde depolanacak iletileri gerektirir. Tüm bu senaryolar, bir bölüm anahtarı kullanılmasını gerektirir. Aynı bölüm anahtarı kullanan tüm iletiler aynı bölüme atanır. Bölüm geçici olarak kullanılamıyorsa, Service Bus bir hata döndürür.

Senaryoya bağlı olarak farklı ileti özellikleri bir bölüm anahtarı olarak kullanılır:

**SessionID**: Bir ileti sahipse [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid) özelliğini ayarlayın ve ardından Service Bus kullanan **SessionID** bölüm anahtarı olarak. Bu şekilde aynı oturumuna ait tüm iletiler aynı ileti aracısı tarafından işlenir. Mesaj sıralama sistemi yanı sıra oturum durumları tutarlılığını sağlamak Service Bus oturumları etkinleştir.

**PartitionKey**: Bir ileti sahipse [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özelliği kullanmamayı [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid) özelliğini ayarlayın ve ardından Service Bus kullanan [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) bölüm anahtarı olarak özellik değeri. İletinin her ikisi de varsa [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid) ve [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özellikler kümesi, her iki özellik aynı olmalıdır. Varsa [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özelliği, farklı bir değere ayarlanır [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid) özelliği, hizmet veri yolu bir geçersiz işlem özel durum verir. [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) oturum olmayan kullanan işlem iletileri gönderen gönderirse, özellik'in kullanılması gerekir. Bölüm anahtarı, bir işlem içinde gönderilen tüm iletiler aynı ileti aracısı tarafından işlenmesini sağlar.

**MessageID**: Kuyruk veya konu varsa [RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) özelliğini **true** ve [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid) veya [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özellikleri ayarlanmadı, ardından [MessageID](/dotnet/api/microsoft.azure.servicebus.message.messageid) özellik değeri, bölüm anahtarı olarak hizmet verir. (Gönderen uygulama daha önceden yoksa Microsoft .NET ve AMQP kitaplıklarını otomatik olarak bir ileti kimliği atayın.) Bu durumda, aynı iletinin tüm kopyaları aynı ileti aracısı tarafından işlenir. Service Bus'ın algılamak ve yinelenen iletileri ortadan kaldırmak bu kimliği sağlar. Varsa [RequiresDuplicateDetection](/dotnet/api/microsoft.azure.management.servicebus.models.sbqueue.requiresduplicatedetection) özelliği ayarlanmamış **true**, Service Bus dikkate [MessageID](/dotnet/api/microsoft.azure.servicebus.message.messageid) bölüm anahtarı olarak özelliği.

### <a name="not-using-a-partition-key"></a>Bir bölüm anahtarının kullanılması değil

Bir bölüm anahtarı olmaması durumunda, Service Bus iletileri ettirirsiniz bölümlenmiş bir kuyruk veya konuda tüm bölümler için dağıtır. Seçilen bölümün kullanılabilir durumda değilse, Service Bus ileti için farklı bir bölüm atar. Bu şekilde, geçici olarak kullanım dışı kalması için Mesajlaşma deposunun rağmen gönderme işlemi başarılı olur. Ancak, garantili bir bölüm anahtarı sağlayan sıralama elde etmez.

Kullanılabilirlik (bölüm anahtarı) ve tutarlılık (bir bölüm anahtarının kullanılması) arasındaki daha ayrıntılı bir tartışma için bkz: [bu makalede](../event-hubs/event-hubs-availability-and-consistency.md). Bu bilgiler, bölümlenmiş Service Bus varlıklarına eşit oranda geçerlidir.

Service Bus vermek için yeterli kuyruğa ileti farklı bir bölüme, zaman [OperationTimeout](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) ileti 15 saniyeden daha büyük olmalıdır gönderen istemci tarafından belirtilen değeri. Ayarladığınız önerilir [OperationTimeout](/dotnet/api/microsoft.azure.servicebus.queueclient.operationtimeout) özelliğini varsayılan değer 60 saniye.

Bir bölüm anahtarı "belirli bir bölüme bir ileti sabitler". Bu bölüm tutan ileti deposuna kullanılamıyorsa, Service Bus bir hata döndürür. Bir bölüm anahtarı olmaması, Service Bus, farklı bir bölüm seçebilirsiniz ve işlemi başarılı olur. Bu nedenle, gerekli olmadığı sürece, bir bölüm anahtarı vermezsiniz önerilir.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Gelişmiş konular: bölümlenen varlıklar ile işlemleri kullanma

Bir işlem kapsamında gönderilen iletilerin bölüm anahtarını belirtmesi gerekir. Anahtar, aşağıdaki özelliklerden biri olabilir: [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey), veya [MessageID](/dotnet/api/microsoft.azure.servicebus.message.messageid). Aynı işlem kapsamında gönderilen tüm iletiler aynı bölüm anahtarı belirtmeniz gerekir. Bir işlem içinde bölüm anahtarı olmayan bir ileti göndermeye, Service Bus bir geçersiz işlem özel durum verir. Farklı bölüm anahtarları aynı işlem içinde birden çok ileti göndermeye, hizmet veri yolu bir geçersiz işlem özel durum verir. Örneğin:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    Message msg = new Message("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.SendAsync(msg); 
    ts.CompleteAsync();
}
committableTransaction.Commit();
```

Bir bölüm anahtarı olarak hizmet özellikleri olarak ayarlamış ise Service Bus ileti belirli bir bölüme sabitler. Bu davranış, bir işlem kullanılır olup olmadığını oluşur. Gerekli değilse bölüm anahtarı belirtmeyin önerilir.

## <a name="using-sessions-with-partitioned-entities"></a>Bölümlenen varlıklar ile oturumlarını kullanma

İşlem ileti bir oturuma kullanan bir konu veya kuyruğa göndermek için ileti olmalıdır [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid) özellik kümesi. Varsa [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey) özelliği de belirtilirse, aynı olmalıdır [SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid) özelliği. Bunlar farklıysa, hizmet veri yolu bir geçersiz işlem özel durum verir.

Normal (bölümlenmemiş) kuyruklar veya konular aksine, farklı oturumlar için birden çok ileti göndermek için tek bir işlem kullanmak mümkün değildir. Service Bus, yüklemeye çalışırsanız, bir geçersiz işlem özel durum döndürür. Örneğin:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    Message msg = new Message("This is a message");
    msg.SessionId = "mySession";
    messageSender.SendAsync(msg); 
    ts.CompleteAsync();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Bölümlenen varlıklar ile otomatik ileti yönlendirmeyi

Service Bus, için ya da bölümlenen varlıklar arasında iletme otomatik ileti destekler. Otomatik ileti yönlendirmeyi etkinleştirmek için [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] kaynak kuyruk veya Abonelik özelliği. İleti bir bölüm anahtarı belirtiyorsa ([SessionID](/dotnet/api/microsoft.azure.servicebus.message.sessionid), [PartitionKey](/dotnet/api/microsoft.azure.servicebus.message.partitionkey), veya [MessageID](/dotnet/api/microsoft.azure.servicebus.message.messageid)), bu bölüm anahtarı hedef varlık için kullanılır.

## <a name="considerations-and-guidelines"></a>Önemli noktalar ve yönergeleri
* **Yüksek tutarlılık özellikleri**: Bir varlık oturumları, yinelenen algılama veya bölümleme anahtarı, açık denetim gibi özellikleri kullanıyorsa, Mesajlaşma işlemleri her zaman belirli bir bölüme yönlendirilir. Yüksek trafik bölümlerden hiçbirine deneyimi veya alttaki deponun sağlam değil, bu işlemleri başarısız ve kullanılabilirlik azalır. Genel olarak, tutarlılık bölümlenmemiş varlıkları yine de çok daha yüksektir; trafiğin yalnızca bir alt tüm trafiği aksine sorunları yaşıyor. Daha fazla bilgi için bkz. Bu [tartışma kullanılabilirlik ve tutarlılık](../event-hubs/event-hubs-availability-and-consistency.md).
* **Yönetim**: Varlığın tüm bölümleri üzerinde oluşturma, güncelleştirme ve silme gibi işlemleri yapılması gerekir. Herhangi bir bölümü iyi durumda olmayan ise, bu işlemler için hataları sonuçlanabilir. İleti sayısı gibi tüm bölümleri alma işlemi için bilgi toplanmalıdır. Varlık kullanılabilirlik durumunu, herhangi bir bölümü iyi durumda olmayan ise, sınırlı bildirilir.
* **Birim ileti senaryoları düşük**: Bu senaryolara, HTTP protokolünü kullanırken özellikle birden çok yapmak zorunda kalabilir tüm iletileri almak için alma işlemleri. Alma istekleri için ön uç üzerinde tüm bölümleri alma gerçekleştirir ve alınan tüm yanıtlarını önbelleğe kaydeder. Aynı bağlantıda bir sonraki alma isteği ve avantaj bu önbelleğe alınan alma gecikme, daha düşük olacaktır. Ancak, birden çok bağlantı veya HTTP kullanırsanız, her istek için yeni bir bağlantı kurar. Bu nedenle, aynı düğümde kavuşmak bir garanti yoktur. Alma işlemi var olan tüm iletileri kilitlidir ve başka bir ön uç önbelleğe alınmış varsa, döndürür **null**. Sonunda iletilerin süresi dolar ve bunları yeniden alabilir. HTTP Etkin tutmayı önerilir.
* **Göz atma/gözlem iletileri**: Yalnızca, eski [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) kitaplığı. [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) her zaman belirtilen ileti sayısını döndürmeyen [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.messagecount) özelliği. Bu davranış için sık karşılaşılan iki nedeni vardır. İletileri koleksiyonu toplanmış boyutu 256 KB'lık boyut sınırını aşıyor, bir nedenidir. Kuyruk veya konu varsa olan başka bir nedenle [EnablePartitioning özelliğinin](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning) kümesine **true**, bir bölümün istenen sayıda ileti tamamlamak için yeterli iletileri olmayabilir. Genel olarak, bir uygulama belirli sayıda ileti almak istiyorsa, çağırmalıdır [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) kadar sürekli olarak bu iletilerin sayısını alır veya göz atmak için daha fazla ileti yok. Kod örnekleri dahil olmak üzere daha fazla bilgi için bkz. [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) veya [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient.peekbatch) API belgeleri.

## <a name="latest-added-features"></a>En son eklenen özellikler

* Ekleme veya kaldırma kural bölümlenen varlıklar ile artık desteklenmektedir. Bölümlenmemiş varlıkları farklıdır, bu işlemler altında işlemler desteklenmez. 
* AMQP ve ileti gönderip bölümlenmiş bir varlığın artık destekleniyor.
* AMQP, şu işlemler için artık desteklenmektedir: [Toplu gönderme](/dotnet/api/microsoft.servicebus.messaging.queueclient.sendbatch), [Batch alma](/dotnet/api/microsoft.servicebus.messaging.queueclient.receivebatch), [seri numarasına göre alma](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive), [Özet](/dotnet/api/microsoft.servicebus.messaging.queueclient.peek), [kilidi yenileme](/dotnet/api/microsoft.servicebus.messaging.queueclient.renewmessagelock), [zamanlaması İleti](/dotnet/api/microsoft.azure.servicebus.queueclient.schedulemessageasync), [zamanlanmış iletiyi iptal](/dotnet/api/microsoft.azure.servicebus.queueclient.cancelscheduledmessageasync), [Kuralı Ekle](/dotnet/api/microsoft.azure.servicebus.ruledescription), [kuralı kaldırmak](/dotnet/api/microsoft.azure.servicebus.ruledescription), [oturum kilidi yenileme](/dotnet/api/microsoft.servicebus.messaging.messagesession.renewlock), [ Küme oturum durumu](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate), [alma oturum durumu](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate), ve [oturumları listeleme](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions).

## <a name="partitioned-entities-limitations"></a>Bölümlenen varlıklar sınırlamaları

Şu anda Service Bus, bölümlenmiş kuyruklar ve konular aşağıdaki kısıtlamaları getirir:

* Bölümlenmiş kuyruklar ve konular Premium Mesajlaşma katmanında desteklenmiyor. Oturumlarının, oturum kimliği kullanarak premier katmanda desteklenir. 
* Bölümlenmiş kuyruklar ve konular, farklı oturumlarında tek bir işlemde ait iletileri gönderme desteklemez.
* Service Bus şu anda ad alanı başına en çok 100 bölümlenmiş kuyruğa veya konuya izin vermektedir. Kota (Premium katmanı için geçerli değildir) ad alanı başına 10.000 varlıkların doğrultusunda her bölümlenmiş bir kuyruk veya konuda sayar.

## <a name="next-steps"></a>Sonraki adımlar

AMQP 1.0 belirtimi Mesajlaşma ilgili temel kavramları hakkında bilgi edinin [AMQP 1.0 protokol Kılavuzu](service-bus-amqp-protocol-guide.md).

[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablepartitioning
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
