---
title: Python'dan Azure Blob storage (nesne depolama) kullanma | Microsoft Docs
description: "Azure Blob Storage (nesne depolama) ile bulutta yapılandırılmamış veri depolayın."
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 0348e360-b24d-41fa-bb12-b8f18990d8bc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 2/24/2017
ms.author: tamram
ms.openlocfilehash: ae5ad68929a6779ed4944de82a609321a5c4b5ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-azure-blob-storage-from-python"></a>Python'dan Azure Blob storage kullanma
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Genel Bakış
Azure Blob Storage, bulutta nesne/blob olarak yapılandırılmamış veri depolayan bir hizmettir. Blob Storage belge, medya dosyası veya uygulama yükleyici gibi her tür metin veya ikili veri depolayabilir. Blob Storage aynı zamanda nesne depolama olarak adlandırılır.

Bu makalede yaygın senaryolar Blob storage kullanarak gerçekleştirmek nasıl yapacağınızı gösterir. Python ve kullanım örnekleri yazılır [Python için Microsoft Azure depolama SDK]. Kapsamdaki senaryolar karşıya yükleme, listeleme, indirme ve BLOB'ları silme içerir.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>İndirme ve Python için Azure depolama SDK yükleme

Python için Azure depolama SDK Python 2.7, 3.3, 3.4, 3.5 veya 3.6 gerektirir ve 4 farklı paketlerde gelir: `azure-storage-blob`, `azure-storage-file`, `azure-storage-table` ve `azure-storage-queue`. Bu öğreticide biz kullanacaksanız `azure-storage-blob` paket.
 
### <a name="install-via-pypi"></a>Pypı yüklemek

Python paket dizinini (Pypı) yüklemek için şunu yazın:

```bash
pip install azure-storage-blob
```


> [!NOTE]
> Azure depolama SDK Python 0.36 veya önceki bir sürümü için yükseltme yapıyorsanız, önce kullanarak kaldırmak gerekir `pip uninstall azure-storage` artık depolama SDK'sı Python için tek bir paket sunacağımız gibi.
> 
> 

Alternatif yükleme yöntemleri için ziyaret [github'da Python için Azure depolama SDK'sı](https://github.com/Azure/azure-storage-python/).

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma
Kullanmak istediğiniz blob türüne göre oluşturma bir **BlockBlobService**, **AppendBlobService**, veya **PageBlobService** nesnesi. Aşağıdaki kod bir **BlockBlobService** nesnesi. Program aracılığıyla Azure blok Blob depolama alanına erişmek istediğiniz tüm Python dosyanın en üstüne yakın aşağıdakileri ekleyin.

```python
from azure.storage.blob import BlockBlobService
```

Aşağıdaki kod oluşturur bir **BlockBlobService** depolama hesabı adı ve hesap anahtarını kullanarak nesne.  'Myaccount' ve 'mykey' hesap adı ve anahtarı ile değiştirin.

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

Aşağıdaki kod örneğinde kullanabileceğiniz bir **BlockBlobService** yoksa kapsayıcıyı oluşturulacak nesne.

```python
block_blob_service.create_container('mycontainer')
```

(Daha önce yaptığınız gibi) Bu kapsayıcıdan BLOB indirmek için depolama erişim anahtarınızı belirtmeniz gerekir böylece varsayılan olarak, yeni özel kapsayıcıdır. Kapsayıcı içinde BLOB'ları herkes tarafından kullanılabilmesini sağlamak istiyorsanız, kapsayıcı oluşturun ve aşağıdaki kodu kullanarak genel erişim düzeyini geçirin.

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

Alternatif olarak, aşağıdaki kodu kullanarak oluşturduktan sonra bir kapsayıcı değiştirebilirsiniz.

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

Bu değişiklikten sonra Internet üzerinden herkes ortak bir kapsayıcıdaki blobları görebilir ancak yalnızca değiştirebilir veya silebilirsiniz.

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Bir blok blobu oluşturmak ve verileri yüklemek için kullanın **oluşturma\_blob\_gelen\_yolu**, **oluşturmak\_blob\_gelen\_akış**, **oluşturmak\_blob\_gelen\_bayt** veya **oluşturma\_blob\_gelen\_metin** yöntemleri. Bunlar veri boyutu 64 MB aştığında gerekli Öbekleme gerçekleştiren üst düzey yöntemleridir.

**oluşturma\_blob\_gelen\_yolu** belirtilen yolun, bir dosyanın içeriğini yükler ve **oluşturma\_blob\_gelen\_akış** zaten açılmış bir dosya/akışı içeriğini yükler. **oluşturma\_blob\_gelen\_bayt** bayt dizisi yükler ve **oluşturma\_blob\_gelen\_metin** belirtilen (varsayılan UTF-8) kodlaması kullanarak belirli bir metin değeri yükler.

Aşağıdaki örnek içeriğini yükler **sunset.png** içine dosya **myblockblob** blob.

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme
BLOB'ları bir kapsayıcıda listelemek için kullanın **listesi\_BLOB'lar** yöntemi. Bu yöntem bir oluşturucuyu döndürür. Aşağıdaki kod çıktıları **adı** her BLOB bir kapsayıcıda konsoluna.

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a>Blob’ları indirme
Bir blob üzerinden veri indirmek için kullanın **almak\_blob\_için\_yolu**, **almak\_blob\_için\_akış**, **almak\_blob\_için\_bayt**, veya **almak\_blob\_için\_metin**. Bunlar veri boyutu 64 MB aştığında gerekli Öbekleme gerçekleştiren üst düzey yöntemleridir.

Aşağıdaki örnek, kullanma gösterir **almak\_blob\_için\_yolu** içeriğini indirmek için **myblockblob** depolamakveblob**çıkış sunset.png** dosya.

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a>Blob silme
Son olarak, bir blobu silmek için çağrı **delete_blob**.

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-to-an-append-blob"></a>Ek blobu yazma
Ek blob, günlük tutma gibi ekleme işlemleri için en iyi duruma getirilmiştir. Blok blobuna benzer şekilde bir ek blobu bloklardan oluşur ancak bir ek bloba yeni bir blok eklediğinizde her zaman blobun sonuna eklenir. Bir ek blobdaki mevcut bloğu güncelleştiremezsiniz veya silemezsiniz. Bir blok blobu olduğu için ek blobun blok kimliği gösterilmez.

Her biri en fazla 4 MB olmak üzere bir ek blobundaki her blok farklı boyutlarda olabilir ve bir ek blobu en fazla 50.000 blok içerebilir. Bu nedenle bir ek blobunun en büyük boyutu 195 GB’den biraz fazladır (4 MB x 50.000 blok).

Aşağıdaki örnek yeni bir ek blob oluşturur ve basit bir günlük yazma işlemini benzeterek buna veri ekler.

```python
from azure.storage.blob import AppendBlobService
append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

# The same containers can hold all types of blobs
append_blob_service.create_container('mycontainer')

# Append blobs must be created before they are appended to
append_blob_service.create_blob('mycontainer', 'myappendblob')
append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')
```

## <a name="next-steps"></a>Sonraki adımlar
Blob Storage’ın temellerini öğrendiğinize göre, daha fazla bilgi edinmek için bu bağlantıları izleyin.

* [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/)
* [Azure Depolama Hizmetleri REST API'si](http://msdn.microsoft.com/library/azure/dd179355)
* [Azure Depolama Ekibi Blog’u]
* [Python için Microsoft Azure depolama SDK]

[Azure Depolama Ekibi Blog’u]: http://blogs.msdn.com/b/windowsazurestorage/
[Python için Microsoft Azure depolama SDK]: https://github.com/Azure/azure-storage-python
