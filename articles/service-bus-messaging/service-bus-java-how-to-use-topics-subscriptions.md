---
title: Java ile Azure Service Bus konu başlıklarını kullanma | Microsoft Docs
description: Azure'da Service Bus konuları ve abonelikleri kullanın.
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 63d6c8bd-8a22-4292-befc-545ffb52e8eb
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/17/2017
ms.author: sethm
ms.openlocfilehash: 9c2501840b3c00a63b0344d48e3225fd2c9d1620
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30927678"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-java"></a>Service Bus konuları ve abonelikleri Java ile kullanma

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu kılavuz, Service Bus konu başlıklarını ve aboneliklerini kullanmayı açıklar. Java ve kullanım örnekleri yazılır [Java için Azure SDK][Azure SDK for Java]. Kapsamdaki senaryolar dahil **konuları ve abonelikleri oluşturma**, **abonelik filtreleri oluşturma**, **konu başlığına ileti gönderme**, **abonelikten ileti alma**, ve **konuları ve abonelikleri silmeyi**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Service Bus konuları ve abonelikleri nelerdir?
Service Bus konuları ve abonelikleri *publish/subscribe* mesajlaşma iletişim modelini destekler. Konular ve abonelikler kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir konu aracılığıyla iletileri değiş tokuş eder.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Service Bus kuyruklarının tersine, burada her ileti bir tüketici tarafından işlenir; konular ve abonelikler, publish/subscribe modelini kullanarak iletişimin "bir-çok" biçimini sağlar. Bir konuya birden fazla abonelik kaydedilebilir. Bir konuya ileti gönderildiğinde, bundan sonra, bağımsız olarak ele almak/işlemek amacıyla her abonelik için kullanılabilir hale getirilir.

Bir konuya abone olunması, konuya gönderilmiş olan iletilerin kopyaların alan sanal kuyruğa benzer. İsteğe bağlı olarak, filtre kuralları konuyla ilgili filtre/bir konunun hangi iletileri hangi konu abonelikleriyle kısıtlamanızı olanak tanıyan bir abonelik başına temelinde kaydedebilirsiniz.

Service Bus konuları ve Abonelikleri, çok sayıda kullanıcılar ve uygulamalar arasında çok sayıda iletileri işlemek için ölçeklendirmek etkinleştirin.

## <a name="create-a-service-namespace"></a>Hizmet ad alanı oluşturma
Azure'da Service Bus konu başlıklarını ve aboneliklerini kullanmaya başlamak için önce oluşturmanız gerekir bir *ad alanı*, uygulamanızın Service Bus kaynaklarını adreslemek için kapsam bir kapsayıcı sağlar.

Ad alanı oluşturmak için:

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırın
Yüklediğinizden emin olun [Java için Azure SDK] [ Azure SDK for Java] önce bu örnek oluşturma. Eclipse kullanıyorsanız, yükleyebileceğiniz [Eclipse için Azure Araç Seti] [ Azure Toolkit for Eclipse] Java için Azure SDK'sı içerir. Daha sonra ekleyebilirsiniz **Java için Microsoft Azure kitaplıkları** projenize:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

Aşağıdakileri ekleyin `import` deyimlerini Java dosyanın en üstüne ekleyin:

```java
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

Java için Azure kitaplıkları, yapı yoluna ekleyin ve proje dağıtım derlemenizi içerir.

## <a name="create-a-topic"></a>Konu başlığı oluşturma
Service Bus konu başlıklarına yönelik yönetim işlemlerini aracılığıyla gerçekleştirilebilecek **ServiceBusContract** sınıfı. A **ServiceBusContract** nesnesi, yönetmek için gerekli izinlere sahip SAS belirteci yalıtır uygun bir yapılandırma ile oluşturulur ve **ServiceBusContract** sınıfı Azure ile iletişimin tek noktasıdır.

**ServiceBusService** sınıfı oluşturmak, numaralandırır ve konuları silmek için yöntemler sağlar. Aşağıdaki örnekte gösterildiği nasıl bir **ServiceBusService** nesnesi, adlandırılan bir konu oluşturmak için kullanılabilir `TestTopic`, adlı bir ad alanı ile `HowToSample`:

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

Yöntemleri vardır **TopicInfo** ayarlanacak konunun özelliklerini etkinleştirme (örneğin: konu başlığına gönderilen iletilere uygulanacak varsayılan yaşam süresi (TTL) değerini ayarlamak için). Aşağıdaki örnek adlı bir konu oluşturun gösterilmektedir `TestTopic` maksimum 5 GB boyuta sahip:

```java
long maxSizeInMegabytes = 5120;  
TopicInfo topicInfo = new TopicInfo("TestTopic");  
topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateTopicResult result = service.createTopic(topicInfo);
```

Kullanabileceğiniz **listTopics** yöntemi **ServiceBusContract** nesneleri zaten bir hizmet ad alanında belirtilen ada sahip bir konu var olup olmadığını denetleyin.

## <a name="create-subscriptions"></a>Abonelikleri oluşturma
Konular için abonelikleri ile de oluşturulur **ServiceBusService** sınıfı. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna gönderilen ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
Yeni bir abonelik oluşturulurken filtre belirtilmezse kullanılan varsayılan filtre **MatchAll** filtresidir. Zaman **MatchAll** filtre kullanıldığında, konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnekte "AllMessages" adlı bir abonelik oluşturulur ve varsayılan **MatchAll** filtresi kullanılır.

```java
SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
CreateSubscriptionResult result =
    service.createSubscription("TestTopic", subInfo);
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
Bir konu başlığına gönderilen iletileri içinde belirli konu aboneliği göstermelidir kapsamına sağlayan filtreler de oluşturabilirsiniz.

Filtre abonelikler tarafından desteklenen en esnek türü [SqlFilter][SqlFilter], SQL92 alt kümesi uygular. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SQL Filtresi ile kullanılabilen ifadeler hakkında daha fazla ayrıntı için gözden [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] sözdizimi.

Aşağıdaki örnek adlı bir abonelik oluşturur `HighMessages` ile bir [SqlFilter] [ SqlFilter] yalnızca özel iletileri seçen nesne **MessageNumber** özelliği 3'ten büyük:

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

Benzer şekilde, aşağıdaki örnekte adlı bir abonelik oluşturulur `LowMessages` ile bir [SqlFilter] [ SqlFilter] yalnızca sahip iletileri seçen nesnesi bir **MessageNumber** özelliğine daha az veya bu değere eşit 3:

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

Ne zaman bir ileti artık gönderildiği `TestTopic`, her zaman alıcılara teslim edilir `AllMessages` aboneliği ve alıcılara teslim seçmeli olarak `HighMessages` ve `LowMessages` abonelikleri (bağlı olarak ileti içeriği).

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Service Bus konu başlığına bir ileti göndermek için uygulamanızı alacağı bir **ServiceBusContract** nesnesi. Aşağıdaki kodda bir ileti göndermeye gösterilmiştir `TestTopic` konu içinde daha önce oluşturduğunuz `HowToSample` ad alanı:

```java
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Service Bus konu başlıklarına gönderilen iletiler örnekleridir [BrokeredMessage] [ BrokeredMessage] sınıfı. [BrokeredMessage][BrokeredMessage]* nesneler sahip bir dizi standart yöntem (gibi **setLabel** ve **TimeToLive**), özel tutmak için kullanılan bir sözlük uygulamaya özgü özellikler ve rastgele uygulama verileri gövdesi. Bir uygulama herhangi bir seri hale getirilebilir nesne oluşturucusuna geçirerek ileti gövdesini ayarlayabilir [BrokeredMessage][BrokeredMessage]ve uygun **DataContractSerializer** sonra nesneyi serileştirmek için kullanılır. Alternatif olarak, bir **java.io.InputStream** sağlanabilir.

Aşağıdaki örnekte nasıl beş test iletisi göndereceğinizi gösterir `TestTopic` **MessageSender** biz önceki kod parçacığında elde.
Not nasıl **MessageNumber** özellik değeri her iletinin tekrarına döngü üzerinde (Bu değer alacak abonelikleri belirler):

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

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Bir konu başlığında tutulan ileti sayısına bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu bir sınır yoktur. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="how-to-receive-messages-from-a-subscription"></a>Abonelikten ileti alma
Abonelikten ileti almak için kullandığınız bir **ServiceBusContract** nesnesi. Alınan iletiler, iki farklı modda çalışabilir: **ReceiveAndDelete** ve **PeekLock** (varsayılan).

Kullanırken **ReceiveAndDelete** modu, alma bir tek işlemi - diğer bir deyişle, Service Bus iletiye yönelik Okuma isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. **ReceiveAndDelete** modu en basit modeldir ve senaryoları bir uygulama içinde tolerans bir hata oluşursa bir ileti işlenmiyor en iyi şekilde çalışır. Örneğin, tüketici alma isteği bildirdiğini ve isteğin işlenmeden çöktüğünü bir senaryo düşünün. Service Bus iletiyi kullanılıyor olarak işaretlenmiş olduğundan uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, sonra da çökmenin öncesinde kullanılan iletiyi eksik olduğunu.

İçinde **PeekLock** modu, alma, iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi olur. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya sonra işlemek için depoladıktan sonra), çağırarak alma işleminin ikinci aşamasını tamamlar **silmek** alınan iletide. Hizmet veri yolu gördüğünde **silmek** çağrısı, iletiyi kullanılıyor olarak işaretler ve konusundan kaldırır.

Aşağıdaki örnek nasıl ileti aldı ve işlenen kullanarak gösterir **PeekLock** (varsayılan mod). Örnek bir döngü gerçekleştirir ve iletileri işleyen `HighMessages` abonelik ve daha fazla ileti olduğunda çıkar (Alternatif olarak, bu yeni iletiler için beklenecek ayarlanabilir).

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
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi herhangi bir nedenden dolayı işleyemedi sonra işleyememesi **unlockMessage** alınan iletide yöntemi (yerine **deleteMessage** yöntemi). Bu konu içinde ileti kilidini açmak ve aynı kullanıcı uygulama tarafından veya başka bir kullanıcı uygulama tarafından tekrar alınabilir kullanılabilir hale getirmek Service Bus neden olur.

Ayrıca konusu içinde kilitli bir ileti ile ilişkili bir zaman aşımı vardır ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açar ve kolaylaştırır kilit zaman aşımı dolmadan yeniden alınabilmesi kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, **deleteMessage** isteği bildirilmeden, sonra yeniden başlatıldığında ileti uygulamaya tekrar teslim. Bu işlem genellikle adlı **en az bir kez işleme**; diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu genellikle kullanılarak elde edilen **getMessageId** iletinin teslimat denemelerinde yöntemi.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Konuları ve abonelikleri silmek için birincil yolu bir **ServiceBusContract** nesnesi. Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir.

```java
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Sonraki Adımlar
Service Bus kuyruklarına öğrendiğinize göre bkz: [Service Bus kuyrukları, konu başlıkları ve abonelikler] [ Service Bus queues, topics, and subscriptions] daha fazla bilgi için.

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: ../azure-toolkit-for-eclipse.md
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.azure.servicebus.sqlfilter
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.azure.servicebus.sqlfilter.sqlexpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

[0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
[2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
[3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
