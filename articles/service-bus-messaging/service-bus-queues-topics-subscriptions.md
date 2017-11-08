---
title: "Azure Service Bus kuyrukları, konu başlıkları ve abonelikleri Mesajlaşma genel bakış | Microsoft Docs"
description: "Service Bus Mesajlaşma genel bakış."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a306ced4-74e9-47c6-990a-d9c47efa31d5
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2017
ms.author: sethm
ms.openlocfilehash: 5bea3b56cea81362b25e696a672bf2a00e26d3ef
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus kuyrukları, konu başlıkları ve abonelikleri

Microsoft Azure Service Bus güvenilir message queuing bulut tabanlı, ileti odaklı Ara teknolojilerini ve dayanıklı yayımlama/Mesajlaşma abonelik, bir kümesini destekler. Bu "aracılı" Mesajlaşma işlevleri, ardından destek yayımlama-abone olma Mesajlaşma özellikleri, zamana bağlı ayırma ve Yük Dengeleme Service Bus Mesajlaşma iş yükü kullanarak ayrılmış olarak düşünülebilir. Ayrılmış iletişim birçok avantaj sunar. Örneğin, sunucular ve istemciler gerektiğinde bağlantı kurabilir ve kendi işlemlerini zaman uyumsuz olarak gerçekleştirebilir.

Hizmet veri yolundaki özünü Mesajlaşma özelliklerini Mesajlaşma varlıkları kuyruklar, konuları ve abonelikleri ve kuralları/eylemleri ' dir.

## <a name="queues"></a>Kuyruklar

Kuyruklar teklif *ilk gelen, giden ilk* (FIFO yöntemine göre) ileti teslimi bir veya birden çok rakip tüketiciye. Diğer bir deyişle, iletiler genellikle alınan ve hangi kuyruğa eklendikleri ve her ileti alındı ve yalnızca bir ileti tüketicisi tarafından işlenen bir düzende alıcılar tarafından işlenen beklenir. Kuyrukları kullanmanın önemli bir avantajı "zamana bağlı ayırma" uygulama bileşenlerinin elde etmektir. İletileri işlemi kuyrukta depolandığından diğer bir deyişle, üreticiler (göndericiler) ve tüketicilerin (Alıcılar) aynı anda ileti alma ve gönderme gerekmez. Ayrıca, üretici işlemek ve iletileri göndermek devam etmek için bir yanıt beklerken yok.

", Üreticilerin ve tüketicilerin iletileri farklı hızlarda gönderip almasına sağlayan Yük Dengeleme" bir avantaj ise. Birçok uygulamada, sistem yükünün zaman içinde değişir; Bununla birlikte, her iş birimi için gereken işleme süresi genellikle sabittir. Aracılığıyla ileti üreticileri ve tüketicileri bir kuyruk, kullanıcı uygulamanın yalnızca ortalama yük yoğun yük yerine işlemek için kullanılacak olduğunu anlamına gelir. Gelen yük hacmi değiştikçe kuyruğun derinliği artar ve daralır. Bu, doğrudan para uygulama yükünü sunmak için gereken altyapı miktarı ile kaydeder. Yük arttıkça kuyruktan okunmak üzere daha fazla çalışan işlemi eklenebilir. Her ileti yalnızca bir çalışan işlemi tarafından işlenir. Ayrıca, çalışan bilgisayarlar işleme gücünü göre farklılık gösterse bile bunların iletileri kendi maksimum hızında çekme olarak çalışan bilgisayarlar için en iyi kullanımı bu çekme tabanlı yük dengeleme sağlar. Bu desen genellikle "tüketici rekabet" düzeni olarak adlandırılır.

İleti üreticileri ve tüketicileri arasında ara için kuyrukları kullanma bileşenleri arasındaki yapısında bir gevşek bağlantı sağlar. Üreticileri ve tüketicileri birbirinden farkında değildir çünkü bir tüketici üretici üzerinde hiçbir etkisi olmadan yükseltilebilir.

Bir kuyruk oluşturma çok adımlı bir işlemdir. Üzerinden Service Bus Mesajlaşma varlıkları (kuyruklar ve konu başlıkları) yönelik yönetim işlemlerini gerçekleştirmek [Microsoft.ServiceBus.NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) Service Bus ad alanı taban adresini sağlayarak oluşturulan sınıfı ve kullanıcı kimlik bilgileri. [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) oluşturmak, numaralandırır ve mesajlaşma varlıkları silmek için yöntemler sağlar. Oluşturduktan sonra bir [Microsoft.ServiceBus.TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider) nesnesinden SAS adını ve anahtar ve bir hizmet ad alanı Yönetim nesnesi, kullanabileceğiniz [Microsoft.ServiceBus.NamespaceManager.CreateQueue](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateQueue_System_String_)sırayı oluşturmak için yöntem. Örneğin:

```csharp
// Create management credentials
TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

Ardından, bağımsız değişken olarak Service Bus URI'si ile bir sıra nesnesi ve bir Mesajlaşma fabrikası oluşturabilirsiniz. Örneğin:

```csharp
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Sonra iletileri kuyruğa gönderebilirsiniz. Örneğin, adında aracılı iletilerin listesini varsa `MessageList`, kodu aşağıdaki örneğe benzer görünür:

```csharp
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

Ardından iletileri sıradan aşağıdaki gibi alabilirsiniz:

```csharp
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

İçinde [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu, alma işlemi tek; diğer bir deyişle, Service Bus isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. **ReceiveAndDelete** modu en basit modeldir ve senaryoları uygulama içinde tolerans bir arıza olması durumunda bir ileti işlenirken değil en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında olduğunda, kullanılıyor olarak işaretler çünkü çökmenin öncesinde kullanılan iletiyi atlamış olur.

İçinde [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu, alma işlemi hale iki aşamalı hangi, iletilere veremeyen uygulamaları desteklemenin mümkün kılar. Service Bus isteği aldığında, kullanılacak sonraki iletiyi bulur, diğer tüketicilerin iletiyi almasını engellemek için kilitler ve uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya iletiyi daha sonra işlemek üzere güvenli şekilde depoladıktan sonra) alınan iletide [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) yöntemini çağırarak alma işleminin ikinci aşamasını tamamlar. Hizmet veri yolu gördüğünde **tam** çağrısı, iletiyi kullanılıyor olarak işaretler.

Uygulama herhangi bir nedenden dolayı iletisi işleyemedi ise çağırabilirsiniz [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) alınan iletide yöntemi (yerine [tam](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete)). Bu iletinin kilidini açmak ve aynı tüketici veya başka bir rakip tüketici tarafından tekrar alınabilir kullanılabilir hale getirmek hizmet veri yolu sağlar. İkincisi, kilidi ile ilişkili bir zaman aşımı yoktur ve (örneğin, uygulama çökerse) Service Bus iletinin kilidini açar ve kolaylaştırır kilit zaman aşımı dolmadan önce iletiyi işlemek uygulama başarısız olursa kullanılabilir olması yeniden alınan) temelde gerçekleştiren bir [Abandon](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) işlemi varsayılan olarak).

Uygulama ileti, ancak önce çökmesi durumunda, unutmayın **tam** isteği bildirilmeden, yeniden başlatıldığında ileti uygulamaya tekrar teslim. Bu genellikle adlandırılır *en az bir kez* işleme; diğer bir deyişle, her ileti en az bir kez işlenir. Ancak, belirli durumlarda aynı ileti yeniden teslim edilebilir. İlave bir mantık yinelemeleri algılamak için uygulamada Required senaryo yinelenen işlemeyi, kabul etmiyorsa, elde edilebilir temel **MessageID** boyunca sabit kalır iletinin özelliği Teslim girişimleri. Bu olarak bilinir *tam olarak bir kez* işleme.

## <a name="topics-and-subscriptions"></a>Konular ve abonelikler
Kuyruklar, her ileti tek bir tüketici tarafından işlenir aksine *konuları* ve *abonelikleri* iletişim, bir çok form sağladığınız bir *Yayımlama/abonelik* düzeni. Alıcılar çok fazla sayıda ölçekleme için yararlı, yayımlanan her ileti konuya kaydedilen her abonelik için kullanılabilir yapılır. İletiler konu başlığına gönderilen ve bir veya daha fazla ilişkili abonelik, bir abonelik başına temelinde ayarlanabilir filtre kuralları bağlı olarak teslim edilir. Abonelikler ek filtreler almak istediği iletileri kısıtlamak için kullanabilirsiniz. İletileri gönderilir aynı şekilde konu kuyruğa gönderilmeden ancak iletileri alınmayan konusundan doğrudan. Bunun yerine, bunlar aboneliklerden alınır. Bir konu aboneliği konu başlığına gönderilen iletilerin kopyalarını alan sanal kuyruğa benzer. İletileri bir abonelikten bir kuyruktan alınan şekilde aynı aldı.

Karşılaştırma, aracılığıyla doğrudan bir konuya bir kuyruk iletisi göndererek işlevselliğini eşler ve bir abonelik için ileti alma işlevselliğini eşler. Bunun yanı sıra, bu abonelik sıraları açısından bu bölümde daha önce açıklanan aynı düzenleri desteği anlamına gelir: Rakip tüketici, zamana bağlı ayırma, Yük Dengeleme ve Yük Dengeleme.

Bir konu oluşturma önceki bölümdeki örnekte gösterildiği gibi bir kuyruk oluşturmak için benzerdir. Hizmet URI'si oluşturmak ve ardından [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) ad alanı istemci oluşturmak için sınıfı. Kullanarak bir konu sonra oluşturabilirsiniz [CreateTopic](/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_) yöntemi. Örneğin:

```csharp
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Ardından, abonelikleri istendiği gibi ekleyin:

```csharp
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Daha sonra bir konu istemci oluşturabilirsiniz. Örneğin:

```csharp
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

İleti gönderen kullanarak göndermek ve önceki bölümde gösterildiği gibi konu gelen ve giden iletileri alacak. Örneğin:

```csharp
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Benzer şekilde sıralar, ileti kullanarak bir abonelik alındığında bir [SubscriptionClient](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient) yerine Nesne bir [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) nesnesi. Konu adı, abonelik ve (isteğe bağlı) alma modu adını parametre olarak geçirme abonelik istemci oluşturun. Örneğin, **stok** abonelik:

```csharp
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Kurallar ve eylemler
Birçok senaryoda belirli özelliklere sahip iletileri farklı şekillerde işlenmelidir. Bunu etkinleştirmek için bu özellikler istenilen ve bu özellikler için bazı değişiklikleri gerçekleştirin iletileri bulmak için abonelikleri yapılandırabilirsiniz. Hizmet veri yolu abonelikleri konu başlığına gönderilen tüm iletiler görürsünüz, ancak bu iletiler bir kısmı yalnızca sanal abonelik kuyruğuna kopyalayabilirsiniz. Bu, abonelik filtreleri kullanılarak gerçekleştirilir. Tür değişiklikler adlı *filtre eylemlerini*. Bir abonelik oluşturduğunuzda, Sistem özellikleri her iki ileti özellikleri üzerinde çalıştığı bir filtre ifadesi sağlayabilirsiniz (örneğin, **etiket**) ve özel uygulama özelliklerini (örneğin,  **StoreName**.) SQL filtre ifadesi bu durumda isteğe bağlıdır; bir SQL filtre ifadesi, bu abonelik için tüm iletiler bir abonelikte tanımlanan herhangi bir filtre işlem gerçekleştirilir.

Önceki örneği, yalnızca'ten gelen iletileri Filtrele kullanarak **Store1**, Pano abonelik gibi oluşturursunuz:

```csharp
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Bu abonelik filtresi yerinde olan iletiler birlikte `StoreName` özelliğini `Store1` için sanal sırasına kopyalanır `Dashboard` abonelik.

Olası filtre değerleri hakkında daha fazla bilgi için belgelerine bakın [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) ve [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) sınıfları. Ayrıca bkz [aracılı Mesajlaşma: Gelişmiş filtreler](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) ve [konu filtreleri](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/TopicFilters) örnekleri.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki konularda daha fazla bilgi ve Service Bus Mesajlaşma kullanma örnekleri Gelişmiş bakın.

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Service Bus aracılı mesajlaşma .NET eğitmeni](service-bus-brokered-tutorial-dotnet.md)
* [Service Bus aracılı Mesajlaşma REST Öğreticisi](service-bus-brokered-tutorial-rest.md)
* [Aracılı Mesajlaşma: Gelişmiş filtreleri örneği](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

