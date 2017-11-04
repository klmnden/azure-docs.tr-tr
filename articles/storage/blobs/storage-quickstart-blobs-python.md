---
title: "Azure hızlı başlangıç - aktarımı nesneleri/Python kullanarak Azure Blob depolama biriminden | Microsoft Docs"
description: "Python kullanarak Azure Blob storage/gruptan nesneleri aktarmak hızlı bir şekilde öğrenin"
services: storage
documentationcenter: storage
author: ruthogunnnaike
manager: cwatson
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/12/2017
ms.author: v-ruogun
ms.openlocfilehash: 76e23d85b392f8120914f6170040c6b3c450aba6
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
#  <a name="transfer-objects-tofrom-azure-blob-storage-using-python"></a>Aktarım nesneleri/Python kullanarak Azure Blob depolama biriminden
Bu hızlı başlangıç Python karşıya yükleyin, indirin ve blok blobları Azure Blob Depolama birimindeki bir kapsayıcıda listelemek için nasıl kullanılacağını öğrenin. 

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için: 
* Yükleme [Python](https://www.python.org/downloads/)
* İndirme ve yükleme [Python için Azure depolama SDK'sı](storage-python-how-to-use-blob-storage.md#download-and-install-azure-storage-sdk-for-python). 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:
[Örnek uygulama](https://github.com/Azure-Samples/storage-blobs-python-quickstart.git) Bu hızlı başlangıç temel bir Python uygulama kullanılır.  

Kullanım [git](https://git-scm.com/) geliştirme ortamınızı uygulamaya bir kopyasını indirmek için. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-python-quickstart.git 
```

Bu komut, yerel git klasörünüze depoya klonlar. Python programı açmak için depolama BLOB'lar python quickstart klasör ve example.py dosya için bakın.  

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma
Uygulamada, oluşturmak için depolama hesabı adı ve hesap anahtarınızı sağlamanız gereken bir `BlockBlobService` nesnesi. Açık `example.py` IDE'yi Çözüm Gezgini'nde dosyasından. Değiştir **accountname** ve **accountkey** değerleri, hesap adı ve anahtarınız ile. 

```python 
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
```

## <a name="run-the-sample"></a>Örnek çalıştırın
Bu örnek 'Belgeleri' klasöründe bir test dosyası oluşturur. Örnek program test dosyası Blob depolama alanına yükler, BLOB kapsayıcı'ları listeler ve dosyanın yeni bir adla yükler. 

Örnek uygulamayı çalıştırın. Aşağıdaki çıkış uygulama çalışırken döndürülen çıktının bir örneği verilmiştir:
  
```
Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Devam etmek için herhangi bir tuşa basın, örnek program depolama kapsayıcısı ve dosyaları siler. Devam etmeden önce iki dosya için 'Belgeleri' klasörünüzü kontrol edin. Ekleri açmak ve aynı görebilirsiniz.

Bir aracı gibi kullanabilir [Azure Storage Gezgini](http://storageexplorer.com) Blob storage'da dosyaları görüntülemek için. Azure Depolama Gezgini, depolama hesabı bilgilerinizi erişmenize olanak sağlayan ücretsiz bir platformlar arası aracıdır. 

Dosyaları doğrulandıktan sonra tanıtım ve test dosyalarını silmeniz herhangi bir tuşa basın. Örnek yaptığı bildiğinize göre kodu aramak için example.py dosyasını açın. 

## <a name="get-references-to-the-storage-objects"></a>Depolama nesneleri başvuruları alma
Yapılacak ilk şey erişmek ve Blob Depolama yönetmek için kullanılan nesnelerin referansları oluşturmaktır. Bu nesneler birbirine oluşturun ve her bir sonraki listesinde tarafından kullanılır.

* Örneği **BlockBlobService** depolama hesabınızdaki Blob hizmetine işaret nesnesi. 

* Örneği **CloudBlobContainer** erişme kapsayıcı temsil eden nesne. Kapsayıcıları dosyalarınızı düzenlemek için bilgisayarınızda klasörleri kullanmak gibi bloblarınızın düzenlemek için kullanılır.

Bulut Blob kapsayıcısı oluşturduktan sonra örneğini oluşturabilirsiniz **CloudBlockBlob** , ilgilendiğiniz ve karşıya yükleme, indirme ve kopyalama gibi işlemler gerçekleştirmek belirli blob'u işaret eden nesne.

> [!IMPORTANT]
> Kapsayıcı adları küçük harf olmalıdır. Bkz: [adlandırma ve başvuran kapsayıcıları, Blobları ve meta veri](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) kapsayıcı ve blob adları hakkında daha fazla bilgi için.

Bu bölümde, nesne örneği, yeni bir kapsayıcı oluşturmak ve BLOB ortak; bu nedenle kapsayıcısında izinleri ayarlama. Kapsayıcı adı verilen **quickstartblobs**. 

```python 
# Create the BlockBlockService that is used to call the Blob service for the storage account
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
 
# Create a container called 'quickstartblobs'.
container_name ='quickstartblobs'
block_blob_service.create_container(container_name) 

# Set the permission so the blobs are public.
block_blob_service.set_container_acl(container_name, public_access=PublicAccess.Container)
```
## <a name="upload-blobs-to-the-container"></a>BLOB kapsayıcıya karşıya yükle

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları en yaygın olarak kullanılır ve bu hızlı başlangıç içinde kullanılan olmasıdır.  

Bir blobu bir dosyayı karşıya yüklemek için yerel diskinizde dizin adı ve dosya adını birleştirerek dosyasının tam yolunu alır. Ardından, belirtilen yolu kullanarak dosyası yükleyebilir **oluşturma\_blob\_gelen\_yolu** yöntemi. 

Örnek kod, yükleme ve indirme, olarak karşıya yüklenecek dosyayı depolamak için kullanılacak yerel bir dosya oluşturur **dosya\_yolu\_için\_dosya** ve blob adını **yerel\_dosya\_adı**. Aşağıdaki örnek dosya adında, kapsayıcıya yüklemeleri **quickstartblobs**.

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

Blob storage ile kullanabileceğiniz çeşitli karşıya yükleme yöntemler vardır. Örneğin, bir bellek akış varsa, kullanabileceğiniz **oluşturmak\_blob\_gelen\_akış** yöntemi yerine **oluşturma\_blob\_gelen\_yolu**. 

Blok blobları 4.7 TB büyüklüğünde olabilir ve herhangi bir şeyin büyük video dosyaları için Excel elektronik tablolar olabilir. Sayfa blobları Iaas sanal makineleri yedeklemek için kullanılan VHD dosyalarını için birincil olarak kullanılır. Ekleme blobları gibi bir dosyaya yazmak ve daha fazla bilgi ekleme tutmak istediğiniz günlük için kullanılır. BLOB storage'da depolanan çoğu blok blobları nesneleridir.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

Kapsayıcı kullanarak dosyaların bir listesini almak **list_blobs** yöntemi. Bu yöntem bir oluşturucuyu döndürür. Aşağıdaki kod BLOB'lar listesini alır ve ardından bunları bir kapsayıcıda bulunan blobların adlarını gösteren döngü.  

```python
# List the blobs in the container
print("\nList blobs in the container")
    generator = block_blob_service.list_blobs(container_name)
    for blob in generator:
        print("\t Blob name: " + blob.name)
```

## <a name="download-the-blobs"></a>BLOB'ları indirme

Kullanarak yerel disk blobları indirmek **almak\_blob\_için\_yolu** yöntemi. Aşağıdaki kod, önceki bölümde karşıya blob indirir. Her iki dosyaları yerel diskteki görebilmek için "_DOWNLOADED" blob adı sonek olarak eklenir. 

```python
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = os.path.join(local_path, string.replace(local_file_name ,'.txt', '_DOWNLOADED.txt'))
print("\nDownloading blob to " + full_path_to_file2)
block_blob_service.get_blob_to_path(container_name, local_file_name, full_path_to_file2)
```

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu hızlı başlangıcı karşıya BLOB'ları artık ihtiyacınız varsa, tüm kapsayıcı kullanarak silebilirsiniz **silmek\_kapsayıcı**. Oluşturulan dosyalar artık gerekirse, kullandığınız **silmek\_blob** dosyaları silmek için yöntem.

```python
# Clean up resources. This includes the container and the temp files
block_blob_service.delete_container(container_name)
os.remove(full_path_to_file)
os.remove(full_path_to_file2)
```

## <a name="next-steps"></a>Sonraki adımlar
 
Bu hızlı başlangıç yerel disk ve Python kullanarak Azure blob storage arasında dosyaları aktarmak nasıl öğrendiniz. Blob storage ile çalışma hakkında daha fazla bilgi için nasıl yapılır Blob depolama alanına devam edin.

> [!div class="nextstepaction"]
> [BLOB Depolama işlemleri nasıl yapılır konuları](./storage-python-how-to-use-blob-storage.md)
 

BLOB'ları ve Depolama Gezgini hakkında daha fazla bilgi için bkz: [Depolama Gezgini ile yönetme Azure Blob storage kaynaklarını](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
