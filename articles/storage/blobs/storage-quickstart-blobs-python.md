---
title: Azure Hızlı Başlangıç - Python ile nesne depolamada blob oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, nesne (Blob) depolamada depolama hesabı ve kapsayıcı oluşturursunuz. Sonra, Azure Depolama’ya blob yüklemek, blob indirmek ve bir kapsayıcıdaki blobları listelemek amacıyla Python için depolama istemcisi kitaplığını kullanırsınız.
services: storage
author: tamram
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 12/14/2018
ms.author: tamram
ms.openlocfilehash: a1a931573967f12eb7abc791bd951dc6e1e9e60b
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59607407"
---
# <a name="quickstart-upload-download-and-list-blobs-with-python"></a>Hızlı Başlangıç: Karşıya yükleme, indirme ve Python ile blobları Listele

Bu hızlı başlangıçta karşıya yükleyin, indirin ve Azure Blob depolamadaki bir kapsayıcıda blok bloblarını listelemek için Python kullanma bakın. Basitçe herhangi bir metin veya ikili veriler de (örneğin, görüntüleri, belge, medya akışı, arşiv veri, vb.) miktarını tutabilir ve dosya paylaşımları, şemasız tablolar ve ileti kuyrukları Azure depolamada farklı nesneleri blobudur. (Daha fazla bilgi için [Azure Storage'a giriş](/azure/storage/common/storage-introduction.md).)

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

Aşağıdaki ek önkoşulların yüklü olduğundan emin olun:

* [Python](https://www.python.org/downloads/)
* [Python için Azure depolama SDK'si](https://github.com/Azure/azure-sdk-for-python)

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:
Bu hızlı başlangıçtaki [örnek uygulama](https://github.com/Azure-Samples/storage-blobs-python-quickstart.git), temel bir Python uygulamasıdır.  

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek için [Git](https://git-scm.com/)'i kullanın. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-python-quickstart.git 
```

Bu komut *Azure-Samples/storage-blobs-python-quickstart* deposunu yerel git klasörünüze kopyalar. Python programını çalıştırmak için, deponun kökündeki *example.py* dosyasını açın.  

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
Uygulamada, `BlockBlobService` nesnesi oluşturmak için depolama hesabı adınızı ve hesap anahtarınızı sağlayın. IDE'nizdeki Çözüm Gezgini'nde *example.py* dosyasını açın. `accountname` ve `accountkey` değerlerini hesap adınız ve anahtarınız ile değiştirin. 

```python 
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
```

## <a name="run-the-sample"></a>Örneği çalıştırma
Bu örnek, *Belgeler* klasöründe bir sınama dosyası oluşturur. Örnek program sınama dosyasını Blob depolamaya yükler, kapsayıcıdaki blobları listeler ve dosyayı yeni bir adla indirir. 

İlk olarak `pip install` komutunu çalıştırarak bağımlılıkları yükleyin:

    pip install azure-storage-blob

Ardından örneği çalıştırın. Aşağıdaki çıkışa benzer iletiler görürsünüz:
  
```
Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Devam etmeden önce, iki dosya için *Belgeler* klasörünüze bakın. Dosyaları açarak aynı olduklarını görebilirsiniz.

Ayrıca, Blob depolamadaki dosyaları görüntülemek için, [Azure Depolama Gezgini](https://storageexplorer.com) gibi bir araç da kullanabilirsiniz. Azure Depolama Gezgini, depolama hesabı bilgilerinize erişmenize olanak tanıyan ücretsiz ve platformlar arası bir araçtır. 

Dosyaları doğruladıktan sonra, tanıtımı tamamlamak ve test dosyalarını silmek için herhangi bir tuşa basın. Artık örnek dosyanın işlevini gördüğünüze göre, koda göz atmak için *example.py* dosyasını açabilirsiniz. 

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Şimdi, nasıl çalıştığını anlayabilmeniz için örnek kodu inceleyelim.

### <a name="get-references-to-the-storage-objects"></a>Depolama nesneleriyle ilgili başvuruları alma
İlk önce, Blob depolamaya erişmek ve Blob depolamayı yönetmek için kullanılan nesnelere başvuru oluşturursunuz. Bu nesneler birbirleri üzerinde derlenir ve her bir dosya, listede yanında yer alan dosya tarafından kullanılır.

* Depolama hesabınızdaki Blob hizmetine işaret eden bir **BlobService** nesne örneği oluşturun. 

* Eriştiğiniz kapsayıcıyı temsil eden bir **CloudBlobContainer** nesne örneği oluşturun. Kapsayıcılar, tıpkı bilgisayarınızdaki dosyaları düzenlemek için klasörleri kullandığınız gibi blobları düzenlemek için kullanılır.

Bulut Blobu kapsayıcınız olduktan sonra, ilgilendiğiniz bloba işaret eden **CloudBlockBlob** nesnesinin örneğini oluşturun. Ardından blobu gerektiği gibi karşıya yükleyebilir, indirebilir ve kopyalayabilirsiniz.

> [!IMPORTANT]
> Kapsayıcı adlarının küçük harfle yazılması gerekir. Kapsayıcılar ve blob adları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, Blobları ve Meta Verileri adlandırma ve bunlara başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Bu bölümde nesne örneği ve yeni bir kapsayıcı oluşturacak ve ardından kapsayıcıdaki izinleri bloblar herkese açık olacak şekilde ayarlayacaksınız. Bu kapsayıcının adı **quickstartblobs**’dur. 

```python 
# Create the BlockBlockService that is used to call the Blob service for the storage account
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
 
# Create a container called 'quickstartblobs'.
container_name ='quickstartblobs'
block_blob_service.create_container(container_name) 

# Set the permission so the blobs are public.
block_blob_service.set_container_acl(container_name, public_access=PublicAccess.Container)
```
### <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları en sık kullanılan bloblardır ve bu hızlı başlangıçta bu bloblar kullanılmıştır.  

Bir dosyayı bloba yüklemek için, yerel diskinizdeki dizin adıyla dosya adını birleştirerek dosyanın tam yolunu alın. Sonra, dosyayı belirtilen yola `create\_blob\_from\_path` yöntemiyle yükleyebilirsiniz. 

Örnek kod, karşıya yükleme ve indirme için kullanılacak yerel bir dosya oluşturur, karşıya yüklenecek dosyayı `file\_path\_to\_file` olarak ve blob adını `local\_file\_name` olarak depolar. Aşağıdaki örnek, dosyayı **quickstartblobs** adlı kapsayıcınıza yükler.

```python
# Create a file in Documents to test the upload and download.
local_path=os.path.expanduser("~\Documents")
local_file_name ="QuickStart_" + str(uuid.uuid4()) + ".txt"
full_path_to_file =os.path.join(local_path, local_file_name)

# Write text to the file.
file = open(full_path_to_file,  'w')
file.write("Hello, World!")
file.close()

print("Temp file = " + full_path_to_file)
print("\nUploading to Blob storage as blob" + local_file_name)

# Upload the created file, use local_file_name for the blob name
block_blob_service.create_blob_from_path(container_name, local_file_name, full_path_to_file)
```

Blob depolamayla kullanabileceğiniz çeşitli karşıya yükleme yöntemleri vardır. Örneğin, bir bellek akışınız varsa `create\_blob\_from\_path` yerine `create\_blob\_from\_stream` yöntemini kullanabilirsiniz. 

Blok bloblarının boyutu 4,7 TB'yi bulabilir ve bu bloblar Excel elektronik tablolarından büyük video dosyalarına kadar birçok türde olabilir. Sayfa blobları öncelikli olarak IaaS VM'lerini destekleyen VHD dosyalarında kullanılır. Ekleme blobları, bir dosyaya yazıp daha sonradan daha fazla bilgi eklemek istediğiniz durumlarda günlüğe kaydetme için kullanılır. Blob depolamada depolanan nesnelerin çoğu blok blobudur.

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

`list_blobs` yöntemiyle kapsayıcıdaki dosya listesini alın. Bu yöntem bir oluşturucu döndürür. Aşağıdaki kod blob listesini alır, &mdash;ardından bu bloblarda döngü yapar&mdash; ve kapsayıcıda bulunan blobların adlarını gösterir.  

```python
# List the blobs in the container
print("\nList blobs in the container")
generator = block_blob_service.list_blobs(container_name)
for blob in generator:
    print("\t Blob name: " + blob.name)
```

### <a name="download-the-blobs"></a>Blobları indirme

`the get\_blob\_to\_path` yöntemini kullanarak blobları yerel diskinize indirin. Aşağıdaki kod, önceki bir bölüme yüklenen blobu indirir. Yerel diskte yer alan iki dosyayı da görebilmeniz için, blob adının sonuna *_DOWNLOADED* son eki getirilir. 

```python
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = os.path.join(local_path, string.replace(local_file_name ,'.txt', '_DOWNLOADED.txt'))
print("\nDownloading blob to " + full_path_to_file2)
block_blob_service.get_blob_to_path(container_name, local_file_name, full_path_to_file2)
```

### <a name="clean-up-resources"></a>Kaynakları temizleme
Bu hızlı başlangıçta karşıya yüklenen bloblara artık ihtiyacınız kalmadığında, `delete\_container` yöntemini kullanarak kapsayıcının tamamını silebilirsiniz. Bunun yerine tek tek dosyaları silmek için `delete\_blob` yöntemini kullanın.

```python
# Clean up resources. This includes the container and the temp files
block_blob_service.delete_container(container_name)
os.remove(full_path_to_file)
os.remove(full_path_to_file2)
```
## <a name="resources-for-developing-python-applications-with-blobs"></a>Bloblarla Python uygulamaları geliştirme kaynakları

Blob depolama ile Python geliştirme hakkında daha fazla bilgi için, şu ek kaynaklara bakın:

### <a name="binaries-and-source-code"></a>İkili dosyalar ve kaynak kodu

- GitHub’da Azure Depolama için [Python istemci kitaplığı kaynak kodunu](https://github.com/Azure/azure-storage-python) görüntüleyin, indirin ve yükleyin.

### <a name="client-library-reference-and-samples"></a>İstemci kitaplığı başvurusu ve örnekleri

- Python istemci kitaplığı hakkında daha fazla bilgi için bkz. [Python API başvurusu](https://docs.microsoft.com/python/api/overview/azure/storage).
- Python istemci kitaplığı kullanılarak yazılmış [Blob depolama örneklerini](https://azure.microsoft.com/resources/samples/?sort=0&service=storage&platform=python&term=blob) araştırın.

## <a name="next-steps"></a>Sonraki adımlar
 
Bu hızlı başlangıçta, dosyaları Python kullanarak yerel bir disk ve Azure Blob depolama arasında aktarmayı öğrendiniz. Blob depolamayla çalışma hakkında daha fazla bilgi edinmek için, Blob depolama nasıl yapılır öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Blob Depolama İşlemleri Nasıl Yapılır](./storage-python-how-to-use-blob-storage.md)
 
Depolama Gezgini ve Bloblar hakkında daha fazla bilgi için bkz. [Azure Blob depolama kaynaklarını Depolama Gezgini'yle yönetme](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
