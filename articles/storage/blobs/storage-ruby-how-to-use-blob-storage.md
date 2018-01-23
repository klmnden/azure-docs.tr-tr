---
title: Ruby'den BLOB storage (nesne depolama) kullanma | Microsoft Docs
description: "Azure Blob Storage (nesne depolama) ile bulutta yapılandırılmamış veri depolayın."
services: storage
documentationcenter: ruby
author: tamram
manager: timlt
editor: tysonn
ms.assetid: e2fe4c45-27b0-4d15-b3fb-e7eb574db717
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 01/18/2018
ms.author: tamram
ms.openlocfilehash: c4c6d47511acdae7afaf4a535c24c6fcc7e389b1
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-use-blob-storage-from-ruby"></a>Ruby’den Blob Storage kullanma
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Genel Bakış
Azure Blob Storage, bulutta nesne/blob olarak yapılandırılmamış veri depolayan bir hizmettir. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır.

Bu kılavuz yaygın senaryolar Blob storage kullanarak gerçekleştirmek nasıl yapacağınızı gösterir. Örnekler, Ruby API kullanılarak yazılır. Kapsamdaki senaryolar dahil **karşıya yükleme, listeleme, indirme,** ve **silme** BLOB'lar.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby uygulaması oluşturma
Bir Ruby uygulaması oluşturun. Yönergeler için bkz: [Linux'ta App Service'te bir Ruby uygulaması oluşturma](https://docs.microsoft.com/azure/app-service/containers/quickstart-ruby).


## <a name="configure-your-application-to-access-storage"></a>Depolama alanına erişmek için uygulamanızı yapılandırın
Azure Storage kullanmak için indirme ve storage REST Hizmetleri ile iletişim kuran bir dizi kolaylık içerir Söyleniş azure paketini kullanmak gerekir.

### <a name="use-rubygems-to-obtain-the-package"></a>Paket elde etmek için RubyGems kullanın
1. Bir komut satırı arabirimi gibi kullandığınız **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (UNIX).
2. "Gem yükle azure storage blobuna" gem ve bağımlılıklarını yüklemek için komut penceresinde yazın.

### <a name="import-the-package"></a>Paket alma
Sık kullandığınız metin Düzenleyicisi'ni kullanarak aşağıdaki depolama kullanmak istediğiniz yere Söyleniş dosyasının üstüne ekleyin:

```ruby
require "azure/storage/blob"
```

## <a name="set-up-an-azure-storage-connection"></a>Bir Azure depolama bağlantı kurma
Azure modülü ortam değişkenleri okur **AZURE\_depolama\_hesap** ve **AZURE\_depolama\_ACCESS_KEY** Azure depolama hesabınıza bağlanmak için gerekli bilgileri için. Bu ortam değişkenleri ayarlanmamışsa, hesap bilgileri kullanarak belirtmelisiniz **Azure::Blob::BlobService:: oluşturmak** aşağıdaki kod ile:

```ruby
blob_client = Azure::Storage::Blob::BlobService.create(
    storage_account_name: account_name,
    storage_access_key: account_key
    )
```

Klasik veya Resource Manager depolama hesabı Azure portalında bu değerleri almak için:

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Kullanmak istediğiniz depolama hesabınıza gidin.
3. Sağ taraftaki ayarları dikey penceresinde tıklayın **erişim tuşları**.
4. Görüntülenen erişim anahtarları dikey penceresinde, erişim tuşu 1 ve 2 erişim anahtarı görürsünüz. Bunlardan kullanabilirsiniz.
5. Anahtarı panoya kopyalamak için Kopyala simgesine tıklayın.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

**Azure::Storage::Blob::BlobService** nesne kapsayıcıları ve blob'larla çalışmanıza olanak sağlar. Bir kapsayıcı oluşturmak için kullanmak **oluşturma\_container()** yöntemi.

Aşağıdaki kod örneği, bir kapsayıcı oluşturur veya hata varsa yazdırır.

```ruby
azure_blob_service = Azure::Storage::Blob::BlobService.create_from_env
begin
    container = azure_blob_service.create_container("test-container")
rescue
    puts $!
end
```

Dosya kapsayıcıda genel hale getirmek istiyorsanız, kapsayıcının izinlerini ayarlayabilirsiniz.

Yalnızca değiştirebileceğiniz <strong>oluşturma\_container()</strong> geçirmek için çağrı **: ortak\_erişim\_düzeyi** seçeneği:

```ruby
container = azure_blob_service.create_container("test-container",
    :public_access_level => "<public access level>")
```

İçin geçerli değerler **: ortak\_erişim\_düzeyi** seçeneği şunlardır:

* **BLOB:** BLOB'lar için herkese okuma erişimi belirtir. Bu kapsayıcı içindeki BLOB verilerini anonim istek okunabilir ancak kapsayıcı verileri mevcut değil. İstemcilerin anonim istek aracılığıyla kapsayıcıdaki blobları numaralandırılamıyor.
* **kapsayıcı:** tam ortak okuma erişimi için kapsayıcı ve blob verilerini belirtir. İstemcilerin anonim istek aracılığıyla kapsayıcıdaki blobları sıralayabilirsiniz, ancak depolama hesabı kapsayıcılara numaralandırılamıyor.

Alternatif olarak, bir kapsayıcı genel erişim düzeyini kullanarak değiştirebileceğiniz **ayarlamak\_kapsayıcı\_acl()** yöntemi ortak erişim düzeyini belirtin.

Aşağıdaki kod örneğinde genel erişim düzeyini değiştirir **kapsayıcı**:

```ruby
azure_blob_service.set_container_acl('test-container', "container")
```

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Bir blobu içerik yüklemek için kullanmak **oluşturma\_blok\_blob()** blob oluşturmak için yöntemi, blob içeriği olarak bir dosya veya dize kullanın.

Aşağıdaki kod dosya yüklemeleri **test.png** "görüntü-blob'u" kapsayıcısında adlı yeni bir blob olarak.

```ruby
content = File.open("test.png", "rb") { |file| file.read }
blob = azure_blob_service.create_block_blob(container.name,
    "image-blob", content)
puts blob.name
```

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme
Kapsayıcılar listelemek için kullanın **list_containers()** yöntemi.
Bir kapsayıcıdaki blobları listelemek için kullanın **listesi\_blobs()** yöntemi. Bir kapsayıcıdaki tüm blobları listelemek için hizmet tarafından döndürülen bir devamlılık belirteci izleyin ve bu belirteciyle list_blobs çalıştırmaya devam edin. Bkz: [listesi BLOB'lar REST API](https://docs.microsoft.com/rest/api/storageservices/list-blobs) Ayrıntılar için.

Aşağıdaki kod tüm BLOB'ları bir kapsayıcıda çıkarır.

```ruby
nextMarker = nil
loop do
    blobs = azure_blob_service.list_blobs(container_name, { marker: nextMarker })
    blobs.each do |blob|
        puts "\tBlob name #{blob.name}"
    end
    nextMarker = blobs.continuation_token
    break unless nextMarker && !nextMarker.empty?
end
```

## <a name="download-blobs"></a>Blob’ları indirme
BLOB'ları indirmek için kullanacağınız **almak\_blob()** içeriği alma yöntemi.

Aşağıdaki kod örneğinde kullanımı gösterilir **almak\_blob()** "görüntü blob'u" içeriğini indirmek ve yerel bir dosyaya yazmak için.

```ruby
blob, content = azure_blob_service.get_blob(container.name,"image-blob")
File.open("download.png","wb") {|f| f.write(content)}
```

## <a name="delete-a-blob"></a>Bir Blob Sil
Son olarak, bir blobu silmek için kullanın **silmek\_blob()** yöntemi. Aşağıdaki kod örneği, bir blobu silmek gösterilmiştir.

```ruby
azure_blob_service.delete_blob(container.name, "image-blob")
```

## <a name="next-steps"></a>Sonraki adımlar
Daha karmaşık depolama görevleri hakkında bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [Azure Depolama Ekibi Blog’u](http://blogs.msdn.com/b/windowsazurestorage/)
* [Ruby için Azure depolama SDK](https://github.com/azure/azure-storage-ruby) github'daki
* [AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

