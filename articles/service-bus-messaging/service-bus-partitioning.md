---
title: Bölümlenmiş Azure Service Bus kuyrukları ve konuları oluşturun | Microsoft Docs
description: Birden çok ileti aracıları kullanarak Service Bus kuyrukları ve konularından bölüm açıklar.
services: service-bus-messaging
author: sethmanheim
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 05/08/2016
ms.author: sethm
ms.openlocfilehash: 0759decec9d80f1f836110a8907049213ca1eed6
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="partitioned-queues-and-topics"></a>Bölümlenmiş kuyruklar ve konular

Azure Service Bus iletileri işlemek için birden çok ileti aracıları ve iletileri depolamak için birden çok Mesajlaşma deposu kullanır. Geleneksel kuyruk veya konu tek ileti aracısı tarafından işlenen ve bir Mesajlaşma deposunda depolanır. Hizmet veri yolu *bölümleri* kuyruklar ve konu başlıkları, etkinleştirmek veya *Mesajlaşma*, birden çok ileti aracıları ve mesajlaşma depoları arasında bölümlenecek. Bölümlendirme, genel üretilen işi bölümlenmiş bir varlığın tek ileti aracısı veya Mesajlaşma deposu performansını tarafından artık sınırlı olduğu anlamına gelir. Ayrıca, bir Mesajlaşma deposu geçici bir kesinti bölümlenmiş kuyruk veya konu kullanılamaz işlemez. Bölümlenmiş kuyrukları ve konularından işlemleri ve oturumlar için destek gibi tüm Gelişmiş Service Bus özellikler içerebilir.

Hizmet veri yolu dahili bileşenleri hakkında daha fazla bilgi için bkz: [Service Bus mimarisi] [ Service Bus architecture] makalesi.

> [!NOTE]
> Bölümlendirme, tüm kuyruklar ve konular temel veya standart SKU'ları, varlık oluşturulurken kullanılabilir. SKU Mesajlaşma Premium için kullanılabilir değildir, ancak tüm mevcut bölümlenen varlıklar Premium içinde beklendiği gibi çalışmaz.
 
Herhangi bir varolan kuyruk veya konu bölümleme seçeneğini değiştirmek mümkün değildir; varlık oluşturduğunuzda seçeneği yalnızca ayarlayabilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Her bölümlenmiş kuyruk veya konu birden çok parçalarını oluşur. Her parça farklı bir Mesajlaşma deposunda depolanır ve farklı ileti aracısı tarafından işlenir. Bölümlenmiş kuyruk veya konu için bir ileti gönderildiğinde, hizmet veri yolu ileti parçasının birine atar. Seçimi rastgele hizmet veri yolu veya gönderen belirtebilirsiniz bir bölüm anahtarı kullanılarak yapılır.

Bir istemci iletileri tüm parçaları bölümlenmiş bir kuyruktan ya da hizmet veri yolu sorguları bölümlenmiş bir konu için bir aboneliğe ilişkin bir ileti almaya istediğinde Mesajlaşma depoları hiçbirini alıcıya alınır ilk ileti döndürür. Diğer iletiler ve, aldığında ek döndürür Service Bus önbellekleri isteklerini alır. Bir alıcı istemci bölümlendirme farkında değildir; bölümlenmiş kuyruk veya konu istemciye yönelik davranışını (örneğin, okuma, tamamlamak, sahipsiz, erteleneceği prefetching) normal bir varlık davranışını aynıdır.

İleti gönderirken ya da bölümlenmiş kuyruk veya konu, bir mesaj alarak ek bir maliyeti yoktur.

## <a name="enable-partitioning"></a>Bölümlemeyi etkinleştir

Bölümlenmiş kuyrukları ve konularından Azure Service Bus ile kullanmak için Azure SDK'ın 2.2 veya sonraki bir sürümü veya belirtin `api-version=2013-10` ya da daha sonra HTTP istekleri.

### <a name="standard"></a>Standart

Standart Mesajlaşma katmanında Service Bus kuyrukları ve 1, 2, 3, 4 veya 5 GB boyutları (varsayılan 1 GB'dır) konularında oluşturabilirsiniz. Etkin bölümlendirme ile Service Bus varlık 16 kopyalarını (16 bölümler) için belirttiğiniz her GB oluşturur. Bu nedenle, 5 GB boyutunda bir sırasına oluşturursanız, en büyük sıra boyutu olan 16 bölümleri olur (5 \* 16) = 80 GB. Kendi girdisi bakarak bölümlenmiş kuyruk veya konu en büyük boyutunu görebilirsiniz [Azure portal][Azure portal], **genel bakış** dikey penceresinde bu varlık için.

### <a name="premium"></a>Premium

Premium katmanı ad alanında Service Bus kuyrukları ve 1, 2, 3, 4, 5, 10, 20, 40 veya 80 GB boyutları (varsayılan 1 GB'dır) konularında oluşturabilirsiniz. Varsayılan olarak etkin bölümlendirme ile Service Bus varlık başına iki bölüm oluşturur. Kendi girdisi bakarak bölümlenmiş kuyruk veya konu en büyük boyutunu görebilirsiniz [Azure portal][Azure portal], **genel bakış** dikey penceresinde bu varlık için.

Premium Mesajlaşma katmanında bölümleme hakkında daha fazla bilgi için bkz: [Service Bus Premium ve standart Mesajlaşma katmanları](service-bus-premium-messaging.md). 

### <a name="create-a-partitioned-entity"></a>Bölümlenmiş bir varlık oluştur

Bölümlenmiş kuyruk veya konu oluşturmanın birkaç yolu vardır. Uygulamanızdan kuyruk veya konu oluşturduğunuzda kuyruk veya konu için sırasıyla ayarlayarak bölümleme etkinleştirebilirsiniz [QueueDescription.EnablePartitioning] [ QueueDescription.EnablePartitioning] veya [TopicDescription.EnablePartitioning] [ TopicDescription.EnablePartitioning] özelliğine **doğru**. Bu özellikleri aynı anda sıranın ayarlanmalı veya konu oluşturulur. Daha önce belirtildiği gibi bu özellikleri olan bir sırayı veya konu değiştirmek mümkün değil. Örneğin:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Alternatif olarak, bölümlenmiş kuyruk veya konudaki oluşturabilirsiniz [Azure portal] [ Azure portal] veya Visual Studio'da. Portalda, kuyruk veya konu oluşturduğunuzda **bölümleme etkinleştirmek** kuyruk veya konu seçeneğinde **oluşturma** iletişim kutusu varsayılan olarak işaretli. Yalnızca standart katmanı varlıktaki bu seçenek devre dışı bırakabilirsiniz; Premium katmanındaki bölümleme her zaman etkindir. Visual Studio'da sırasıyla **etkinleştirmek bölümleme** onay kutusu **yeni kuyruk** veya **yeni konu** iletişim kutusu.

## <a name="use-of-partition-keys"></a>Bölüm anahtarlarını kullanımı
İleti sıraya alınan bölümlenmiş kuyruk veya konu içine olduğunda, hizmet veri yolu bir bölüm anahtarının varlığını denetler. Bulursa, bu anahtarı temel parça seçer. Bölüm anahtarı bulamazsa, bir iç algoritmadan yola çıkılarak parça seçer.

### <a name="using-a-partition-key"></a>Bir bölüm anahtarının kullanılması
Oturumları veya işlemleri gibi bazı senaryolar belirli parçadaki depolanması için iletileri gerektirir. Bu senaryolar bölüm anahtarı kullanılmasını gerektirir. Aynı bölüm anahtarı kullanan tüm iletiler için aynı parça atanır. Parça geçici olarak devre dışı ise, hizmet veri yolu bir hata döndürür.

Senaryoya bağlı olarak farklı ileti özellikleri bir bölüm anahtarı olarak kullanılır:

**SessionID**: bir ileti sahipse [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] özelliğini ayarlayın, sonra Service Bus, bu özellik bölüm anahtarı olarak kullanır. Bu şekilde aynı oturuma ait tüm iletileri aynı ileti aracısı tarafından işlenir. Oturumları oturum durumları tutarlılık yanı sıra sıralama ileti güvence altına almak Service Bus etkinleştirin.

**PartitionKey**: bir ileti sahipse [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] özelliği kullanmamayı [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] özelliğini ayarlayın, sonra Service Bus kullanan [PartitionKey] [ PartitionKey] özelliği bölüm anahtarı olarak. İletinin her ikisi de varsa [SessionID] [ SessionId] ve [PartitionKey] [ PartitionKey] özellikler kümesi, her iki özellik aynı olmalıdır. Varsa [PartitionKey] [ PartitionKey] özelliği farklı bir değere ayarlanmış [SessionID] [ SessionId] özelliği, hizmet veri yolu geçersiz işlemi özel durum döndürür. [PartitionKey] [ PartitionKey] özelliği, oturum olmayan farkında işlem iletileri gönderen gönderirse, kullanılmalıdır. Bölüm anahtarı, bir işlem içinde gönderilen tüm iletiler aynı Mesajlaşma aracısı tarafından işlenmesini sağlar.

**MessageID**: kuyruk veya konu varsa [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] özelliğini **true** ve [ BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] veya [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] özellikleri ayarlanmadı, sonra [ BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] özelliği, bölüm anahtarı olarak hizmet verir. (Gönderen uygulama yoksa Microsoft .NET ve AMQP kitaplıklarını otomatik olarak bir ileti kimliği atayın.) Bu durumda, aynı iletiyi tüm kopyalarını aynı ileti aracısı tarafından işlenir. Bu kimliği algılamak ve yinelenen iletileri ortadan kaldırmak hizmet veri yolu sağlar. Varsa [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] özelliği ayarlı değil **true**, Service Bus dikkate almaz [MessageID] [ MessageId] bölüm anahtarı olarak özelliği.

### <a name="not-using-a-partition-key"></a>Bir bölüm anahtarının kullanılması değil
Bölüm anahtarı olmaması durumunda, hizmet veri yolu ileti bölümlenmiş kuyruk veya konu tüm parçaları ile hepsini şekilde dağıtır. Seçilen parça kullanılabilir durumda değilse, hizmet veri yolu ileti için farklı bir parça atar. Bu şekilde, bir Mesajlaşma deposu geçici kullanılamama rağmen gönderme işlemi başarılı olur. Ancak, garantili bölüm anahtarı sağlayan sıralama elde.

Kullanılabilirlik (bölüm anahtarı) ve (bölüm anahtarı kullanarak) tutarlılık karşılaştırmasını daha ayrıntılı bir tartışma için bkz: [bu makalede](../event-hubs/event-hubs-availability-and-consistency.md). Bu bilgiler bölümlenmiş Service Bus varlıklar için eşit oranda geçerlidir.

Hizmet veri yolu vermek için yeterli kuyruğa ileti farklı bir parça zaman [MessagingFactorySettings.OperationTimeout] [ MessagingFactorySettings.OperationTimeout] ileti 15 saniyeden büyük olmalıdır gönderir istemci tarafından belirtilen değeri. Ayarladığınız önerilir [OperationTimeout] [ OperationTimeout] özelliğinin varsayılan değeri 60 saniye.

Bölüm anahtarı "belirli bir parça iletiye sabitler". Bu parça tutan Mesajlaşma deposu kullanılamıyorsa, hizmet veri yolu bir hata döndürür. Bölüm anahtarı olmadığında, Service Bus farklı bir parça seçebilir ve işlemi başarılı olur. Bu nedenle, gerekli olmadığı sürece bir bölüm anahtarı sağlamazsanız önerilir.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Gelişmiş konular: bölümlenmiş varlıklarıyla işlemleri kullanın
Bir işlem kapsamında gönderilen iletilerin bölüm anahtarını belirtmesi gerekir. Anahtar aşağıdaki özelliklerinden biri olabilir: [BrokeredMessage.SessionId][BrokeredMessage.SessionId], [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey], veya [ BrokeredMessage.MessageId][BrokeredMessage.MessageId]. Aynı işlem bir parçası olarak gönderilen tüm iletiler aynı bölüm anahtarı belirtmeniz gerekir. Bir işlem içinde bölüm anahtarı olmayan bir ileti göndermek çalışırsanız, hizmet veri yolu geçersiz işlemi özel durum döndürür. Aynı işlem içinde farklı bölüm anahtarlara sahip birden fazla ileti göndermeye, hizmet veri yolu geçersiz işlemi özel durum döndürür. Örneğin:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Bölüm anahtarı olarak hizmet özelliklerinden herhangi birini ayarlandıysa, hizmet veri yolu belirli bir parça iletiye sabitler. Bu davranış, bir işlem kullanılan olup olmadığına bakılmaksızın oluşur. Gerekli değilse bölüm anahtarı belirtmeyin önerilir.

## <a name="using-sessions-with-partitioned-entities"></a>Bölümlenmiş varlıklarıyla oturumları kullanma
Oturum durumunu algılayan konu veya sıra için işlem ileti göndermek için ileti olmalıdır [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] özellik kümesi. Varsa [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] özelliği de belirtilen, aynı olmalıdır [SessionID] [ SessionId] özelliği. Bunlar farklıysa, hizmet veri yolu geçersiz işlemi özel durum döndürür.

Normal (bölümlenmemiş) sıralar veya konuları farklı olarak, farklı oturumlar için birden çok ileti göndermek için tek bir işlem kullanmak mümkün değil. Bulamazsa, hizmet veri yolu geçersiz işlemi özel durum döndürür. Örneğin:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Bölümlenmiş varlıklarla otomatik ileti iletme
Service Bus otomatik ileti gelen ya da bölümlenen varlıklar arasında iletme destekler. Otomatik ileti iletmeyi etkinleştirmek için ayarlanmış [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] kaynak sırası veya Abonelik özelliği. İleti bir bölüm anahtarı belirtiyorsa ([SessionID][SessionId], [PartitionKey][PartitionKey], veya [MessageID][MessageId]), bu bölüm anahtarı hedef varlık için kullanılır.

## <a name="considerations-and-guidelines"></a>Dikkat edilecek noktalar ve yönergeleri
* **Yüksek tutarlılık özellikleri**: bir varlığı oturumları, yinelenen algılama veya bölümlendirme anahtarı açık denetim gibi özellikler kullanan sonra Mesajlaşma işlemleri belirli parçada her zaman yönlendirilecek. Herhangi bir parçasının yüksek trafik deneyimi veya alttaki deponun sağlam değil, bu işlemler başarısız ve kullanılabilirlik azalır. Genel olarak, tutarlılık bölümlenmemiş varlıklar hala çok daha yüksektir; yalnızca bir alt trafik tüm trafiği aksine sorunları yaşıyor. Daha fazla bilgi için bkz [kullanılabilirlik ve tutarlılık tartışma](../event-hubs/event-hubs-availability-and-consistency.md).
* **Yönetim**: oluşturma, güncelleştirme ve silme gibi işlemleri varlığın tüm parçaları üzerinde gerçekleştirilmesi gerekir. Herhangi bir parça sağlıksız ise, bu işlemler için hataları sonuçlanabilir. İleti sayısı gibi alma işlemi için bilgi tüm parçaları toplanması gerekir. Herhangi bir parça sağlıksız ise, varlık kullanılabilirlik durumunu sınırlı raporlanır.
* **Düşük birim ileti senaryosu**: Bu senaryolara, özellikle HTTP protokolünü kullanarak birden çok gerçekleştirmeniz gerekebilir tüm iletileri almak için alma işlemleri. Alma istekleri için ön uç üzerinde tüm parçaları alma gerçekleştirir ve alınan tüm yanıtlarını önbelleğe kaydeder. Aynı bağlantı üzerinde bir sonraki alma isteği ve yararlanan bu önbelleğe alınan alma gecikme daha düşük olacaktır. Ancak, birden çok bağlantınız veya HTTP kullanıyorsanız, her istek için yeni bir bağlantı kurar. Bu nedenle, aynı düğümde güden hiçbir garantisi yoktur. Tüm mevcut iletiler kilitli ve içinde başka bir ön uç önbelleğe varsa, alma işlemi döndürür **null**. İletileri sonunda süresi dolacak ve yeniden alırsınız. HTTP Etkin tutmayı önerilir.
* **Gözat/gözlem iletileri**: [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) her zaman belirtilen ileti sayısını döndürmüyor [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription.messagecount) özelliği. Bu davranış için iki ortak nedenleri vardır. İletileri koleksiyonu toplanmış boyutu 256 KB en fazla boyutu aşıyor bir nedenidir. Kuyruk veya konu varsa olan başka bir nedenle [EnablePartitioning özelliğinin](/dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning) kümesine **doğru**, bir bölüm iletileri istenen sayıda tamamlamak için yeterli iletileri olmayabilir. Bir uygulama belirli sayıda ileti almak istiyorsa, genel olarak, çağırmalıdır [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) sürekli olarak bu ileti sayısını alır veya atmaya daha fazla ileti yok kadar. Kod örnekleri dahil olmak üzere daha fazla bilgi için bkz: [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch) veya [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient.peekbatch) API belgeleri.

## <a name="latest-added-features"></a>En son eklenen özellikler
* Ekleme veya kaldırma kural bölümlenmiş varlıklarıyla artık desteklenmektedir. Bölümlenmiş olmayan varlıklar farklıdır, bu işlemler altında işlemleri desteklenmez. 
* AMQP, gönderme ve alma ve ileti bölümlenmiş bir varlıktan artık desteklenmektedir.
* AMQP aşağıdaki işlemler için artık desteklenmektedir: [toplu iş gönderme](/dotnet/api/microsoft.servicebus.messaging.queueclient.sendbatch), [toplu alma](/dotnet/api/microsoft.servicebus.messaging.queueclient.receivebatch), [alma sıra numarası tarafından](/dotnet/api/microsoft.servicebus.messaging.queueclient.receive), [Peek](/dotnet/api/microsoft.servicebus.messaging.queueclient.peek), [yenileme kilit](/dotnet/api/microsoft.servicebus.messaging.queueclient.renewmessagelock), [zamanlama ileti](/dotnet/api/microsoft.servicebus.messaging.queueclient.schedulemessageasync), [iptal zamanlanmış iletisi](/dotnet/api/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync), [Kuralı Ekle](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [kuralı kaldırmak](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [oturum yenileme kilit](/dotnet/api/microsoft.servicebus.messaging.messagesession.renewlock), [kümesi oturum durumu](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate), [Get oturum durumu](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate), ve [listeleme oturumları](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions).

## <a name="partitioned-entities-limitations"></a>Bölümlenen varlıklar sınırlamaları
Şu anda hizmet veri yolu bölümlenmiş kuyrukları ve konularından aşağıdaki sınırlamalar getirir:

* Bölümlenmiş kuyruklar ve konu başlıkları tek bir işlemde farklı oturumlar ait iletileri gönderme desteklemez.
* Service Bus şu anda ad alanı başına en çok 100 bölümlenmiş kuyruğa veya konuya izin vermektedir. Ad alanı (Premium katmanı için geçerli değildir) başına 10.000 varlık kota doğrultusunda her bölümlenmiş kuyruk veya konu sayar.

## <a name="next-steps"></a>Sonraki adımlar
AMQP 1.0 belirtiminde Mesajlaşma temel kavramlar hakkında bilgi [AMQP 1.0 protokolü Kılavuzu](service-bus-amqp-protocol-guide.md).

[Service Bus architecture]: service-bus-architecture.md
[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablepartitioning
[BrokeredMessage.SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid
[BrokeredMessage.PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.partitionkey
[SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.partitionkey
[QueueDescription.RequiresDuplicateDetection]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid
[MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid
[MessagingFactorySettings.OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
