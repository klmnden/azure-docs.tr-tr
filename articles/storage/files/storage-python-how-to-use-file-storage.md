---
title: Python ile Azure dosyaları için geliştirme | Microsoft Docs
description: Python uygulamaları ve dosya verilerini depolamak için Azure dosyaları'nı kullanma hizmetlerini geliştirmeyi öğrenin.
services: storage
author: roygara
ms.service: storage
ms.devlang: python
ms.topic: article
ms.date: 12/14/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: ecb3ef82196c3b6febd44850b47f467ba37facc2
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63763534"
---
# <a name="develop-for-azure-files-with-python"></a>Python ile Azure dosyaları için geliştirme
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

Bu öğretici, dosya verilerini depolamak için Azure dosyaları'nı kullanma uygulamaları veya hizmetleri geliştirmek için Python'ı kullanmanın temellerini gösterilecektir. Bu öğreticide, biz basit bir konsol uygulaması oluşturun ve nasıl Python ve Azure dosyaları ile temel Eylemler gerçekleştirileceğini göstereceğiz:

* Azure dosya paylaşımları oluşturma
* Dizinler oluşturma
* Dosyalar ve dizinler bir Azure dosya paylaşımı içindeki numaralandırın
* Yükleme, indirme ve dosya silme

> [!Note]  
> Azure dosyaları SMB üzerinden erişilebildiğinden, standart Python g/ç sınıfları ve işlevleri kullanarak Azure dosya paylaşımına erişen basit uygulamalar yazmak mümkündür. Bu makalede, Azure depolama Python SDK'SININ, kullanan uygulamaların nasıl yazılacağı anlatmaktadır [Azure dosya REST API'sini](https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api) Azure dosyaları'na bahsedeceğiz.

## <a name="download-and-install-azure-storage-sdk-for-python"></a>İndirin ve Python için Azure depolama SDK'sını yükleyin

[Python için Azure depolama SDK'sı](https://github.com/azure/azure-storage-python) Python 2.7, 3.3, 3.4, 3.5 ve 3.6 gerektirir.
 
## <a name="install-via-pypi"></a>Pypı yüklemek

Python paket dizinini (Pypı) yüklemek için şunu yazın:

```bash
pip install azure-storage-file
```

> [!NOTE]
> Python 0.36 veya önceki bir sürümü için Azure depolama SDK'sından yükseltme yapıyorsanız, eski SDK'sını kullanarak kaldırma `pip uninstall azure-storage` en yeni paketi yüklemeden önce.

Diğer yükleme yöntemleri için ziyaret [GitHub üzerinde Python için Azure depolama SDK'sı](https://github.com/Azure/azure-storage-python/).

## <a name="view-the-sample-application"></a>Örnek uygulamayı görüntüleme
f görüntülemek ve Azure dosyaları ile Python kullanma gösteren bir örnek uygulama çalıştırmak için bkz. [Azure Depolama: Python Azure dosyaları ile çalışmaya başlama](https://github.com/Azure-Samples/storage-file-python-getting-started). 

Örnek uygulamayı çalıştırmak için her ikisi de yüklediğinizden emin olun `azure-storage-file` ve `azure-storage-common` paketleri.

## <a name="set-up-your-application-to-use-azure-files"></a>Azure dosyaları'nı kullanmak için uygulamanızı ayarlama
Ekranın üst kısmında programlı bir şekilde Azure Depolama'ya erişmek istediğiniz herhangi bir Python kaynak dosyası ekleyin.

```python
from azure.storage.file import FileService
```

## <a name="set-up-a-connection-to-azure-files"></a>Azure dosyaları için bir bağlantı ayarlayın 
`FileService` Nesne, paylaşımları, dizinleri ve dosyaları ile çalışmanıza olanak tanır. Aşağıdaki kod oluşturur bir `FileService` depolama hesabı adını ve hesap anahtarını kullanarak nesne. `<myaccount>` ve `<mykey>` ifadelerini hesap adınız ve anahtarınız ile değiştirin.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma
Aşağıdaki kod örneği, kullanabileceğiniz bir `FileService` yoksa paylaşımı oluşturmak için nesne.

```python
file_service.create_share('myshare')
```

## <a name="create-a-directory"></a>Dizin oluşturma
Ayrıca, depolama, yerine tümünün kök dizininde alt dizinlerin içindeki dosyaları koyarak de düzenleyebilirsiniz. Azure dosyaları hesabınıza izin verdiği sayıda dizin oluşturmanıza olanak sağlar. Aşağıdaki kodu adlı bir alt dizin oluşturur **sampledir** kök dizininin altındaki.

```python
file_service.create_directory('myshare', 'sampledir')
```

## <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Dosyalar ve dizinler bir Azure dosya paylaşımı içindeki numaralandırın
Dosyaların ve dizinlerin bir paylaşımda listelemek için kullanın **listesi\_dizinleri\_ve\_dosyaları** yöntemi. Bu yöntem bir oluşturucu döndürür. Aşağıdaki kod çıkışları **adı** her dosya ve dizin konsoluna bir paylaşımda.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

## <a name="upload-a-file"></a>Dosyayı karşıya yükleme 
Azure dosya paylaşımını içeren en azından dosyalarının bulunduğu bir kök dizin. Bu bölümde, bir paylaşım kök dizini üzerine yerel depodan bir dosyayı karşıya yüklemeyi öğreneceksiniz.

Bir dosya oluşturun ve verileri yüklemek için kullanın `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` veya `create_file_from_text` yöntemleri. Bunlar veri boyutu 64 MB'ı aştığında gerekli Öbekleme gerçekleştiren üst düzey yöntemlerdir.

`create_file_from_path` Belirtilen yol, bir dosyanın içeriğini yükler ve `create_file_from_stream` zaten açılmış bir dosya/akışı içeriğini yükler. `create_file_from_bytes` bir bayt dizisi yükler ve `create_file_from_text` (varsayılan olarak UTF-8) belirtilen kodlama kullanılarak belirtilen metin değerini yükler.

Aşağıdaki örnek içeriğini yükler **sunset.png** doyasını **myfile** dosya.

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
Bir dosyadan verileri indirmek için kullanın `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, veya `get_file_to_text`. Bunlar veri boyutu 64 MB'ı aştığında gerekli Öbekleme gerçekleştiren üst düzey yöntemlerdir.

Aşağıdaki örneği kullanarak göstermektedir `get_file_to_path` içeriğini indirmek için **myfile** dosya ve depolama için **çıkış sunset.png** dosya.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

## <a name="delete-a-file"></a>Dosyayı silme
Son olarak, bir dosyayı silmek için çağrı `delete_file`.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="create-share-snapshot"></a>Paylaşım anlık görüntüsü oluşturma
Tüm dosya paylaşımınızı zaman kopyasını bir noktası oluşturabilirsiniz.

```python
snapshot = file_service.snapshot_share(share_name)
snapshot_id = snapshot.snapshot
```

**Meta veriler ile paylaşım anlık görüntüsü oluşturma**

```python
metadata = {"foo": "bar"}
snapshot = file_service.snapshot_share(share_name, metadata=metadata)
```

## <a name="list-shares-and-snapshots"></a>Paylaşımları ve anlık görüntüleri 
Belirli bir paylaşım için anlık görüntüleri listeleyebilirsiniz.

```python
shares = list(file_service.list_shares(include_snapshots=True))
```

## <a name="browse-share-snapshot"></a>Paylaşım anlık görüntüsüne göz atın
Dosyalar ve dizinler sürede o noktadan itibaren almak için her bir paylaşım anlık görüntüsü içeriğine göz atabilirsiniz.

```python
directories_and_files = list(file_service.list_directories_and_files(share_name, snapshot=snapshot_id))
```

## <a name="get-file-from-share-snapshot"></a>Dosya paylaşım anlık görüntüsünden alın
Bir dosya paylaşım anlık görüntüsünden geri yükleme senaryonuz için indirebilirsiniz.

```python
with open(FILE_PATH, 'wb') as stream:
    file = file_service.get_file_to_stream(share_name, directory_name, file_name, stream, snapshot=snapshot_id)
```

## <a name="delete-a-single-share-snapshot"></a>Tek bir paylaşım anlık görüntüsünü Sil  
Tek bir paylaşım anlık görüntüsünü silebilirsiniz.

```python
file_service.delete_share(share_name, snapshot=snapshot_id)
```

## <a name="delete-share-when-share-snapshots-exist"></a>Paylaşım anlık görüntüleri varken Paylaşımı Sil
Bir paylaşım anlık görüntüleri içeren tüm anlık görüntüleri ilk silinene kadar silinemez.

```python
file_service.delete_share(share_name, delete_snapshots=DeleteSnapshot.Include)
```

## <a name="next-steps"></a>Sonraki adımlar
Python ile Azure dosyaları işlemek öğrendiniz, daha fazla bilgi için bu bağlantıları izleyin.

* [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/)
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355)
* [Microsoft Azure depolama için Python SDK'sı](https://github.com/Azure/azure-storage-python)
