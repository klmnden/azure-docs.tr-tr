---
title: Java ile Azure Service Bus konu başlıklarını kullanma | Microsoft Docs
description: Azure'da Service Bus konuları ve abonelikleri kullanın.
services: service-bus-messaging
documentationcenter: java
author: spelluru
manager: timlt
editor: ''
ms.assetid: 63d6c8bd-8a22-4292-befc-545ffb52e8eb
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 09/17/2018
ms.author: spelluru
ms.openlocfilehash: 0be5f9842cd3aa90d82f3efe44451e624ed5d371
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47395687"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-java"></a>Java ile Service Bus konu başlıklarını ve aboneliklerini kullanma

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu kılavuzda, Service Bus konu başlıklarını ve aboneliklerini kullanmayı açıklar. Java ve kullanım örnekleri yazılır [Java için Azure SDK'sı][Azure SDK for Java]. Senaryoları ele alınmaktadır **konuları ve abonelikleri oluşturma**, **abonelik filtreleri oluşturma**, **konu başlığına ileti gönderme**, **alma bir Abonelikteki iletileri**, ve **konuları ve abonelikleri silmeyi**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Service Bus konuları ve abonelikleri nelerdir?
Service Bus konuları ve abonelikleri *publish/subscribe* mesajlaşma iletişim modelini destekler. Konular ve abonelikler kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir konu aracılığıyla iletileri değiş tokuş eder.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Service Bus kuyrukları, her ileti tek bir tüketici tarafından işlenir kullanılmasının konuları ve abonelikleri bir yayımlama/abone olma modelini kullanarak iletişimin bir-çok biçimi sağlayın. Bir konuya birden fazla abonelik kaydedilebilir. Bir konuya ileti gönderildiğinde, bundan sonra, bağımsız olarak ele almak/işlemek amacıyla her abonelik için kullanılabilir hale getirilir.

Bir konuya abone olunması, konuya gönderilmiş olan iletilerin kopyaların alan sanal kuyruğa benzer. İsteğe bağlı olarak, bir konu filtreleyin veya hangi konu aboneliklerinin bir konuya hangi mesajları kısıtlayabilirsiniz olanak tanıyan bir abonelik başına temelinde filtre kuralları da kaydedebilirsiniz.

Service Bus konuları ve Abonelikleri, çok sayıda kullanıcılar ve uygulamalar arasında çok sayıda iletileri işlemek üzere ölçeği sağlar.

## <a name="create-a-service-namespace"></a>Hizmet ad alanı oluşturma
Azure'da Service Bus konu başlıklarını ve aboneliklerini kullanmaya başlamak için önce oluşturmanız gerekir bir *ad alanı*, uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sağlar.

Ad alanı oluşturmak için:

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırma
Yüklediğinizden emin olun [Java için Azure SDK'sı] [ Azure SDK for Java] Bu örnek derlemeden önce. Eclipse kullanıyorsanız yükleyebileceğiniz [Eclipse için Azure Araç Seti] [ Azure Toolkit for Eclipse] , Java için Azure SDK'sı içerir. Daha sonra ekleyebilirsiniz **Microsoft Java için Azure kitaplıkları** projenize:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Aşağıdaki `import` Java dosyasının en üstüne ifadeleri:

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Java için Azure kitaplıkları derleme yolunuza ekleyin ve proje dağıtım derlemenizi içerir.

## <a name="create-a-topic"></a>Konu başlığı oluşturma
Service Bus konu başlıklarına yönelik yönetim işlemlerini aracılığıyla gerçekleştirilebilir **ServiceBusContract** sınıfı. A **ServiceBusContract** nesnesi, yönetim izinleriyle SAS belirteci kapsülleyen uygun bir yapılandırma ile oluşturulur ve **ServiceBusContract** tek yakınlardaki bir sınıftır Azure ile iletişim.

**ServiceBusService** sınıfı oluşturmak, listeleme ve silme konuları için yöntemler sağlar. Aşağıdaki örnekte gösterildiği nasıl bir **ServiceBusService** nesne adlı bir konu oluşturmak için kullanılabilir `TestTopic`, ad alanı ile `HowToSample`:

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
      "HowToSample",
      "RootManageSharedAccessKey",
      "SAS_key_value",
      ".servicebus.windows.net"
      );

ServiceBusContract service = ServiceBusService.create(config);
TopicInfo topicInfo = new TopicInfo("TestTopic");
try  
{
    CreateTopicResult result = service.createTopic(topicInfo);
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Temel yöntem vardır **Topicınfo** ayarlamak için konunun özelliklerini etkinleştirme (örneğin: konu başlığına gönderilen iletilere uygulanacak varsayılan yaşam süresi (TTL) değerini ayarlamak için). Aşağıdaki örnekte adlı bir konu başlığının nasıl oluşturulacağını gösterir `TestTopic` en fazla 5 GB'lık:

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

Kullanabileceğiniz **listTopics** metodunda **ServiceBusContract** nesneleri belirtilen ada sahip bir konu, bir hizmet ad alanı içinde zaten mevcut olup olmadığını denetleyin.

## <a name="create-subscriptions"></a>Abonelik oluşturma
Konu abonelikleri ile de oluşturulur **ServiceBusService** sınıfı. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna gönderilen ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
Yeni bir abonelik oluşturulurken filtre belirtilmezse kullanılan varsayılan filtre **MatchAll** filtresidir. Zaman **MatchAll** filtresinin kullanılacağının, konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnekte adlı bir abonelik oluşturur `AllMessages` ve varsayılan `MatchAll` filtre.

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
Ayrıca, belirli bir konu aboneliğinde bir konu başlığına gönderilen iletilerin gösterilmesi gerekir kapsamı olanak sağlayan filtreler de oluşturabilirsiniz.

En esnek filtre türü, abonelikler tarafından desteklenen [SqlFilter][SqlFilter], SQL92 kümesini uygular. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SQL Filtresi ile kullanılabilen ifadeler hakkında daha fazla ayrıntı için gözden [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] söz dizimi.

Aşağıdaki örnekte adlı bir abonelik oluşturur `HighMessages` ile bir [SqlFilter] [ SqlFilter] yalnızca özel bir bulunduran iletileri seçen nesneyi **Lowmessages** özellik 3'ten büyük:

```java
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Benzer şekilde, aşağıdaki örnekte adlı bir abonelik oluşturur `LowMessages` ile bir [SqlFilter] [ SqlFilter] yalnızca bulunduran iletileri seçen nesneyi bir **Lowmessages** özellik daha az veya eşit 3:

```java
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Ne zaman bir ileti hemen gönderilir `TestTopic`, abone alıcılar için her zaman teslim `AllMessages` aboneliği ve seçmeli olarak teslim edilen alıcılar abone için `HighMessages` ve `LowMessages` abonelikler (bağlı ileti içeriği).

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Bir Service Bus konusuna bir ileti göndermek için uygulamanızı alır bir **ServiceBusContract** nesne. Aşağıdaki kod bir ileti göndermek nasıl gösterir `TestTopic` konu içinde daha önce oluşturduğunuz `HowToSample` ad alanı:

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Service Bus konu başlıklarına gönderilen iletiler örnekleridir [BrokeredMessage] [ BrokeredMessage] sınıfı. [BrokeredMessage][BrokeredMessage]* nesnelerin bir dizi standart yöntemleri vardır (aşağıdaki gibi **setLabel** ve **TimeToLive**), özel tutmak için kullanılan bir sözlüğü uygulamaya özgü özellikler ve rastgele uygulama verileri gövdesi. Uygulamanın herhangi bir seri hale getirilebilir nesnesi oluşturucusuna geçirerek ileti gövdesini ayarlayabilirsiniz [BrokeredMessage][BrokeredMessage]ve uygun **DataContractSerializer** sonra nesneyi serileştirmek için kullanılır. Alternatif olarak, bir **java.io.InputStream** sağlanabilir.

Aşağıdaki örnek nasıl beş test iletisi göndereceğinizi gösterir `TestTopic` **MessageSender** önceki kod parçacığında elde ediyoruz.
Not nasıl **Lowmessages** her ileti özelliği değerinin değişen döngü yinelemeyi (Bu değer alacak abonelikleri belirler):

```java
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Bir konu başlığında tutulan ileti sayısına bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu sınırı yoktur. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="how-to-receive-messages-from-a-subscription"></a>Abonelikten ileti alma
Abonelikten ileti almak için kullanmak bir **ServiceBusContract** nesne. Alınan iletiler, iki farklı modda çalışabilir: **ReceiveAndDelete** ve **PeekLock** (varsayılan).

Kullanırken **ReceiveAndDelete** modunda almak bir tek işlem - diğer bir deyişle, Service Bus iletiye yönelik Okuma isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. **ReceiveAndDelete** modu en basit modeldir ve içinde bir uygulama tolere edebilen bir hata oluşursa bir iletiyi işlememeye izin senaryolarda en iyi şekilde çalışır. Örneğin, hangi tüketici alma isteği bildirdiğini ve ardından işlenmeden önce kilitleniyor bir senaryo düşünün. Service Bus iletiyi kullanılıyor olarak işaretlediğinden, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında ardından onu çökmenin öncesinde kullanılan iletiyi eksik.

İçinde **PeekLock** modu, alma, atlanan iletilere veremeyen uygulamaları desteklemenin mümkün hale getiren bir iki aşamalıdır. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya güvenilir bir şekilde işlemek üzere depolar sonra) çağırarak alma işleminin ikinci aşamasını tamamlar **Sil** alınan iletide. Service Bus gördüğünde **Sil** çağrısı, bu iletiyi kullanılıyor olarak işaretler ve konu başlığından kaldırır.

Aşağıdaki örnek nasıl ileti alındı ve işlenen kullanarak gösterir **PeekLock** (varsayılan mod). Örnek döngü yapar ve iletileri işleyen `HighMessages` abonelik ve daha fazla ileti olduğunda çıkar (Alternatif olarak, bu yeni iletileri için beklenecek ayarlanabilir).

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi işlemek için herhangi bir nedenle silemiyor sonra çağırabilirsiniz **unlockMessage** yöntemi alınan iletide (yerine **deleteMessage** yöntemi). Bu yöntem çağrısı, Service Bus konu başlığı içindeki iletinin kilidini açmasına ve iletiyi aynı kullanıcı uygulama veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale neden olur.

Ayrıca konusu içinde kilitli iletiye ilişkin bir zaman aşımı yoktur ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açar ve alınabilmesini kilit zaman aşımı dolmadan tekrar kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, **deleteMessage** isteği bildirilmeden, sonra yeniden başlatıldığında ileti uygulamaya yeniden teslim. Bu işlem genellikle çağrılırken **en az bir kez işlenmesini**, diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Kullanarak bunu yapabilirsiniz **getMessageId** yöntemi iletinin teslim denemeleri arasında sabit kalır.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Konuları ve abonelikleri silmek için birincil yolu bir **ServiceBusContract** nesne. Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir.

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi için [Service Bus kuyrukları, konular ve abonelikler] [ Service Bus queues, topics, and subscriptions] daha fazla bilgi için.

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.azure.servicebus.sqlfilter
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.azure.servicebus.sqlfilter.sqlexpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
