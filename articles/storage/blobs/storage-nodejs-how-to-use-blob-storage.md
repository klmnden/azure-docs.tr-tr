---
title: Node.js'den BLOB storage kullanma | Microsoft Docs
description: "Azure Blob Storage (nesne depolama) ile bulutta yapılandırılmamış veri depolayın."
services: storage
documentationcenter: nodejs
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 8b0df222-1ca8-4967-8248-6d6d720947b8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: e52f38d5fb3c100e4275032f9a2a1234961c672b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blob-storage-from-nodejs"></a>Node.js’den Blob Storage kullanma
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-all](../../../includes/storage-check-out-samples-all.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, Blob storage kullanarak yaygın senaryolar gerçekleştirme gösterilmektedir. Örnekler Node.js API üzerinden yazılır. Kapsamdaki senaryolar karşıya yükleme listesinde, indirin ve BLOB'ları silme nasıl içerir.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Bir Node.js uygulaması oluşturma hakkında daha fazla yönerge için bkz. [oluşturma Azure App Service'te Node.js web uygulamasına] [derleme ve Azure bulut hizmeti bir Node.js uygulamasını dağıtma](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md) --Windows PowerShell kullanarak veya [derleme ve Web Matrix Azure'u Node.js web uygulamasına dağıtma](https://www.microsoft.com/web/webmatrix/).

## <a name="configure-your-application-to-access-storage"></a>Depolama alanına erişmek için uygulamanızı yapılandırın
Azure depolama kullanmak için bir dizi depolama REST Hizmetleri ile iletişim kolaylık kitaplıkları içerir Node.js için Azure depolama SDK'sı gerekir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paket elde etmek için düğüm paketi Yöneticisi (NPM) kullanın
1. Bir komut satırı arabirimi gibi kullandığınız **PowerShell** (Windows) **Terminal** (Mac) veya **Bash** (örnek uygulamanızı oluşturduğunuz klasöre gitmek için UNIX).
2. Tür **npm yükleme azure depolama** komut penceresinde. Aşağıdaki kod örneğinde komut çıktısı benzer.

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
3. El ile çalıştırabilirsiniz **ls** doğrulamak için komutu bir **düğümü\_modülleri** klasörü oluşturuldu. Bu klasör içinde bulma **azure depolama** , depolama erişim için gereken kitaplıklar içeren paket.

### <a name="import-the-package"></a>Paket alma
Not Defteri'nde veya başka bir metin düzenleyicisi kullanarak, aşağıdaki üst kısmına ekleyin **server.js** uygulamanın dosya depolama kullanmayı düşündüğünüz burada:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Bir Azure depolama bağlantı kurma
Azure modülü ortam değişkenleri okur `AZURE_STORAGE_ACCOUNT` ve `AZURE_STORAGE_ACCESS_KEY`, veya `AZURE_STORAGE_CONNECTION_STRING`, Azure depolama hesabınıza bağlanmak için gerekli bilgileri için. Bu ortam değişkenleri ayarlanmamışsa çağrılırken hesap bilgileri belirtmelisiniz **createBlobService**.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma
**BlobService** nesne kapsayıcıları ve blob'larla çalışmanıza olanak sağlar. Aşağıdaki kod oluşturur bir **BlobService** nesnesi. Aşağıdaki üst kısmına yakın ekleyin **server.js**:

```nodejs
var blobSvc = azure.createBlobService();
```

> [!NOTE]
> Kullanarak bir blob anonim olarak erişebilir **createBlobServiceAnonymous** ve ana bilgisayar adresi sağlama. Örneğin, `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.
>
>

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Yeni bir kapsayıcı oluşturmak için kullanın **createContainerIfNotExists**. Aşağıdaki kod örneğinde 'mycontainer' adlı yeni bir kapsayıcı oluşturur:

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
    if(!error){
      // Container exists and is private
    }
});
```

Kapsayıcı yeni oluşturduysanız, `result.created` doğrudur. Kapsayıcı zaten varsa, `result.created` false olur. `response`kapsayıcı ETag bilgiler dahil olmak üzere işlemi hakkında bilgiler içerir.

### <a name="container-security"></a>Kapsayıcı güvenlik
Varsayılan olarak yeni kapsayıcı özeldir ve anonim olarak erişilemez. Böylece anonim olarak erişebilir kapsayıcı genel hale getirmek üzere, kapsayıcının erişim düzeyini ayarlayabilir **blob** veya **kapsayıcı**.

* **BLOB** -blob içeriğinin ve bu kapsayıcı içindeki meta verileri, ancak bir kapsayıcıdaki tüm blob'lara listeleme gibi değil kapsayıcı meta verileri için anonim okuma erişimi sağlar
* **kapsayıcı** -kapsayıcı meta verileri yanı sıra blob içeriğinin ve meta verileri için anonim okuma erişimi sağlar

Aşağıdaki kod örneğinde erişim düzeyini ayarlama gösterilmektedir **blob**:

```nodejs
blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
    if(!error){
      // Container exists and allows
      // anonymous read access to blob
      // content and metadata within this container
    }
});
```

Alternatif olarak, bir kapsayıcı erişim düzeyini kullanarak değiştirebileceğiniz **setContainerAcl** erişim düzeyini belirtir. Aşağıdaki kod örneğinde kapsayıcıya erişim düzeyi değişir:

```nodejs
blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
  if(!error){
    // Container access level set to 'container'
  }
});
```

Sonuç geçerli dahil olmak üzere işlemi hakkında bilgi içeren **ETag** kapsayıcısı için.

### <a name="filters"></a>Filtreler
Kullanılarak gerçekleştirilen işlemler için isteğe bağlı filtreleme işlemleri uygulayabilirsiniz **BlobService**. İşlemleri filtreleme içerebilir günlüğe kaydetme, otomatik olarak yeniden deneniyor, vs. İmzalı bir yöntem uygulayan nesneler filtreleri şunlardır:

```nodejs
function handle (requestOptions, next)
```

İstek seçenekleri önişleme yaptıktan sonra yöntemi "İleri", çağırmak bir geri çağırma aşağıdaki imzayla geçirme gerekir:

```nodejs
function (returnObject, finalCallback, next)
```

Bu geri çağırma ve (sunucunun istek yanıtı) returnObject işlemden sonra diğer filtrelerle işleme devam etmek için varsa sonraki çağırma ya da yalnızca hizmet başlatma sonuna finalCallback çağırmak geri çağırma gerekir.

Yeniden deneme mantığını uygulaması iki filtre Node.js için Azure SDK'sı ile birlikte **ExponentialRetryPolicyFilter** ve **LinearRetryPolicyFilter**. Aşağıdaki oluşturur bir **BlobService** kullanan nesneyi **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var blobSvc = azure.createBlobService().withFilter(retryOperations);
```

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Üç BLOB türü vardır: blok blobları, sayfa blobları ve ilave blobları. Blok blobları büyük verileri daha verimli bir şekilde karşıya izin verir. Ekleme blobları için iyileştirilmiş ekleme işlemleri. Sayfa bloblarını okuma/yazma işlemleri için en iyi duruma getirilir. Daha fazla bilgi için bkz: [anlama blok Blobları, ekleme Blobları ve sayfa Bloblarını](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Blok blobları
Verileri bir blok blobuna yüklemek için aşağıdakileri kullanın:

* **createBlockBlobFromLocalFile** - yeni bir blok blobu oluşturur ve bir dosyanın içeriğini yükler
* **createBlockBlobFromStream** - yeni bir blok blobu oluşturur ve bir akış içeriğini yükler
* **createBlockBlobFromText** - yeni bir blok blobu oluşturur ve bir dize içeriğini yükler
* **createWriteStreamToBlockBlob** -blok blobu yazma akışına sağlar

Aşağıdaki kod örneğinde yükleyen içeriğini **sınama.txt** içine dosya **myblob**.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

`result` Bu yöntemler tarafından döndürülen gibi işlemler hakkında bilgi içeren **ETag** BLOB.

### <a name="append-blobs"></a>Ekleme blobları
Yeni bir ek blob verileri yüklemek için aşağıdakileri kullanın:

* **createAppendBlobFromLocalFile** - yeni bir ek blob oluşturur ve bir dosyanın içeriğini yükler
* **createAppendBlobFromStream** - yeni bir ek blob oluşturur ve bir akış içeriğini yükler
* **createAppendBlobFromText** - yeni bir ek blob oluşturur ve bir dize içeriğini yükler
* **createWriteStreamToNewAppendBlob** - yeni bir ek blob oluşturur ve ardından ona yazmak için bir akış sağlar

Aşağıdaki kod örneğinde yükleyen içeriğini **sınama.txt** içine dosya **myappendblob**.

```nodejs
blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

Bir blok var olan bir ek blobu eklemek için aşağıdakileri kullanın:

* **appendFromLocalFile** -mevcut bir ek blobu bir dosyanın içeriğini ekleme
* **appendFromStream** -mevcut bir ek blobu bir akış içeriğini ekleme
* **appendFromText** -mevcut bir ek blobu bir dize içeriğini ekleme
* **appendBlockFromStream** -mevcut bir ek blobu bir akış içeriğini ekleme
* **appendBlockFromText** -mevcut bir ek blobu bir dize içeriğini ekleme

> [!NOTE]
> appendFromXXX API'leri, bazı istemci tarafı doğrulama hızlı gereksiz sunucu çağrılarını önlemek başarısız yapar. appendBlockFromXXX olmaz.
>
>

Aşağıdaki kod örneğinde yükleyen içeriğini **sınama.txt** içine dosya **myappendblob**.

```nodejs
blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
  if(!error){
    // text appended
  }
});
```

### <a name="page-blobs"></a>Sayfa blobları
Bir sayfa blob'u verileri yüklemek için aşağıdakileri kullanın:

* **createPageBlob** -belirli bir uzunlukta yeni bir sayfa blob'u oluşturur
* **createPageBlobFromLocalFile** - yeni bir sayfa blob'u oluşturur ve bir dosyanın içeriğini yükler
* **createPageBlobFromStream** - yeni bir sayfa blob'u oluşturur ve bir akış içeriğini yükler
* **createWriteStreamToExistingPageBlob** -mevcut bir sayfa blobu yazma akışına sağlar
* **createWriteStreamToNewPageBlob** - yeni bir sayfa blob'u oluşturur ve ardından ona yazmak için bir akış sağlar

Aşağıdaki kod örneğinde yükleyen içeriğini **sınama.txt** içine dosya **mypageblob**.

```nodejs
blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
  if(!error){
    // file uploaded
  }
});
```

> [!NOTE]
> Sayfa bloblarını 512 bayt 'sayfalar' oluşur. 512 birden fazla olmayan bir boyutu ile verileri karşıya yüklenirken bir hata alırsınız.
>
>

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme
BLOB'ları bir kapsayıcıda listelemek için kullanın **listBlobsSegmented** yöntemi. BLOB'lar ile belirli bir önek dönmek istiyorsanız kullanın **listBlobsSegmentedWithPrefix**.

```nodejs
blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
  if(!error){
      // result.entries contains the entries
      // If not all blobs were returned, result.continuationToken has the continuation token.
  }
});
```

`result` İçeren bir `entries` her bir blob açıklayan nesnelerinin bir dizisidir koleksiyonu. Tüm BLOB'lar döndürülemez, `result` de sağlar bir `continuationToken`, ikinci parametre olarak ek girişlerini almak için kullandığınız.

## <a name="download-blobs"></a>Blob’ları indirme
Bir blob üzerinden veri indirmek için aşağıdakileri kullanın:

* **getBlobToLocalFile** -blob içeriği dosyaya yazar
* **getBlobToStream** -blob içeriklerini bir akışa Yazar
* **getBlobToText** -blob içeriklerini bir dizeye Yazar
* **createReadStream** -blobundan okumak için bir akış sağlar

Aşağıdaki kod örneğinde kullanımı gösterilir **getBlobToStream** içeriğini indirmek için **myblob** blob ve onun için depolamak **çýktý.txt** bir akış kullanarak dosya:

```nodejs
var fs = require('fs');
blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
  if(!error){
    // blob retrieved
  }
});
```

`result` Blob hakkında bilgi içeren dahil olmak üzere **ETag** bilgi.

## <a name="delete-a-blob"></a>Blob silme
Son olarak, bir blobu silmek için çağrı **deleteBlob**. Aşağıdaki kod örneğinde adlı blob siler **myblob**.

```nodejs
blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
  if(!error){
    // Blob has been deleted
  }
});
```

## <a name="concurrent-access"></a>Eş zamanlı erişim
Eş zamanlı erişim bir blob'a birden çok istemciden veya birden çok örneği desteklemek için kullanabileceğiniz **Etag'ler** veya **kiraları**.

* **ETag** -blob veya kapsayıcı başka bir işlem tarafından değiştirilmiş algılamak için bir yol sağlar
* **Kira** -elde özel ve yenilenebilir, yazma erişimi veya bir blob için bir süre silmek için bir yol sağlar

### <a name="etag"></a>ETag
Aynı anda birden çok istemciler veya örnekleri Blob veya sayfa bloğuna yazmaya izin vermeniz gerekiyorsa, kullanım Etag'ler Blob. ETag başlangıçta okumak veya başka bir istemci ya da işlem tarafından kaydedilen değişikliklerin üzerine önlemek izin veren, oluşturulan beri değiştirilmiş kapsayıcı veya blob ise belirlemenize olanak tanır.

İsteğe bağlı kullanarak ETag koşulları ayarlayabilirsiniz `options.accessConditions` parametresi. Aşağıdaki kod örneği yalnızca karşıya **sınama.txt** blob zaten var ve ETag değerine sahip dosyasında alan tarafından `etagToMatch`.

```nodejs
blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
    if(!error){
    // file uploaded
  }
});
```

Etag'ler kullanırken genel modeli aşağıdaki gibidir:

1. ETag oluştur, liste veya alma işlemi sonucu olarak alın.
2. ETag değeri değiştirilmedi denetimi bir eylemi gerçekleştirir.

Değer değiştirilmişse, bu ETag değeri elde olduğundan başka bir istemci veya örnek blob veya kapsayıcı değiştirildiğini gösterir.

### <a name="lease"></a>Kira
Kullanarak yeni bir kira elde edebilirsiniz **acquireLease** yöntemi, üzerinde bir kira elde etmek ister misiniz blob veya kapsayıcı belirtme. Örneğin, aşağıdaki kod üzerinde bir kira alması **myblob**.

```nodejs
blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
  if(!error) {
    console.log('leaseId: ' + result.id);
  }
});
```

Sonraki işlemlerde **myblob** sağlamalısınız `options.leaseId` parametresi. Kimliği olarak döndürülür kira `result.id` gelen **acquireLease**.

> [!NOTE]
> Varsayılan olarak, kira süresi sonsuzdur. Sağlayarak (arasındaki 15 ila 60 saniye) sonsuz olmayan süre belirtebilirsiniz `options.leaseDuration` parametresi.
>
>

Bir kira kaldırmak için kullanın **releaseLease**. Bir kira sonu, ancak diğer özgün süresi sona erinceye kadar yeni bir kira ele geçirmesini önlemek için kullanmak **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Paylaşılan erişim imzaları ile çalışma
Paylaşılan erişim imzaları (SAS) depolama hesabı adı veya anahtarları sağlamadan BLOB'ları ve kapsayıcıları ayrıntılı erişim sağlamak için güvenli bir yoludur. Paylaşılan erişim imzalar, genellikle bir mobil uygulamanın blob'lara erişmesine izin verme gibi verilerinizi sınırlı erişim sağlamak için kullanılır.

> [!NOTE]
> Blobları anonim erişime izin verebilir olsa da, paylaşılan erişim imzaları SAS oluşturmak gibi denetimli erişim sağlamak izin verin.
>
>

Bir bulut tabanlı hizmeti gibi güvenilir bir uygulama kullanarak paylaşılan erişim imzaları oluşturur **generateSharedAccessSignature** , **BlobService**ve bir mobil uygulama gibi güvenilmeyen veya yarı güvenilir bir uygulama sağlar. Paylaşılan erişim imzaları başlangıç tanımlayan bir ilke kullanılarak oluşturulan ve bitiş tarihleri paylaşılan erişim imzaları geçerli olarak paylaşılan erişim imzaları sahibi için erişim düzeyi verilir.

Aşağıdaki kod örneği üzerinde okuma işlemleri gerçekleştirmek için paylaşılan erişim imzaları sahibi izin veren yeni bir paylaşılan erişim ilkesi oluşturur **myblob** blob ve 100 dakika sonra onu oluşturulduğu zaman geçerli olsun.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
};

var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
var host = blobSvc.host;
```

Paylaşılan erişim imzaları sahibi kapsayıcı erişmeyi denediğinde, gerekli olduğu gibi konak bilgileri'nin de, sağlanan gerekir unutmayın.

İstemci uygulama ile paylaşılan erişim imzalarını kullanır **BlobServiceWithSAS** blob karşı işlemlerini gerçekleştirmek için. Aşağıdaki bilgiler alır **myblob**.

```nodejs
var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
  if(!error) {
    // retrieved info
  }
});
```

Blob değiştirmek için bir girişimde, salt okunur erişim ile paylaşılan erişim imzaları oluşturulduktan sonra bir hata döndürülür.

### <a name="access-control-lists"></a>Erişim denetimi listeleri
Erişim ilkesi için SAS ayarlamak için bir erişim denetimi listesi (ACL) da kullanabilirsiniz. Bu, bir kapsayıcı erişimi olan ancak her istemci için farklı erişim ilkeleri sağlamak birden çok istemciye izin vermek istiyorsanız yararlıdır.

Bir ACL her ilkesiyle ilişkili bir Kimliğe sahip bir dizi erişim ilkeleri kullanılarak uygulanır. Aşağıdaki kod örneği iki ilke, 'User1' diğeri için 'kullanıcı2' tanımlar:

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

Aşağıdaki kod örneği için geçerli ACL alır **mycontainer**ve ardından yeni ilkeleri kullanılarak ekler **setBlobAcl**. Bu yaklaşım sağlar:

```nodejs
var extend = require('extend');
blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
  if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

ACL ayarladıktan sonra daha sonra bir ilke kimliği temel paylaşılan erişim imzaları oluşturabilirsiniz. Aşağıdaki kod örneğinde 'kullanıcı2' yeni paylaşılan erişim imzaları oluşturur:

```nodejs
blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın.

* [Azure Depolama düğümü API Başvurusu için SDK] [Azure Depolama düğümü API Başvurusu için SDK]  
* [Azure depolama ekibi blogu] [Azure depolama ekibi blogu]  
* [Azure depolama SDK'sı düğüm için] [ Azure Storage SDK for Node] github'daki  
* [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/)  
* [AzCopy komut satırı yardımcı programı ile veri aktarımı](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)  

[Azure Storage SDK for Node]: https://github.com/Azure/azure-storage-node  

[Build and deploy a Node.js web app to Azure using Web Matrix]: https://www.microsoft.com/web/webmatrix/  
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx  
[Azure portal]: https://portal.azure.com  
[Derleme ve Azure bulut hizmeti bir Node.js uygulamasını dağıtma](../../cloud-services/cloud-services-nodejs-develop-deploy-app.md)  
[Azure depolama ekibi blogu]: http://blogs.msdn.com/b/windowsazurestorage/  
[Azure Depolama düğümü API Başvurusu için SDK]: http://dl.windowsazure.com/nodestoragedocs/index.html  
