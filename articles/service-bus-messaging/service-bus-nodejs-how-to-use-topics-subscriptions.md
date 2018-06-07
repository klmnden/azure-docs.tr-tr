---
title: Azure Service Bus konuları ve abonelikleri Node.js ile kullanma | Microsoft Docs
description: Service Bus konuları ve abonelikleri Azure üzerinde bir Node.js uygulamasını kullanmayı öğrenin.
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: d3a7ebd135f705a6a3ea91feb4e037a9ed6d0c79
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641314"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-nodejs"></a>Kullanım Service Bus konuları ve abonelikleri Node.js ile nasıl

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu kılavuz, Service Bus konuları ve abonelikleri Node.js uygulamalardan kullanmayı açıklar. Kapsamdaki senaryolar şunlardır:

- Konuları ve abonelikleri oluşturma 
- Abonelik filtreleri oluşturma 
- Bir konu başlığına ileti gönderme 
- Abonelikten ileti alma
- Konuları ve abonelikleri silme 

Konu başlıkları ve abonelikler hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

[!INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Boş bir Node.js uygulaması oluşturun. Bir Node.js uygulaması oluşturma ile ilgili yönergeler için bkz: [Node.js uygulama için bir Azure Web sitesi oluşturun ve dağıtın], [Node.js bulut hizmeti] [ Node.js Cloud Service] Windows kullanma PowerShell veya Web sitesini WebMatrix ile.

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırın
Service Bus hizmetini kullanmak için Node.js Azure paketini indirin. Bu paket Service Bus REST Hizmetleri ile iletişim kitaplıkları kümesi içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paket elde etmek için düğüm paketi Yöneticisi (NPM) kullanın
1. Bir komut satırı arabirimi gibi açıp **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (UNIX).
2. Örnek uygulamanızı oluşturduğunuz klasöre gidin.
3. Tür **npm yükleme azure** komut penceresinde hangi sonuç aşağıdaki çıktı:

   ```
       azure@0.7.5 node_modules\azure
   ├── dateformat@1.0.2-1.2.3
   ├── xmlbuilder@0.4.2
   ├── node-uuid@1.2.0
   ├── mime@1.2.9
   ├── underscore@1.4.4
   ├── validator@1.1.1
   ├── tunnel@0.0.2
   ├── wns@0.5.3
   ├── xml2js@0.2.7 (sax@0.5.2)
   └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
   ```
3. El ile çalıştırabilirsiniz **ls** doğrulamak için komutu bir **düğümü\_modülleri** klasörü oluşturuldu. Bu klasöre Bul **azure** Service Bus konu başlıklarını erişmek için gereken kitaplıklar içeren paket.

### <a name="import-the-module"></a>Modülünü içeri aktarın
Not Defteri'nde veya başka bir metin düzenleyicisi kullanarak, aşağıdaki üst kısmına ekleyin **server.js** uygulamanın dosya:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Hizmet veri yolu bağlantı kurma
Ortam değişkeni Azure modül okur `AZURE_SERVICEBUS_CONNECTION_STRING` önceki adımda elde ettiğiniz bağlantı dizesi için "kimlik bilgilerini edinin." Bu ortam değişkenini ayarlanmamışsa çağrılırken hesap bilgileri belirtmelisiniz `createServiceBusService`.

Azure bulut hizmeti için ortam değişkenlerini ayarlama örneği için bkz: [depolama Node.js bulut hizmetiyle][Node.js Cloud Service with Storage].



## <a name="create-a-topic"></a>Konu başlığı oluşturma
**ServiceBusService** nesne konularda çalışmanıza olanak sağlar. Aşağıdaki kod oluşturur bir **ServiceBusService** nesnesi. Üst kısmına ekleyin **server.js** azure modülü içeri aktarmak için deyimi sonra dosyayı:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Çağırırsanız `createTopicIfNotExists` üzerinde **ServiceBusService** nesnesi, (varsa) belirtilen konu döndürülür veya belirtilen ada sahip yeni bir konu oluşturulur. Aşağıdaki kod `createTopicIfNotExists` oluşturmak veya adlı konuya bağlanmak için `MyTopic`:

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

`createServiceBusService` Yöntemi de ileti yaşam süresi veya en fazla konu başlığı boyutu gibi varsayılan konu ayarlarını geçersiz kılmak üzere etkinleştirmeniz ek seçenekleri destekler. 

Aşağıdaki örnek, bir dakikalık yaşam süresi ile 5 GB ila en fazla konu başlığı boyutu ayarlar:

```javascript
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filtreler
İsteğe bağlı filtreleme işlemleri kullanarak gerçekleştirilen işlemler için uygulanabilir **ServiceBusService**. İşlemleri filtreleme içerebilir günlüğe kaydetme, otomatik olarak yeniden deneniyor, vs. İmzalı bir yöntem uygulayan nesneler filtreleri şunlardır:

```javascript
function handle (requestOptions, next)
```

İstek seçenekleri ön işleme gerçekleştirdikten sonra yöntemini çağırır `next`ve bir geri çağırma aşağıdaki imzayla geçirir:

```javascript
function (returnObject, finalCallback, next)
```

İşleme sonra bu geri çağırma `returnObject` (yanıt istek sunucuya), geri çağırma gerekir (varsa) diğer filtreleri işlemeye devam sonraki çağırma ya da çağırma `finalCallback` hizmet başlatma sonlandırmak için.

Yeniden deneme mantığını uygulaması iki filtre Node.js için Azure SDK'sı ile birlikte **ExponentialRetryPolicyFilter** ve **LinearRetryPolicyFilter**. Aşağıdaki oluşturur bir **ServiceBusService** kullanan nesneyi **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Abonelikleri oluşturma
Konu aboneliklerini içeren de oluşturulur **ServiceBusService** nesnesi. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna teslim edilen ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

> [!NOTE]
> Abonelikleri kalıcı ya da bunlar kadar veya ilişkili konu birlikte silinir. Uygulamanız bir aboneliği oluşturmak için mantığı içeriyorsa, ilk abonelik kullanarak olup olmadığını kontrol edin `getSubscription` yöntemi.
>
>

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
**MatchAll** bir abonelik oluşturulurken kullanılan varsayılan filtresini filtredir. **MatchAll** filtresini kullandığınızda konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnek AllMessages adlı bir abonelik oluşturulur ve varsayılan **MatchAll** Filtresi.

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
Ayrıca, bir konu başlığına gönderilen iletileri kapsamına belirli konu aboneliği içinde gösterilmesi gerekir izin filtreler oluşturabilirsiniz.

Filtre abonelikler tarafından desteklenen en esnek türü **SqlFilter**, SQL92 alt kümesi uygular. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SQL Filtresi ile kullanılabilen ifadeler hakkında daha fazla ayrıntı için gözden [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] sözdizimi.

Filtreler eklenebilir aboneliği kullanarak `createRule` yöntemi **ServiceBusService** nesnesi. Bu yöntem, mevcut bir aboneliğe yeni filtreler eklemenizi sağlar.

> [!NOTE]
> Varsayılan filtre otomatik olarak tüm yeni abonelikleri uygulandığından, varsayılan filtre önce kaldırmanız gerekir veya **MatchAll** belirttiğiniz diğer filtreleri geçersiz kılar. Varsayılan kural kullanarak kaldırabilirsiniz `deleteRule` yöntemi **ServiceBusService** nesnesi.
>
>

Aşağıdaki örnek adlı bir abonelik oluşturur `HighMessages` ile bir **SqlFilter** özel iletileri yalnızca seçer `messagenumber` özelliği 3'ten büyük:

```javascript
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'HighMessages',
            'HighMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Benzer şekilde, aşağıdaki örnekte adlı bir abonelik oluşturulur `LowMessages` ile bir **SqlFilter** olan iletiler yalnızca seçer bir `messagenumber` özelliğine daha az veya bu değere eşit 3:

```javascript
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'LowMessages',
            'LowMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Ne zaman bir ileti artık gönderildiği `MyTopic`, alıcılara teslim edilecek `AllMessages` konu aboneliği ve alıcılara teslim seçmeli olarak `HighMessages` ve `LowMessages` (bağlı olarak konu abonelikleri ileti içeriği).

## <a name="how-to-send-messages-to-a-topic"></a>Bir konuya ileti göndermek nasıl
Service Bus konu başlığına bir ileti göndermek için uygulamanızı kullanmalısınız `sendTopicMessage` yöntemi **ServiceBusService** nesnesi.
Service Bus konu başlıklarına gönderilen iletiler **BrokeredMessage** nesneleri.
**BrokeredMessage** nesneleri olan bir standart özellikler kümesi (gibi `Label` ve `TimeToLive`), bir uygulamaya özgü özel özellikleri tutmak için kullanılan sözlüğü ve dize verileri gövdesi içerir. Bir uygulama için bir dize değeri geçirerek ileti gövdesini ayarlayabilir `sendTopicMessage` ve gereken tüm standart özellikleri varsayılan değerlerle doldurulmuş.

Aşağıdaki örnekte nasıl beş test iletisi göndereceğinizi gösterir `MyTopic`. `messagenumber` Özellik değeri her iletinin tekrarına döngü üzerinde (Bu alacak abonelikleri belirler):

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Bir konu başlığında tutulan ileti sayısına bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu bir sınır yoktur. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
İletileri kullanarak bir abonelik alınan `receiveSubscriptionMessage` yöntemi **ServiceBusService** nesnesi. Varsayılan olarak, bunlar okurken iletiler abonelikten silinir. Bununla birlikte, isteğe bağlı bir parametre ayarlayabilirsiniz `isPeekLock` için **true** (Özet) okuyup abonelikten silmeden ileti kilitleyin.

Okuma ve ileti alma işleminin bir parçası olarak silme varsayılan davranışı en basit modeldir ve uygulamanın hata oluştuğunda bir iletiyi işlemeyi değil dayanabileceği senaryoları için en iyi şekilde çalışır. Bu davranış anlamak için tüketici alma isteği bildirdiğini ve isteğin işlenmeden çöktüğünü bir senaryo düşünün. Service Bus iletiyi kullanılıyor olarak işaretlenmiş nedeniyle uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, sonra da çökmenin öncesinde kullanılan iletiyi atlamış olur.

Varsa `isPeekLock` parametrenin ayarlanmış **doğru**, alma, atlanan iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında kullanmak için sonraki iletiyi bulur, diğer tüketicilerin iletiyi almasını engellemek için kilitler ve uygulamaya döndürür.
Uygulama iletiyi işler (veya sonra işlemek için depoladıktan sonra) çağırarak alma işleminin ikinci aşamasını tamamlar **deleteMessage** yöntemi ve parametre olarak silmek için ileti geçirir. **DeleteMessage** yöntemi iletiyi kullanılmış olarak işaretler ve abonelikten kaldırır.

Aşağıdaki örnek nasıl ileti aldı ve işlenen kullanarak gösterir `receiveSubscriptionMessage`. Örnek ilk alır ve bir ileti 'LowMessages' abonelikten siler ve sonra kullanarak 'HighMessages' abonelik bir ileti alır `isPeekLock` true olarak ayarlanmış. Ardından kullanarak iletiyi siler `deleteMessage`:

```javascript
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        })
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi herhangi bir nedenden dolayı işleyemedi sonra işleyememesi `unlockMessage` yöntemi **ServiceBusService** nesnesi. Bu yöntem, Service Bus hizmetinin Abonelikteki iletinin kilidini açmasına ve tekrar alınabilir hale getirmesine neden olur. Bu örnekte aynı kullanıcı uygulama tarafından veya başka bir kullanıcı uygulama tarafından.

Ayrıca abonelikte kilitlenen iletiye ilişkin bir zaman aşımı vardır. (Örneğin, uygulama çökerse) kilit zaman aşımı süresi dolmadan önce iletiyi işlemek uygulama başarısız olursa, Service Bus otomatik olarak iletinin kilidini açar ve tekrar alınabilmesini sağlar.

Uygulama iletiyi ancak önce çökmesi durumunda `deleteMessage` yöntemi çağrıldığında, yeniden başlatıldığında ileti uygulamaya tekrar teslim. Bu davranış adlandırılırlar *en az bir kez işleme*. Diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin uygulamanıza mantığı eklemeniz gerekir. Kullanabileceğiniz **MessageID** iletinin teslimat denemelerinde özelliği.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Konu başlıkları ve abonelikler kalıcıdır ve açıkça olmalıdır aracılığıyla ya da silinmiş [Azure portal] [ Azure portal] veya program aracılığıyla.
Aşağıdaki örnek adlı konuyu silmek gösterilmiştir `MyTopic`:

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir. Aşağıdaki örnekte nasıl adlı bir abonelik silineceğini gösterir `HighMessages` gelen `MyTopic` konu:

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

## <a name="next-steps"></a>Sonraki Adımlar
Service Bus konuları öğrendiğinize göre daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* Bkz: [kuyruklar, konu başlıkları ve abonelikler][Queues, topics, and subscriptions].
* [SqlFilter][SqlFilter] için API başvurusu.
* Ziyaret [düğümü için Azure SDK] [ Azure SDK for Node] github'daki.

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Node.js uygulama için bir Azure Web sitesi oluşturun ve dağıtın]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md

