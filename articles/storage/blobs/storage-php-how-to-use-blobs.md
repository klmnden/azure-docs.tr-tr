---
title: Php'den BLOB storage (nesne depolama) kullanma | Microsoft Docs
description: "Azure Blob Storage (nesne depolama) ile bulutta yapılandırılmamış veri depolayın."
documentationcenter: php
services: storage
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 1af56b59-b3f0-4b46-8441-aab463ae088e
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 9de2f7e81d75669267fe6448030c118d06b3f88a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-blob-storage-from-php"></a>Php'den BLOB storage kullanma
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Genel Bakış
Azure Blob Storage, bulutta nesne/blob olarak yapılandırılmamış veri depolayan bir hizmettir. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır.

Bu kılavuz Azure blob hizmeti kullanılarak yaygın senaryolar gerçekleştirme gösterir. PHP ve kullanım örnekleri yazılır [PHP için Azure Storage istemci Kitaplığı][download]. Kapsamdaki senaryolar dahil **karşıya**, **listeleme**, **indirme**, ve **silme** BLOB'lar. BLOB'ları hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma
Azure blob hizmete erişen bir PHP uygulaması oluşturmak için yalnızca sınıflarda başvuran gereksinimdir [PHP için Azure Storage istemci Kitaplığı] [ download] gelen kodunuzu içinde. Not Defteri dahil olmak üzere uygulamanızı oluşturmak için tüm geliştirme araçlarını kullanabilirsiniz.

Bu kılavuzda, bir PHP uygulamasının içinde yerel olarak veya bir Azure web rolü, çalışan rolü veya Web sitesi içinde çalışan kodu adlı Blob Depolama hizmeti özellikleri kullanın.

## <a name="get-the-azure-client-libraries"></a>Azure istemci kitaplıkları Al
[!INCLUDE [get-client-libraries](../../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Blob hizmetine erişmek için uygulamanızı yapılandırın
Azure blob hizmeti API'ları kullanmak için aktarmanız gerekir:

1. Otomatik Yükleyiciden kullanarak dosya başvuru [require_once] deyimi, ve
2. Kullanabileceğinize sınıfları başvuru.

Aşağıdaki örnek otomatik Yükleyiciden dosya ve başvuru dahil gösterilmektedir **ServicesBuilder** sınıfı.

```php
require_once 'vendor/autoload.php';
use MicrosoftAzure\Storage\Common\ServicesBuilder;
```

Aşağıdaki örneklerde `require_once` deyimi her zaman gösterilir, ancak yalnızca örnek yürütmek gerekli sınıfları başvurulur.

## <a name="set-up-an-azure-storage-connection"></a>Bir Azure depolama bağlantı kurma
Bir Azure blob hizmeti istemcisi örneği oluşturmak için öncelikle geçerli bir bağlantı dizesi olması gerekir. Blob hizmeti bağlantı dizesini biçimdedir:

Canlı hizmetine erişmek için:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Depolama öykünücüsü erişmek için:

```php
UseDevelopmentStorage=true
```

Herhangi bir Azure hizmeti istemcisi oluşturmak için kullanmanız gerekir **ServicesBuilder** sınıfı. Şunları yapabilirsiniz:

* doğrudan bağlantı dizesi geçirin veya
* Ortam değişkenleri Web uygulamanız bağlantı dizesini depolamak için kullanın. Bkz: [Azure web uygulaması yapılandırma ayarları](../../app-service/web-sites-configure.md) bağlantı dizelerini yapılandırma belge.

Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

A **BlobRestProxy** nesnesi ile bir blob kapsayıcı oluşturun olanak tanır **createContainer** yöntemi. Bir kapsayıcı oluştururken, kapsayıcı seçeneklerini ayarlayabilirsiniz, ancak bunun nedenle gerekli değildir. (Aşağıdaki örnek kapsayıcı erişim denetim listesi (ACL) ve kapsayıcı meta verileri nasıl ayarlanacağını gösterir.)

```php
require_once 'vendor\autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
use MicrosoftAzure\Storage\Common\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


// OPTIONAL: Set public access policy and metadata.
// Create container options object.
$createContainerOptions = new CreateContainerOptions();

// Set public access policy. Possible values are
// PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
// CONTAINER_AND_BLOBS:
// Specifies full public read access for container and blob data.
// proxys can enumerate blobs within the container via anonymous
// request, but cannot enumerate containers within the storage account.
//
// BLOBS_ONLY:
// Specifies public read access for blobs. Blob data within this
// container can be read via anonymous request, but container data is not
// available. proxys cannot enumerate blobs within the container via
// anonymous request.
// If this value is not specified in the request, container data is
// private to the account owner.
$createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

// Set container metadata.
$createContainerOptions->addMetaData("key1", "value1");
$createContainerOptions->addMetaData("key2", "value2");

try    {
    // Create container.
    $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Çağırma **setPublicAccess (PublicAccessType::CONTAINER\_ve\_BLOB'lar)** kapsayıcı ve blob verilerini anonim istekler aracılığıyla erişilebilir hale getirir. Çağırma **setPublicAccess(PublicAccessType::BLOBS_ONLY)** yalnızca blob veri anonim istekler erişilebilir hale getirir. Kapsayıcı ACL'ler hakkında daha fazla bilgi için bkz: [kümesi kapsayıcı ACL (REST API'si)][container-acl].

Blob hizmeti hata kodları hakkında daha fazla bilgi için bkz: [Blob hizmeti hata kodları][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Bir BLOB dosya karşıya yüklemek için kullanmak **BlobRestProxy -> createBlockBlob** yöntemi. Bu işlem mevcut değil veya varsa üzerine yazar blob oluşturur. Aşağıdaki kod örneğinde kapsayıcısı zaten oluşturulmuş ve kullandığı varsayar [fopen] [ fopen] dosyasını bir akış olarak açın.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

$content = fopen("c:\myfile.txt", "r");
$blob_name = "myblob";

try    {
    //Upload blob
    $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Önceki örnek bir akış olarak bir blob'u karşıya unutmayın. Ancak, bir blob ayrıca bir dize kullanmak, örneğin, yüklenebilir [dosya\_almak\_içeriği] [ file_get_contents] işlevi. Önceki örneği kullanarak bunu değiştirmek `$content = fopen("c:\myfile.txt", "r");` için `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme
BLOB'ları bir kapsayıcıda listelemek için kullanın **BlobRestProxy -> listBlobs** yöntemi ile bir **foreach** döngü için döngü sonucu. Aşağıdaki kod bir kapsayıcıda çıktı olarak her bir blob adını görüntüler ve tarayıcıya URI'sini görüntüler.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // List blobs.
    $blob_list = $blobRestProxy->listBlobs("mycontainer");
    $blobs = $blob_list->getBlobs();

    foreach($blobs as $blob)
    {
        echo $blob->getName().": ".$blob->getUrl()."<br />";
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="download-a-blob"></a>Blob indirme
Bir blob indirmek için arama **BlobRestProxy -> getBlob** yöntemi,'ı çağırın **getContentStream** elde edilen yöntemi **GetBlobResult** nesnesi.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


try    {
    // Get blob.
    $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
    fpassthru($blob->getContentStream());
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Yukarıdaki örnekte bir akış kaynağı (varsayılan davranış) olarak bir blob alır unutmayın. Ancak, kullanabileceğiniz [akış\_almak\_içeriği] [ stream-get-contents] döndürülen akışa bir dizeye dönüştürmek için işlevi.

## <a name="delete-a-blob"></a>Blob silme
Bir blobu silmek için blob adını ve kapsayıcı adı geçirmek **BlobRestProxy -> deleteBlob**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // Delete blob.
    $blobRestProxy->deleteBlob("mycontainer", "myblob");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-a-blob-container"></a>Bir blob kapsayıcısından silin
Son olarak, bir blob kapsayıcısını silmek için kapsayıcı adına geçirmek **BlobRestProxy -> deleteContainer**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create blob REST proxy.
$blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

try    {
    // Delete container.
    $blobRestProxy->deleteContainer("mycontainer");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179439.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure blob hizmeti temel bilgileri öğrendiğinize göre daha karmaşık depolama görevleri hakkında bilgi edinmek için aşağıdaki bağlantıları izleyin.

* Ziyaret [depolama PHP istemci kitaplığı için API Başvurusu](http://azure.github.io/azure-storage-php/)
* Bkz: [Gelişmiş Blob örnek](https://github.com/Azure/azure-storage-php/blob/master/samples/BlobSamples.php).

Daha fazla bilgi için Ayrıca bkz. [PHP Geliştirici Merkezi](/develop/php/).

[download]: https://github.com/Azure/azure-storage-php
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
