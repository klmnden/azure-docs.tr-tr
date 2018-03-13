---
title: Node.js'den kuyruk depolama kullanma | Microsoft Docs
description: "Oluşturmak ve Kuyruklar silmek için Azure Queue hizmetini kullanmayı öğrenin ve Ekle, Al ve iletilerini silin. Node.js içinde yazılmış örneklerini içerir."
services: storage
documentationcenter: nodejs
author: craigshoemaker
manager: jeconnoc
editor: tysonn
ms.assetid: a8a92db0-4333-43dd-a116-28b3147ea401
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: cshoe
ms.openlocfilehash: 2565f56324a070368c499a62ab54bb98830d8c20
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="how-to-use-queue-storage-from-nodejs"></a>Node.js’den Kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Microsoft Azure Queue hizmetini kullanarak yaygın senaryolar gerçekleştirme gösterir. Örnekler, Node.js API kullanılarak yazılır. Kapsamdaki senaryolar dahil **ekleme**, **gözatma**, **alma**, ve **silme** kuyruk iletileri yanı **oluşturma ve silme**.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Bir Node.js uygulaması oluşturma
Boş bir Node.js uygulaması oluşturun. Bir Node.js uygulaması oluşturma yönergeleri için bkz: [Azure App Service'te bir Node.js web uygulaması oluşturma](../../app-service/app-service-web-get-started-nodejs.md), [derleme ve Azure bulut hizmeti bir Node.js uygulamasını dağıtma](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) Windows PowerShell veya kullanma[ Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="configure-your-application-to-access-storage"></a>Uygulamanızı erişimli depolama için yapılandırın
Azure depolama kullanmak için bir dizi depolama REST Hizmetleri ile iletişim kolaylık kitaplıkları içerir Node.js için Azure depolama SDK'sı gerekir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paket elde etmek için düğüm paketi Yöneticisi (NPM) kullanın
1. Bir komut satırı arabirimi gibi kullandığınız **PowerShell** (Windows) **Terminal** (Mac) veya **Bash** (UNIX) örnek uygulamanızı oluşturduğunuz klasöre gidin.
2. Tür **npm yükleme azure depolama** komut penceresinde. Komut çıktısı aşağıdaki örneğe benzer.
 
    ```bash
    azure-storage@0.5.0 node_modules\azure-storage
    +-- extend@1.2.1
    +-- xmlbuilder@0.4.3
    +-- mime@1.2.11
    +-- node-uuid@1.4.3
    +-- validator@3.22.2
    +-- underscore@1.4.4
    +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
    +-- xml2js@0.2.7 (sax@0.5.2)
    +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    ```

3. El ile çalıştırabilirsiniz **ls** doğrulamak için komutu bir **düğümü\_modülleri** klasörü oluşturuldu. Bu klasör içinde bulacaksınız **azure depolama** depolama birimine erişmesi gereken kitaplıkları içeren paket.

### <a name="import-the-package"></a>Paket alma
Not Defteri'nde veya başka bir metin düzenleyicisi kullanarak, en üstüne aşağıdakileri ekleyin **server.js** uygulamanın dosya depolama kullanmayı düşündüğünüz burada:

```javascript
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a>Bir Azure depolama bağlantı Kur
Azure modülü AZURE ortam değişkenleri okur\_depolama\_HESABINI ve AZURE\_depolama\_erişim\_anahtar ya da AZURE\_depolama\_bağlantı\_Azure depolama hesabınıza bağlanmak için gerekli bilgileri DİZESİ. Bu ortam değişkenleri ayarlanmamışsa çağrılırken hesap bilgileri belirtmelisiniz **createQueueService**.

Ortam değişkenlerini ayarlama örnek için [Azure Portal](https://portal.azure.com) bir Azure Web sitesi için bkz: [Azure tablo hizmeti kullanarak Node.js web uygulaması](../../cosmos-db/table-storage-cloud-service-nodejs.md).

## <a name="how-to-create-a-queue"></a>Nasıl yapılır: bir sıra oluşturun
Aşağıdaki kod oluşturur bir **QueueService** kuyruklarla çalışmanıza olanak tanır nesnesi.

```javascript
var queueSvc = azure.createQueueService();
```

Kullanım **createQueueIfNotExists** zaten var veya zaten yoksa, belirtilen ada sahip yeni bir sıra oluşturur, belirtilen sırada döndüren yöntemi.

```javascript
queueSvc.createQueueIfNotExists('myqueue', function(error, results, response){
  if(!error){
    // Queue created or exists
  }
});
```

Sıranın oluşturduysanız, `result.created` doğrudur. Sıranın varsa `result.created` false olur.

### <a name="filters"></a>Filtreler
İsteğe bağlı filtreleme işlemleri kullanarak gerçekleştirilen işlemler için uygulanabilir **QueueService**. İşlemleri filtreleme içerebilir günlüğe kaydetme, otomatik olarak yeniden deneniyor, vs. İmzalı bir yöntem uygulayan nesneler filtreleri şunlardır:

```javascript
function handle (requestOptions, next)
```

İstek seçenekleri önişleme yaptıktan sonra "İleri" aşağıdaki imzalı bir geri çağırma geçirme çağrılacak yöntemi gerekir:

```javascript
function (returnObject, finalCallback, next)
```

Bu geri çağırma ve (sunucunun istek yanıtı) returnObject işlemden sonra geri çağırma diğer filtreleri işleme devam etmek için varsa sonraki çağırma veya yalnızca finalCallback Aksi halde hizmet başlatma sonuna çağırma gerekiyor.

Yeniden deneme mantığını uygulaması iki filtre Node.js için Azure SDK'sı ile birlikte **ExponentialRetryPolicyFilter** ve **LinearRetryPolicyFilter**. Aşağıdaki oluşturur bir **QueueService** kullanan nesneyi **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Nasıl yapılır: bir sıraya bir ileti Ekle
Kuyruğa bir ileti eklemek için kullanın **CreateMessage nesne** yeni bir ileti oluşturun ve sıraya eklemek için yöntem.

```javascript
queueSvc.createMessage('myqueue', "Hello world!", function(error, results, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl yapılır: sonraki iletiye
Kuyruğun önündeki iletiye sıradan çağırarak kaldırmadan iletiye göz atabilirsiniz **peekMessages** yöntemi. Varsayılan olarak, **peekMessages** tek bir ileti iletiye göz atar.

```javascript
queueSvc.peekMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
  }
});
```

`result` İleti içerir.

> [!NOTE]
> Kullanarak **peekMessages** sıraya ileti olduğunda ileti döndürdü ancak bir hata döndürmez.
> 
> 

## <a name="how-to-dequeue-the-next-message"></a>Nasıl yapılır: sonraki iletiyi sıradan çıkarma
İleti işlenirken iki aşamalı bir işlemdir:

1. İleti dequeue.
2. İletiyi silin.

Bir ileti dequeue için kullanmak **getMessages**. Diğer bir istemcileri onları işleyebilmek için iletiler kuyrukta görünmez kılar. Uygulamanızı bir ileti işlediğinde çağrısı **deleteMessage** kuyruktan silinemiyor. Aşağıdaki örnek bir ileti alır, ardından da siler:

```javascript
queueSvc.getMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
    var message = results[0];
    queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
      if(!error){
        //message deleted
      }
    });
  }
});
```

> [!NOTE]
> Varsayılan olarak, bir ileti 30 saniye, daha sonra diğer istemcilere görünür yalnızca gizlenir. Kullanarak farklı bir değer belirtebilirsiniz `options.visibilityTimeout` ile **getMessages**.
> 
> [!NOTE]
> Kullanarak **getMessages** sıraya ileti olduğunda ileti döndürdü ancak bir hata döndürmez.
> 
> 

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl yapılır: kuyruğa alınan iletinin içeriğini değiştirme
Bir ileti sırası kullanarak yerinde içeriğini değiştirebilirsiniz **updateMessage**. Aşağıdaki örnek ileti metnini güncelleştirir:

```javascript
queueSvc.getMessages('myqueue', function(error, getResults, getResponse){
  if(!error){
    // Got the message
    var message = getResults[0];
    queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, updateResults, updateResponse){
      if(!error){
        // Message updated successfully
      }
    });
  }
});
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Nasıl yapılır: kuyruktan alma için ek seçenekler iletileri
Bir sıradan ileti alma özelleştirebilirsiniz iki yolu vardır:

* `options.numOfMessages` -Almak toplu iletiler (en fazla 32.)
* `options.visibilityTimeout` -Daha uzun veya kısaysa görünmezlik zaman aşımı ayarlayın.

Aşağıdaki örnek kullanır **getMessages** tek çağrıda 15 iletileri almak için yöntemi. Her bir iletiyi kullanarak işler sonra bir döngü için. Ayrıca bu yöntem tarafından döndürülen tüm iletiler için beş dakika için görünmezlik zaman aşımı ayarlar.

```javascript
queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, results, getResponse){
  if(!error){
    // Messages retrieved
    for(var index in result){
      // text is available in result[index].messageText
      var message = results[index];
      queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, deleteResponse){
        if(!error){
          // Message deleted
        }
      });
    }
  }
});
```

## <a name="how-to-get-the-queue-length"></a>Nasıl yapılır: kuyruk uzunluğu alma
**GetQueueMetadata** iletiler kuyrukta yaklaşık sayısı dahil olmak üzere kuyruk hakkındaki meta verileri döndürür.

```javascript
queueSvc.getQueueMetadata('myqueue', function(error, results, response){
  if(!error){
    // Queue length is available in results.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a>Nasıl yapılır: Sorgular listesi
Kuyrukların listesini almak için kullanın **listQueuesSegmented**. Belirli bir önek tarafından filtre uygulanmış bir listesini almak için kullanmak **listQueuesSegmentedWithPrefix**.

```javascript
queueSvc.listQueuesSegmented(null, function(error, results, response){
  if(!error){
    // results.entries contains the list of queues
  }
});
```

Tüm Kuyruklar döndürülemez, `result.continuationToken` ilk parametresi olarak kullanılabilir **listQueuesSegmented** veya öğesinin ikinci parametresi, **listQueuesSegmentedWithPrefix** daha fazla sonuç almak için.

## <a name="how-to-delete-a-queue"></a>Nasıl yapılır: bir kuyruk silme
Bir kuyruk ve içerdiği tüm iletileri silmek için arama **deleteQueue** nesnesinde yöntemi.

```javascript
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

Silmeden bir Sıraya alınan tüm iletileri silmek için kullanın **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Nasıl yapılır: paylaşılan erişim imzaları ile çalışma
Paylaşılan erişim imzaları (SAS) depolama hesabı adı veya anahtarları sağlamadan sıraları ayrıntılı erişim sağlamak için güvenli bir yoludur. SAS genellikle iletiler göndermek bir mobil uygulama izin verme gibi sıralar, sınırlı erişim sağlamak için kullanılır.

SAS kullanarak bulut tabanlı bir hizmete gibi güvenilir bir uygulama oluşturur **generateSharedAccessSignature** , **QueueService**ve güvenilmeyen veya yarı güvenilir uygulama sağlar. Örneğin, bir mobil uygulama. SAS SAS sahibi verilen erişim düzeyini yanı sıra SAS geçerli olduğu başlangıç ve bitiş tarihleri açıklar, bir ilke kullanılarak oluşturulur.

Aşağıdaki örnek, iletileri kuyruğa eklemek SAS sahibi izin veren yeni bir paylaşılan erişim ilkesi oluşturur ve 100 dakika oluşturulduktan sonra süresi dolar.

```javascript
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};

var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
var host = queueSvc.host;
```

SAS sahibi sıranın erişmeyi denediğinde, gerekli olduğu gibi konak bilgileri'nin de, sağlanan gerekir unutmayın.

İstemci uygulama ile SAS kullanan **QueueServiceWithSAS** sıranın karşı işlemlerini gerçekleştirmek için. Aşağıdaki örnek, sıraya bağlanır ve bir ileti oluşturur.

```javascript
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

Okuma, güncelleştirme veya iletileri silme girişiminde yapılmışsa SAS Ekle erişimle oluşturulmasının üzerinden bir hata döndürülür.

### <a name="access-control-lists"></a>Erişim denetimi listeleri
Erişim ilkesi için bir SAS ayarlamak için erişim denetim listesi (ACL) da kullanabilirsiniz. Bu kuyruğa erişim, ancak her istemci için farklı erişim ilkeleri sağlamak birden çok istemciye izin vermek istiyorsanız yararlıdır.

Bir ACL her ilkesiyle ilişkili bir Kimliğe sahip bir dizi erişim ilkeleri kullanılarak uygulanır. Aşağıdaki örnek, iki ilke tanımlar; bir 'Kullanıcı1' ve 'kullanıcı2' için:

```javascript
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Aşağıdaki örnek için geçerli ACL alır **Sıram**, yeni ilkeleri kullanılarak ekler **setQueueAcl**. Bu yaklaşım sağlar:

```javascript
var extend = require('extend');
queueSvc.getQueueAcl('myqueue', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

ACL ayarladıktan sonra daha sonra bir ilke kimliği dayalı bir SAS oluşturabilirsiniz. Aşağıdaki örnek, 'kullanıcı2' için yeni bir SAS oluşturur:

```javascript
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a>Sonraki Adımlar
Kuyruk depolamanın temellerini öğrendiğinize göre daha karmaşık depolama görevleri hakkında bilgi edinmek için aşağıdaki bağlantıları izleyin.

* Ziyaret [Azure depolama ekibi blogu][Azure Storage Team Blog].
* Ziyaret [düğümü için Azure depolama SDK'sı] [ Azure Storage SDK for Node] github'daki.



[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx

[Azure Portal]: https://portal.azure.com

[Azure App Service'te bir Node.js web uygulaması oluşturma](../../app-service/app-service-web-get-started-nodejs.md)

[Bir Node.js uygulaması derleme ve Azure Bulut Hizmeti’ne dağıtma](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md)

[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/

[Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/
