---
title: Kuyruk Depolama'yı Node.js - Azure depolama kullanma
description: Oluşturmak ve Kuyruklar, silmek için Azure Queue hizmetini kullanmayı öğrenin ve Ekle, Al ve iletilerini silin. Node.js'de yazılmış örnekleri.
services: storage
author: mhopkins-msft
ms.service: storage
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: mhopkins
ms.reviewer: cbrooks
ms.subservice: queues
ms.openlocfilehash: 01afe1ab7b9028f3f77d52f7d6f8ced27f6a79c7
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65142705"
---
# <a name="how-to-use-queue-storage-from-nodejs"></a>Node.js’den Kuyruk depolama kullanma
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Microsoft Azure kuyruk hizmeti kullanılarak yaygın senaryoları gerçekleştirmek nasıl gösterir. Örnek Node.js API'si kullanılarak yazılır. Senaryoları ele alınmaktadır **ekleme**, **gözatma**, **alma**, ve **silme** kuyruk iletileri yanı  **oluşturma ve kuyrukları silme**.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Boş bir Node.js uygulaması oluşturun. Bir Node.js uygulaması oluşturma yönergeleri için bkz: [Azure App Service'te bir Node.js web uygulaması oluşturma](../../app-service/app-service-web-get-started-nodejs.md), [Azure bulut hizmeti için bir Node.js uygulaması derleme ve dağıtma](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) veyaWindowsPowerShellkullanma[ Visual Studio Code'u](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="configure-your-application-to-access-storage"></a>Depolama erişim için uygulamanızı yapılandırma
Azure depolama kullanmak için depolama REST Hizmetleri ile iletişim kuran bir dizi kolaylık içeren Node.js için Azure depolama SDK'sı gerekir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Bir paketi almasını düğüm paketi Yöneticisi'ni (NPM) kullanın
1. Bir komut satırı arabirimi gibi kullanın **PowerShell** (Windows) **Terminal** (Mac) veya **Bash** (Unix), örnek uygulamanızı oluşturduğunuz klasöre gidin.
2. Komut penceresine **npm install azure-storage** yazın. Komutun çıktısı aşağıdaki örneğe benzer.
 
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

3. El ile çalıştırabileceğiniz **ls** doğrulamak için komutu bir **düğüm\_modülleri** klasörü oluşturuldu. Bu klasörün içinde, depolama alanına erişmek için ihtiyaç duyduğunuz kitaplıkları içeren **azure-storage** paketini bulacaksınız.

### <a name="import-the-package"></a>Paketi içeri aktarma
Dosyanın en üstüne ekleyin aşağıdaki Not Defteri veya başka bir metin düzenleyicisi kullanarak **server.js** uygulamanın dosya depolama alanı kullanmayı düşündüğünüz burada:

```javascript
var azure = require('azure-storage');
```

## <a name="setup-an-azure-storage-connection"></a>Bir Azure depolama bağlantısı kurma
Azure modülü AZURE ortam değişkenlerini okur\_depolama\_hesabı ve AZURE\_depolama\_erişim\_anahtarı veya AZURE\_depolama\_bağlantı\_ Azure depolama hesabınıza bağlanmak için gereken bilgi DİZESİ. Bu ortam değişkenleri ayarlanmazsa çağırırken hesap bilgilerini belirtmelisiniz **createQueueService**.

## <a name="how-to-create-a-queue"></a>Nasıl Yapılır: Kuyruk oluşturma
Aşağıdaki kod oluşturur bir **QueueService** kuyrukları ile çalışmanıza olanak sağlayan nesne.

```javascript
var queueSvc = azure.createQueueService();
```

Kullanım **createQueueIfNotExists** yöntemi zaten var veya zaten yoksa, belirtilen ada sahip yeni bir kuyruk oluşturur, belirtilen sırada döndürür.

```javascript
queueSvc.createQueueIfNotExists('myqueue', function(error, results, response){
  if(!error){
    // Queue created or exists
  }
});
```

Kuyruğu oluşturduysanız `result.created` geçerlidir. Kuyruk yoksa `result.created` false'tur.

### <a name="filters"></a>Filtreler
İsteğe bağlı filtreleme işlemleri kullanılarak gerçekleştirilen işlemler için uygulanabilir **QueueService**. Filtreleme işlemleri içerebilir günlüğe kaydetme, otomatik olarak yeniden deneniyor, vs. Filtreler, imza ile bir yöntem uygulayan nesnelerdir:

```javascript
function handle (requestOptions, next)
```

İstek seçenekleri önişleme yaptıktan sonra "İleri" geçirme imzayla bir geri çağırma işlemini çağırmak yöntem gerekir:

```javascript
function (returnObject, finalCallback, next)
```

Bu geri çağırma ve (istek sunucuya yanıt) returnObject işlemden sonra geri çağırma varsa diğer filtrelerle işleme devam etmek için İleri çağırma veya yalnızca finalCallback aksi barındırılan hizmeti kurmak sonlandırmak için çağırma gerekir çağırma.

Yeniden deneme mantığı uygulayan iki filtre (**ExponentialRetryPolicyFilter** ve **LinearRetryPolicyFilter**), Node.js için Azure SDK’sına dahil edilir. Aşağıdaki oluşturur bir **QueueService** kullanan nesne **ExponentialRetryPolicyFilter**:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var queueSvc = azure.createQueueService().withFilter(retryOperations);
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Nasıl Yapılır: Kuyruğa bir ileti Ekle
Bir kuyruğa ileti eklemek için kullanın **CreateMessage nesne** yöntemi yeni bir ileti oluşturun ve kuyruğa ekleyin.

```javascript
queueSvc.createMessage('myqueue', "Hello world!", function(error, results, response){
  if(!error){
    // Message inserted
  }
});
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl Yapılır: Sonraki iletiye gözatın
Kuyruğun iletiyi kuyruktan kaldırmadan çağırarak peek **peekMessages** yöntemi. Varsayılan olarak, **peekMessages** tek bir ileti göz atar.

```javascript
queueSvc.peekMessages('myqueue', function(error, results, response){
  if(!error){
    // Message text is in results[0].messageText
  }
});
```

`result` İleti içerir.

> [!NOTE]
> Kullanarak **peekMessages** kuyrukta ileti olduğunda ileti döndürdü ancak bir hata döndürmez.
> 
> 

## <a name="how-to-dequeue-the-next-message"></a>Nasıl Yapılır: Sonraki iletiyi sıradan çıkar
Bir iletiyi işlemeyi iki aşamalı bir işlemdir:

1. İletiyi sıradan çıkar.
2. İleti Sil.

Bir iletiyi sıradan çıkar kullanın **getMessages**. Diğer istemci bunları işleyebilmek için bu iletiler kuyrukta görünmez hale getirir. Uygulamanızı bir ileti işlediğinde, çağrı **deleteMessage** kuyruktan silinemedi. Aşağıdaki örnek, bir ileti alır, ardından siler:

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
> Varsayılan olarak, bir ileti 30 saniye sonra diğer istemcilere görünür durumda, yalnızca gizlenir. Kullanarak farklı bir değer belirtebilirsiniz `options.visibilityTimeout` ile **getMessages**.
> 
> [!NOTE]
> Kullanarak **getMessages** kuyrukta ileti olduğunda ileti döndürdü ancak bir hata döndürmez.
> 
> 

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl Yapılır: Kuyruğa Alınan iletinin içeriğini değiştirme
Bir ileti kuyruğu kullanarak yerinde içeriğini değiştirebilirsiniz **updateMessage**. Aşağıdaki örnek, bir ileti metni güncelleştirir:

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

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Nasıl Yapılır: İletileri sıradan çıkarmak için ek seçenekler
Bir kuyruktan ileti alma özelleştirebilirsiniz iki yolu vardır:

* `options.numOfMessages` -Alma toplu iletiler (en fazla 32.)
* `options.visibilityTimeout` -Uzun veya daha kısa bir görünmezlik zaman aşımını ayarlayın.

Aşağıdaki örnekte **getMessages** tek çağrıda 15 iletileri almak için yöntemi. Her bir iletiyi kullanarak işler sonra bir for döngüsü. Ayrıca bu yöntem tarafından döndürülen tüm iletiler için beş dakika için görünmezlik zaman aşımı ayarlar.

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

## <a name="how-to-get-the-queue-length"></a>Nasıl Yapılır: Kuyruk uzunluğu alma
**GetQueueMetadata** iletiler kuyrukta yaklaşık sayısı dahil olmak üzere kuyruk hakkındaki meta verileri döndürür.

```javascript
queueSvc.getQueueMetadata('myqueue', function(error, results, response){
  if(!error){
    // Queue length is available in results.approximateMessageCount
  }
});
```

## <a name="how-to-list-queues"></a>Nasıl Yapılır: Kuyrukları listeleme
Kuyrukların listesini almak için kullanın **listQueuesSegmented**. Belirli bir ön eke göre filtrelenmiş bir listesini almak için kullanın **listQueuesSegmentedWithPrefix**.

```javascript
queueSvc.listQueuesSegmented(null, function(error, results, response){
  if(!error){
    // results.entries contains the list of queues
  }
});
```

Tüm Kuyruklar döndürülemez, `result.continuationToken` ilk parametresi olarak kullanılabilmesi için **listQueuesSegmented** veya ikinci parametresi **listQueuesSegmentedWithPrefix** daha fazla sonuç almak için.

## <a name="how-to-delete-a-queue"></a>Nasıl Yapılır: Bir kuyruk silme
Bir kuyruk ve içerdiği tüm iletileri silmek için çağrı **deleteQueue** kuyruk nesnesi üzerinde yöntemi.

```javascript
queueSvc.deleteQueue(queueName, function(error, response){
  if(!error){
    // Queue has been deleted
  }
});
```

Silmeden bir kuyruktan tüm iletileri silmek için kullanın **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Nasıl yapılır: Paylaşılan Erişim İmzaları ile çalışma
Paylaşılan erişim imzaları (SAS) depolama hesabı adı veya anahtarlarının sağlamadan kuyruklar için ayrıntılı erişim sağlamak için güvenli bir yoludur. SAS genellikle iletiler göndermek mobil uygulama izin verme gibi Kuyruklarınızı sınırlı erişim sağlamak için kullanılır.

SAS kullanarak bulut tabanlı bir hizmet gibi güvenilen bir uygulama oluşturur **generateSharedAccessSignature** , **QueueService**, güvenilmeyen veya yarı güvenilir uygulamaya sağlar. Örneğin, bir mobil uygulama. SAS’ın geçerli olduğu başlangıç ve bitiş tarihlerini ve SAS sahibine verilen erişim düzeyini açıklayan bir ilke kullanılarak SAS oluşturulur.

Aşağıdaki örnek, kuyruğa ileti eklemek SAS sahibi izin veren yeni bir paylaşılan erişim ilkesi oluşturur ve 100 dakika oluşturulduktan sonra süresi dolar.

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

SAS sahibi sıranın erişmeye çalıştığında, gerekli olduğu gibi konak bilgilerini ayrıca sağlanması gerektiğini unutmayın.

İstemci uygulama ile SAS kullanan **QueueServiceWithSAS** sıra üzerinde işlemler gerçekleştirmek için. Aşağıdaki örnek, kuyruğa bağlanır ve bir ileti oluşturur.

```javascript
var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
  if(!error){
    //message added
  }
});
```

Okuma, güncelleştirme veya iletileri silme girişiminde yapıldıysa SAS ile erişim Ekle, oluşturulmasının üzerinden bir hata döndürülür.

### <a name="access-control-lists"></a>Erişim denetimi listeleri
Ayrıca SAS için erişim ilkesini ayarlamak istediğinizde de bir Erişim Denetim Listesi (ACL) kullanabilirsiniz. Bu sıranın erişim, ancak her istemci için farklı erişim ilkeleri sağlar, birden çok istemci izin vermek istiyorsanız yararlıdır.

Her politikayla ilişkilendirilmiş bir kimlik ile, bir erişim ilkeleri dizisi kullanılarak ACL uygulanır. Aşağıdaki örnek, iki ilke tanımlar; bir 'Kullanıcı1' ve 'kullanıcı2' için:

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

Aşağıdaki örnek, geçerli ACL'si alır **myqueue**, ardından yeni ilkeleri kullanılarak ekler **setQueueAcl**. Bu yaklaşım aşağıdakilere olanak sağlar:

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

ACL ayarlandıktan sonra bir ilke kimliği dayalı bir SAS oluşturabilirsiniz. Aşağıdaki örnek, 'user2' için yeni bir SAS oluşturur:

```javascript
queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });
```

## <a name="next-steps"></a>Sonraki Adımlar
Kuyruk depolamanın temellerini öğrendiğinize göre daha karmaşık depolama görevleri hakkında bilgi edinmek için bu bağlantıları izleyin.

* Ziyaret [Azure depolama ekibi blogu][Azure Storage Team Blog].
* Ziyaret [düğüm için Azure depolama SDK'sı] [ Azure Storage SDK for Node] github deposu.



[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node

[using the REST API]: https://msdn.microsoft.com/library/azure/hh264518.aspx

[Azure Portal]: https://portal.azure.com

[Azure App Service'te bir Node.js web uygulaması oluşturma](../../app-service/app-service-web-get-started-nodejs.md)

[Bir Node.js uygulaması derleme ve Azure Bulut Hizmeti’ne dağıtma](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md)

[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/

[Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/
