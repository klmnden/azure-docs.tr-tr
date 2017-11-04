---
title: "Node.js içinde Service Bus kuyruklarını kullanma | Microsoft Docs"
description: "Bir Node.js uygulamasını Azure'da Service Bus kuyruklarını kullanmayı öğrenin."
services: service-bus-messaging
documentationcenter: nodejs
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 5b309534f7aef602610cfdb6aa784d180551e1ec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-queues-with-nodejs"></a>Node.js ile Service Bus kuyruklarını kullanma

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu makalede, Node.js ile Service Bus kuyruklarını kullanmayı açıklar. Örnekler JavaScript'te yazılmış ve Node.js Azure modülü kullanın. Kapsamdaki senaryolar dahil **sıra oluşturma**, **ileti gönderme ve alma**, ve **sıraları silme**. Kuyruklar hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Boş bir Node.js uygulaması oluşturun. Bir Node.js uygulaması oluşturma hakkında daha fazla yönerge için bkz: [oluşturma ve bir Azure Web sitesine bir Node.js uygulaması dağıtma][Create and deploy a Node.js application to an Azure Website], veya [Node.js bulut hizmeti] [ Node.js Cloud Service] Windows PowerShell kullanarak.

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırın
Azure Service Bus hizmetini kullanmak için karşıdan yükle ve Node.js Azure paketini kullanın. Bu paket Service Bus REST Hizmetleri ile iletişim kitaplıkları kümesi içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paket elde etmek için düğüm paketi Yöneticisi (NPM) kullanın
1. Kullanım **Node.js için Windows PowerShell** gitmek için komut penceresi **c:\\düğümü\\sbqueues\\WebRole1** örneğinizi oluşturduğunuz klasörü uygulama.
2. Tür **npm yükleme azure** komut penceresinde hangi neden aşağıdakine benzer bir çıktı:

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
3. El ile çalıştırabilirsiniz **ls** doğrulamak için komutu bir **node_modules** klasörü oluşturuldu. Bu klasöre Bul **azure** Service Bus kuyruklarını erişmek için gereken kitaplıklar içeren paket.

### <a name="import-the-module"></a>Modülünü içeri aktarın
Not Defteri'nde veya başka bir metin düzenleyicisi kullanarak, aşağıdaki üst kısmına ekleyin **server.js** uygulamanın dosya:

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Bir Azure hizmet veri yolu bağlantı kurma
Ortam değişkeni Azure modül okur `AZURE_SERVICEBUS_CONNECTION_STRING` Service Bus hizmetine bağlanmak için gerekli bilgileri elde edilir. Bu ortam değişkenini ayarlanmamışsa çağrılırken hesap bilgileri belirtmelisiniz `createServiceBusService`.

Azure bulut hizmeti için bir yapılandırma dosyasında ortam değişkenlerini ayarlama örneği için bkz: [depolama Node.js bulut hizmetiyle][Node.js Cloud Service with Storage].

Ortam değişkenlerini ayarlama örnek için [Azure portal] [ Azure portal] bir Azure Web sitesi için bkz: [Node.js Web uygulaması depolama ile] [ Node.js Web Application with Storage].

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
**ServiceBusService** nesnesi ile Service Bus kuyruklarını çalışmanıza olanak sağlar. Aşağıdaki kod oluşturur bir **ServiceBusService** nesnesi. Üst kısmına ekleyin **server.js** Azure modülü içeri aktarmak için deyimi sonra dosyayı:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Çağırarak `createQueueIfNotExists` üzerinde **ServiceBusService** nesnesi, (varsa) belirtilen sırada döndürülür veya belirtilen ada sahip yeni bir sıra oluşturulur. Aşağıdaki kod `createQueueIfNotExists` oluşturmak veya adlı kuyruğuna bağlanmak için `myqueue`:

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

`createServiceBusService` Yöntemi de ileti zamanı dinamik veya en büyük sıra boyutu gibi varsayılan sırası ayarlarını geçersiz kılmanıza olanak sağlayan ek seçenekleri destekler. Aşağıdaki örnek en büyük sıra boyutu 5 GB ve bir süre için 1 dakika (TTL) değerini Canlı ayarlar:

```javascript
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Filtreler
İsteğe bağlı filtreleme işlemleri kullanarak gerçekleştirilen işlemler için uygulanabilir **ServiceBusService**. İşlemleri filtreleme içerebilir günlüğe kaydetme, otomatik olarak yeniden deneniyor, vs. İmzalı bir yöntem uygulayan nesneler filtreleri şunlardır:

```javascript
function handle (requestOptions, next)
```

Ön işleme isteği seçenekleri yaptıktan sonra yöntemini çağırmalı `next`, bir geri çağırma aşağıdaki imzayla geçirme:

```javascript
function (returnObject, finalCallback, next)
```

İşleme sonra bu geri çağırma `returnObject` (yanıt istek sunucuya), geri çağırma ya da çağırmanız gerekir `next` diğer filtreleri işlemeye devam et veya yalnızca çağrılacak varsa `finalCallback`, hizmet sona erer çağırma.

Yeniden deneme mantığını uygulaması iki filtre Node.js için Azure SDK'sı ile birlikte `ExponentialRetryPolicyFilter` ve `LinearRetryPolicyFilter`. Aşağıdaki kod oluşturur bir `ServiceBusService` kullanan nesneyi `ExponentialRetryPolicyFilter`:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Uygulama çağrılarınızı bir Service Bus kuyruğuna bir ileti göndermek için `sendQueueMessage` yöntemi **ServiceBusService** nesnesi. İletileri gönderilen (ve öğesinden alınan) hizmet kuyruklar veri yolu **BrokeredMessage** nesneleri ve bir standart özellikler kümesi sahip (gibi **etiket** ve **TimeToLive**), Özel uygulamaya özgü özellikler ve rastgele uygulama verileri gövdesi tutmak için kullanılan sözlüğü. Bir uygulama, iletisi olarak bir dize geçirerek ileti gövdesini ayarlayabilir. Gerekli tüm standart özellikleri varsayılan değerlerle doldurulur.

Aşağıdaki örnek adlı sırasına sınama iletisi göndermek nasıl gösterir `myqueue` kullanarak `sendQueueMessage`:

```javascript
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotalar hakkında daha fazla bilgi için bkz: [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma
İletileri kullanarak bir Sıraya alınan `receiveQueueMessage` yöntemi **ServiceBusService** nesnesi. Varsayılan olarak, bunlar okurken iletiler sıradan silinir; Ancak, (Özet) okuma ve iletiyi sıradan isteğe bağlı parametresi ayarlanarak silmeden kilitlemek `isPeekLock` için **doğru**.

Okuma ve ileti alma işleminin bir parçası olarak silme varsayılan davranışı en basit modeldir ve uygulamanın hata oluştuğunda bir iletiyi işlemeyi değil dayanabileceği senaryoları için en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretlenmiş nedeniyle uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, sonra da çökmenin öncesinde kullanılan iletiyi atlamış olur.

Varsa `isPeekLock` parametrenin ayarlanmış **doğru**, alma, iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya sonra işlemek için depoladıktan sonra), çağırarak alma işleminin ikinci aşamasını tamamlar `deleteMessage` yöntemi ve parametre olarak silinecek ileti sağlama. `deleteMessage` Yöntemi iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırır.

Aşağıdaki örnek, kullanarak iletileri almak ve işlemek gösterilmiştir `receiveQueueMessage`. Örnek ilk alır ve bir iletiyi siler ve kullanarak bir ileti alır `isPeekLock` kümesine **true**, iletiyi kullanarak siler `deleteMessage`:

```javascript
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi herhangi bir nedenden dolayı işleyemedi sonra işleyememesi `unlockMessage` yöntemi **ServiceBusService** nesnesi. Bu, Service Bus hizmetinin Kuyruktaki iletinin kilidini açmasına ve iletiyi aynı veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale getirmesine neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı vardır ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açmak ve onu kilit zaman aşımı dolmadan yeniden alınabilmesi kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, `deleteMessage` yöntemi çağrıldıktan sonra yeniden başlatıldığında ileti uygulamaya tekrar teslim edilir. Bu genellikle adlandırılır *en az bir kez işleme*, diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu genellikle kullanılarak elde edilen **MessageID** özelliğini iletinin teslimat denemelerinde.

## <a name="next-steps"></a>Sonraki adımlar
Kuyruklar hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın.

* [Kuyruklar, konu başlıkları ve abonelikler][Queues, topics, and subscriptions]
* [Düğümü için Azure SDK] [ Azure SDK for Node] github'daki
* [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application to an Azure Website]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]:../cosmos-db/table-storage-cloud-service-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
