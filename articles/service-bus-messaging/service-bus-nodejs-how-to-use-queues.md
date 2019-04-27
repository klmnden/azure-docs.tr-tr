---
title: Node.js'de Service Bus kuyruklarını kullanma | Microsoft Docs
description: Azure'da bir Node.js uygulamasından Service Bus kuyruklarını kullanmayı öğrenin.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/10/2019
ms.author: aschhab
ms.openlocfilehash: 6159609f894f967e8ee372a0ee316eb900537aba
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60589997"
---
# <a name="how-to-use-service-bus-queues-with-nodejs"></a>Node.js ile Service Bus kuyruklarını kullanma

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu öğreticide, bir Service Bus kuyruğundaki iletileri alıp ileti göndermek için Node.js uygulamalarının nasıl oluşturulacağını öğrenin. Örnekler, JavaScript'te yazılmış ve Node.js Azure modülü kullanır. 

## <a name="prerequisites"></a>Önkoşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Etkinleştirebilir, [MSDN abone Avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) veya kaydolun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
2. Çalışmak için bir kuyruk yoksa, izleyeceğiniz adımlar [Service Bus kuyruğuna oluşturmak için Azure portalını kullanın](service-bus-quickstart-portal.md) makalenin bir kuyruk oluşturun.
    1. Hızlı Okuma **genel bakış** Service Bus **kuyrukları**. 
    2. Hizmet veri yolu oluşturma **ad alanı**. 
    3. Alma **bağlantı dizesi**. 

        > [!NOTE]
        > Oluşturacağınız bir **kuyruk** Bu öğreticide Node.js kullanarak Service Bus ad alanında. 
 

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Boş bir Node.js uygulaması oluşturun. Bir Node.js uygulaması oluşturma hakkında yönergeler için bkz: [oluşturun ve bir Azure Web sitesine bir Node.js uygulaması dağıtma][Create and deploy a Node.js application to an Azure Website], veya [Node.js bulut hizmeti] [ Node.js Cloud Service] Windows PowerShell kullanarak.

## <a name="configure-your-application-to-use-service-bus"></a>Service Bus hizmetini kullanmak için uygulamanızı yapılandırma
Azure Service Bus hizmetini kullanmak için indirin ve Node.js Azure paketi kullanın. Bu paket, hizmet veri yolu REST Hizmetleri ile iletişim kuran bir dizi içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Bir paketi almasını düğüm paketi Yöneticisi'ni (NPM) kullanın
1. Kullanım **Node.js için Windows PowerShell** gitmek için komut penceresinde **c:\\düğüm\\sbqueues\\WebRole1** örneğinizi oluşturduğunuz klasör uygulama.
2. Tür **npm yükleme azure** komut penceresinde, neden aşağıdakine benzer bir çıktı:

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
3. Bir **node_modules** klasörünün oluşturulduğunu doğrulamak için **ls** komutunu kendiniz çalıştırabilirsiniz. Bu klasörün içinde Bul **azure** Service Bus kuyruklarına erişmek için ihtiyacınız olan kitaplıkları içeren paket.

### <a name="import-the-module"></a>Modülü içeri aktarın
Not Defteri veya başka bir metin düzenleyicisi kullanarak, aşağıdaki üst kısmına ekleyin **server.js** uygulamanın dosya:

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Bir Azure Service Bus bağlantısı kurma
Azure modülünü ortam değişkenini okur `AZURE_SERVICEBUS_CONNECTION_STRING` Service Bus'a bağlanmak için gereken bilgileri elde edilir. Bu ortam değişkeni ayarlanmazsa çağırırken hesap bilgilerini belirtmelisiniz `createServiceBusService`.

Ortam değişkenlerini ayarlama bir örnek için [Azure portalında] [ Azure portal] bir Azure Web sitesi için bkz: [depolama ile Node.js Web uygulaması] [ Node.js Web Application with Storage].

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
**ServiceBusService** nesnesi, Service Bus kuyrukları ile çalışmanıza olanak sağlar. Aşağıdaki kod oluşturur bir **ServiceBusService** nesne. En ekleme **server.js** dosya, Azure modülü içeri aktarmak için deyim sonra:

```javascript
var serviceBusService = azure.createServiceBusService();
```

Çağırarak `createQueueIfNotExists` üzerinde **ServiceBusService** nesne, belirtilen sırada döndürülen (varsa) veya belirtilen ada sahip yeni bir kuyruk oluşturulur. Aşağıdaki kod `createQueueIfNotExists` oluşturmak veya adlı kuyruğa bağlanmak `myqueue`:

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

`createServiceBusService` Yöntemi, Canlı ya da en büyük sıra boyutu ileti süresi gibi varsayılan kuyruk ayarlarını geçersiz kılmak etkinleştirdiğiniz ek seçenekler de destekler. Aşağıdaki örnek en büyük sıra boyutu 5 GB ve bir saat 1 dakika (TTL) değerini Canlı ayarlar:

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
İsteğe bağlı filtreleme işlemleri kullanılarak gerçekleştirilen işlemler için uygulanabilir **ServiceBusService**. Filtreleme işlemleri içerebilir günlüğe kaydetme, otomatik olarak yeniden deneniyor, vs. Filtreler, imza ile bir yöntem uygulayan nesnelerdir:

```javascript
function handle (requestOptions, next)
```

Ön işleme istek seçenekleri yaptıktan sonra yöntemini çağırmalı `next`, bir geri çağırma imzayla geçirme:

```javascript
function (returnObject, finalCallback, next)
```

İşleme sonra bu geri çağırma `returnObject` (istek sunucuya yanıt), geri çağırma ya da çağırmalıdır `next` diğer filtrelerle işleme devam etmek için veya çağırma varsa `finalCallback`, hizmet çağrılmasını sonlandırır .

Node.js için Azure SDK ile birlikte yeniden deneme mantığını iki filtre `ExponentialRetryPolicyFilter` ve `LinearRetryPolicyFilter`. Aşağıdaki kod oluşturur bir `ServiceBusService` kullanan nesne `ExponentialRetryPolicyFilter`:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Uygulama çağrılarınızı, bir Service Bus kuyruğuna bir ileti göndermek için `sendQueueMessage` metodunda **ServiceBusService** nesne. Gönderilen (ve öğesinden alınan) hizmet sıraları Bus iletileri **BrokeredMessage** nesneleri ve yüklü bir standart özellikler kümesi (gibi **etiket** ve **TimeToLive**), Özel uygulamaya özgü özellikler ve rastgele uygulama verileri gövdesi tutmak için kullanılan sözlüğü. Bir uygulama, bir dize iletisi olarak geçirerek ileti gövdesini ayarlayabilirsiniz. Gerekli tüm standart özellikleri varsayılan değerlerle doldurulur.

Aşağıdaki örnekte adlı kuyruk için bir test iletisi göndermek nasıl gösterir `myqueue` kullanarak `sendQueueMessage`:

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

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotaları hakkında daha fazla bilgi için bkz: [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Bir kuyruktan ileti alma
İletilerin kullanarak bir sıraya alındığı `receiveQueueMessage` metodunda **ServiceBusService** nesne. Okunurlar gibi varsayılan olarak, ileti kuyruktan silinir; Ancak, (Özet) okuyun ve iletiyi kuyruktan isteğe bağlı parametresi ayarlanarak silmeden kilitleme `isPeekLock` için **true**.

Okuma ve ileti alma işleminin bir parçası olarak silmeye varsayılan davranışı en basit modeldir ve uygulamanın bir arıza olması durumunda bir iletiyi işlememeye izin dayanabilir senaryolar için en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus iletiyi kullanılıyor olarak işaretleyeceğinden, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında ardından onu çökmenin öncesinde kullanılan iletiyi atlamış olur.

Varsa `isPeekLock` parametrenin ayarlanmış **true**, alma, atlanan iletilere veremeyen uygulamaları desteklemenin mümkün kılar bir iki aşamalı işlemi haline gelir. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya güvenilir bir şekilde işlemek üzere depolar sonra) çağırarak alma işleminin ikinci aşamasını tamamlar `deleteMessage` yöntemi ve iletiyi bir parametre olarak silinmesini sağlar. `deleteMessage` Yöntemi iletiyi kullanılıyor olarak işaretler ve kuyruktan kaldırır.

Aşağıdaki örneği kullanarak iletileri almak ve işlemek nasıl gösterir `receiveQueueMessage`. Örnek ilk alır ve bir ileti siler ve ardından bir iletiyi kullanarak alan `isPeekLock` kümesine **true**, ardından iletiyi kullanarak siler `deleteMessage`:

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
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulamanın iletiyi işlemek için herhangi bir nedenle silemiyor sonra çağırabilirsiniz `unlockMessage` metodunda **ServiceBusService** nesne. Bu, Service Bus'ın Kuyruktaki iletinin kilidini açmasına ve iletiyi aynı kullanıcı uygulama veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale neden olur.

Ayrıca kuyrukta kilitlenen iletiye ilişkin bir zaman aşımı yoktur ve uygulama önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus otomatik olarak iletinin kilidini açmasına ve hale kilit zaman aşımı dolmadan tekrar kullanılabilir.

Uygulama iletiyi ancak önce çökmesi durumunda, `deleteMessage` yöntemi çağrılır, ardından yeniden başlatıldığında ileti uygulamaya yeniden teslim edilebilir. Buna genellikle denir *en az bir kez işleme*, diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu işlem, genellikle iletinin teslimat denemelerinde korunan **MessageId** özelliği kullanılarak gerçekleştirilir.

## <a name="next-steps"></a>Sonraki adımlar
Kuyruklar hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın.

* [Kuyruklar, konular ve abonelikler][Queues, topics, and subscriptions]
* [Node için Azure SDK] [ Azure SDK for Node] GitHub deposunu
* [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application to an Azure Website]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
