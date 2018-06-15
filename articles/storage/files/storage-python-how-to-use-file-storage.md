---
title: Python ile Azure dosyaları için geliştirme | Microsoft Docs
description: Python uygulama ve dosya verilerini depolamak için Azure dosyaları kullanan hizmetleri geliştirmeyi öğrenin.
services: storage
documentationcenter: python
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/19/2017
ms.author: tamram
ms.openlocfilehash: 1102fd516b5497b4c482986b64fa7c96e9ccc54a
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738270"
---
# <a name="develop-for-azure-files-with-python"></a>Python ile Azure dosyaları için geliştirme
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

Bu öğretici, temel uygulamaları veya dosya verilerini depolamak için Azure dosyaları kullanan hizmetler geliştirmek için Python kullanımını gösterilmektedir. Bu öğreticide, basit bir konsol uygulaması oluşturur ve Python ve Azure dosyaları ile temel eylemleri gerçekleştirme göster:

* Azure dosya paylaşımları oluşturma
* Dizinler oluşturma
* Dosya ve dizinlerin bir Azure dosya paylaşımında listeleme
* Karşıya yükleyin, indirin ve dosya silme

> [!Note]  
> Azure dosyaları SMB üzerinden erişilebileceği için standart Python g/ç sınıfları ve işlevleri kullanarak Azure dosya paylaşımına erişim basit uygulamaları yazmak mümkündür. Bu makalede Azure depolama Python kullanan SDK'ın, uygulamaların nasıl yazılacağını anlatmaktadır [Azure dosyaları REST API](https://docs.microsoft.com/rest/api/storageservices/fileservices/file-service-rest-api) Azure dosyaları anlaşmak için.

## <a name="download-and-install-azure-storage-sdk-for-python"></a>İndirme ve Python için Azure depolama SDK yükleme

Python için Azure depolama SDK Python 2.7, 3.3, 3.4, 3.5 veya 3.6 gerektirir ve 4 farklı paketlerde gelir: `azure-storage-blob`, `azure-storage-file`, `azure-storage-table` ve `azure-storage-queue`. Bu öğreticide biz kullanacaksanız `azure-storage-file` paket.
 
## <a name="install-via-pypi"></a>Pypı yüklemek

Python paket dizinini (Pypı) yüklemek için şunu yazın:

```bash
pip install azure-storage-file
```


> [!NOTE]
> Azure depolama SDK Python 0.36 veya önceki bir sürümü için yükseltme yapıyorsanız, önce kullanarak kaldırmak gerekir `pip uninstall azure-storage` artık depolama SDK'sı Python için tek bir paket sunacağımız gibi.
> 
> 

Alternatif yükleme yöntemleri için ziyaret [github'da Python için Azure depolama SDK'sı](https://github.com/Azure/azure-storage-python/).

## <a name="set-up-your-application-to-use-azure-files"></a>Azure dosyaları kullanmak için uygulamanızı ayarlama
Azure Storage programlı olarak erişmek istediğiniz tüm Python kaynak dosyanın en üstüne yakın aşağıdakileri ekleyin.

```python
from azure.storage.file import FileService
```

## <a name="set-up-a-connection-to-azure-files"></a>Azure dosyaları'na bir bağlantı ayarlayın 
`FileService` Nesne paylaşımları, dizinleri ve dosyaları ile çalışmanıza olanak sağlar. Aşağıdaki kod oluşturur bir `FileService` depolama hesabı adı ve hesap anahtarını kullanarak nesne. Değiştir `<myaccount>` ve `<mykey>` , hesap adı ve anahtarınız ile.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Aşağıdaki kod örneğinde kullanabileceğiniz bir `FileService` yoksa paylaşımı oluşturmak için nesne.

```python
file_service.create_share('myshare')
```

## <a name="create-a-directory"></a>Dizin oluşturma
Depolama kök dizininde yerine bunların tümünün alt dizinleri içindeki dosyaları yerleştirerek de düzenleyebilirsiniz. Azure dosyaları hesabınızı izin verdiği sayıda dizinleri oluşturmanıza olanak sağlar. Aşağıdaki kodu adlı bir alt dizinin oluşturacak **sampledir** kök dizininin altında.

```python
file_service.create_directory('myshare', 'sampledir')
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Dosya ve dizinlerin bir Azure dosya paylaşımında listeleme
Dosyaların ve dizinlerin bir paylaşımda listelemek için kullanın **listesi\_dizinleri\_ve\_dosyaları** yöntemi. Bu yöntem bir oluşturucu döndürür. Aşağıdaki kod çıktıları **adı** her bir dosya ve dizin konsoluna bir paylaşımda.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme 
Azure dosya paylaşımı içeren en azından dosyalarının bulunduğu bir kök dizin. Bu bölümde, bir paylaşım kök dizini üzerine yerel depolama biriminden bir dosya karşıya nasıl yükleneceğini öğreneceksiniz.

Bir dosya oluşturun ve verileri yüklemek için kullanmak `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` veya `create_file_from_text` yöntemleri. Bunlar veri boyutu 64 MB aştığında gerekli Öbekleme gerçekleştiren üst düzey yöntemleridir.

`create_file_from_path` Belirtilen yol bir dosyanın içeriğini yükler ve `create_file_from_stream` zaten açılmış bir dosya/akışı içeriğini yükler. `create_file_from_bytes` bayt dizisi yükler ve `create_file_from_text` belirtilen (varsayılan UTF-8) kodlaması kullanarak belirli bir metin değeri yükler.

Aşağıdaki örnek içeriğini yükler **sunset.png** içine dosya **myfile** dosya.

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want to create this blob in the root directory, so we specify None for the directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

## <a name="download-a-file"></a>Dosya indirme
Bir dosyadan veri indirmek için kullanacağınız `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, veya `get_file_to_text`. Bunlar veri boyutu 64 MB aştığında gerekli Öbekleme gerçekleştiren üst düzey yöntemleridir.

Aşağıdaki örnek, kullanma gösterir `get_file_to_path` içeriğini indirmek için **myfile** dosya ve onun için depolamak **çıkış sunset.png** dosya.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

## <a name="delete-a-file"></a>Dosyayı silme
Son olarak, bir dosyayı silmek için çağrı `delete_file`.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="create-share-snapshot-preview"></a>Paylaşım anlık görüntü (Önizleme)
Tüm dosya paylaşımınıza zaman kopyasını bir noktası oluşturabilirsiniz.

```python
snapshot = file_service.snapshot_share(share_name)
snapshot_id = snapshot.snapshot
```

**Meta verileri ile paylaşımı anlık görüntü oluşturma**

```python
metadata = {"foo": "bar"}
snapshot = file_service.snapshot_share(share_name, metadata=metadata)
```

## <a name="list-shares-and-snapshots"></a>Paylaşımları ve anlık görüntüleri 
Belirli bir paylaşımına ilişkin tüm anlık görüntülerin listeleyebilirsiniz.

```python
shares = list(file_service.list_shares(include_snapshots=True))
```

## <a name="browse-share-snapshot"></a>Paylaşım anlık görüntü Gözat
Dosyalar ve dizinler o noktadan itibaren zamanında almak için her paylaşımı anlık görüntü içeriğini göz atabilirsiniz.

```python
directories_and_files = list(file_service.list_directories_and_files(share_name, snapshot=snapshot_id))
```

## <a name="get-file-from-share-snapshot"></a>Dosya Paylaşımı anlık görüntüden Al
Bir dosya paylaşımı anlık görüntüden geri yükleme senaryonuz için yükleyebilirsiniz.

```python
with open(FILE_PATH, 'wb') as stream:
    file = file_service.get_file_to_stream(share_name, directory_name, file_name, stream, snapshot=snapshot_id)
```

## <a name="delete-a-single-share-snapshot"></a>Tek paylaşım anlık görüntüyü Sil  
Tek paylaşım anlık görüntü silebilirsiniz.

```python
file_service.delete_share(share_name, snapshot=snapshot_id)
```

## <a name="delete-share-when-share-snapshots-exist"></a>Paylaşım anlık görüntüler varsa paylaşımı silin
Tüm anlık görüntüleri ilk silinene kadar anlık görüntüleri içeren bir paylaşımına silinemiyor.

```python
file_service.delete_share(share_name, delete_snapshots=DeleteSnapshot.Include)
```

## <a name="next-steps"></a>Sonraki adımlar
Python ile Azure dosyaları yönetmek nasıl öğrendiğinize göre daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* [Python Geliştirici Merkezi](/develop/python/)
* [Azure Depolama Hizmetleri REST API'si](http://msdn.microsoft.com/library/azure/dd179355)
* [Microsoft Azure depolama için Python SDK'sı](https://github.com/Azure/azure-storage-python)