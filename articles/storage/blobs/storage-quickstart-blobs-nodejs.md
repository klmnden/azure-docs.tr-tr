---
title: "Azure Hızlı Başlangıç - Node.js kullanarak nesneleri Azure Blob depolama içine/dışına aktarma | Microsoft Docs"
description: "Hızlı bir şekilde Node.js kullanarak nesneleri Azure Blob depolama içine/dışına aktarmayı öğrenin"
services: storage
author: tamram
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 02/06/2018
ms.author: tamram
ms.openlocfilehash: 07845d0e1917c00dbd6098ef2bfbd9dcbbf2f97b
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-nodejs"></a>Node.js kullanarak nesneleri Azure Blob depolama içine/dışına aktarma

Bu hızlı başlangıçta, Azure Blob depolamadaki bir kapsayıcıda blok bloblarını karşıya yüklemek, indirmek ve listelemek için Node.js’yi nasıl kullanabileceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* [Node.js](https://nodejs.org/en/)’yi yükleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlangıçta kullanılan [örnek uygulama](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git), temel bir konsol uygulamasıdır. 

Uygulamanın bir kopyasını geliştirme ortamınıza indirmek içi [Git](https://git-scm.com/)’i kullanın.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

Bu komut, depoyu yerel Git klasörünüze kopyalar. Uygulamayı açmak için, storage-blobs-node-quickstart klasörünü arayın, açın ve index.js dosyasına iki kez tıklayın.

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma

Uygulamada, depolama hesabınız için bağlantı dizesi sağlamanız gerekir. `index.js` dosyasını açın ve `connectionString` değişkenini bulun. Değişkenin değerini, Azure portalında kaydettiğiniz bağlantı dizesinin tüm değerleriyle değiştirin. Depolama bağlantı dizeniz, aşağıdakilere benzer şekilde görünmelidir:

```javascript
// Create a blob client for interacting with the blob service from connection string
// How to create a storage connection string - http://msdn.microsoft.com/library/azure/ee758697.aspx
var connectionString = '<Your connection string here>';
var blobService = storage.createBlobService(connectionString);
```

## <a name="install-required-packages"></a>Gerekli paketleri yükleme

Uygulama dizininde, `package.json` dosyasında listelenen gerekli paketleri yüklemek için `npm install` komutunu çalıştırın.

```javascript
npm install
```

## <a name="run-the-sample"></a>Örneği çalıştırma

Bu örnek, Belgelerim’de bir sınama dosyası oluşturur, bunu Blob depolamaya yükler, blobları kapsayıcıda listeler ve ardından eski ve yeni dosyaları karşılaştırabilmeniz için dosyayı yeni bir adla indirir.

`node index.js` yazarak örneği çalıştırın. Aşağıdaki çıktı, Windows sisteminden alınmıştır.  Linux kullanılıyorsa uygun dosya yollarına sahip benzer bir çıktı beklenebilir.

```
Azure Storage Node.js Client Library Blobs Quick Start

1. Creating a container with public access: quickstartcontainer-79a3eea0-bec9-11e7-9a36-614cd00ca63d

2. Creating a file in ~/Documents folder to test the upload and download

   Local File: C:\Users\admin\Documents\HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

3. Uploading BlockBlob: quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

   Uploaded Blob URL: https://mystorageaccount.blob.core.windows.net/quickstartcontainer-79a3eea0-bec9-11e7-9a36-614cd00ca63d/quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

4. Listing blobs in container

   - quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt (type: BlockBlob)


5. Downloading blob

   Downloaded File: C:\Users\admin\Documents\HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d_DOWNLOADED.txt

Sample finished running. When you hit <ENTER> key, the temporary files will be deleted and the sample application will exit.
```

Devam etmeden önce, iki dosya için MyDocuments’i kontrol edin. Dosyaları açarak aynı olduklarını görebilirsiniz.

Ayrıca, Blob depolamadaki dosyaları görüntülemek için, [Azure Depolama Gezgini](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) gibi bir araç da kullanabilirsiniz. Azure Depolama Gezgini, depolama hesabı bilgilerinize erişmenize olanak tanıyan ücretsiz ve platformlar arası bir araçtır.

Dosyaları doğruladıktan sonra, tanıtımı tamamlamak ve sınama dosyalarını silmek için herhangi bir tuşa basın. Artık örnek dosyanın işlevini gördüğünüze göre, koda göz atmak için index.js dosyasını açabilirsiniz. 

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Sonraki aşamada, nasıl çalıştığını anlayabilmeniz için örnek kodu inceleyeceğiz.

### <a name="get-references-to-the-storage-objects"></a>Depolama nesneleriyle ilgili başvuruları alma

İlk önce, Blob depolamaya erişmek ve Blob depolamayı yönetmek için kullanılan **BlobService** nesnesine başvuru oluşturmaktır. Bu nesneler birbirleri üzerinde derlenir - her bir dosya, listede yanında yer alan dosya tarafından kullanılır.

* Depolama hesabınızdaki Blob hizmetine işaret eden bir [BlobService](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService__ctor) nesnesi oluşturun.

* Yeni bir kapsayıcı oluşturun ve ardından kapsayıcıdaki izinleri bloblar herkese açık olacak ve bloblara yalnızca bir URL ile erişilebilecek şekilde ayarlayın. Kapsayıcı, **quickstartcontainer-** ile başlar.

Örnek her çalıştırıldığında yeni bir kapsayıcı oluşturmak istediğimizden, bu örnekte [createContainerCreateIfNotExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists) komutu kullanılır. Bir uygulamanın tamamında aynı kapsayıcıyı kullandığınız bir üretim ortamında, CreateIfNotExists’i yalnızca bir kez çağırmanız önerilir. Alternatif olarak, kapsayıcıyı önceden oluşturabilirsiniz. Böylece kapsayıcıyı kodda oluşturmanıza gerek kalmaz.

```javascript
// Create a container for organizing blobs within the storage account.
console.log('1. Creating a Container with Public Access:', blockBlobContainerName, '\n');
blobService.createContainerIfNotExists(blockBlobContainerName, { 'publicAccessLevel': 'blob' }, function (error) {
    if (error) return callback(error);
```

### <a name="upload-blobs-to-the-container"></a>Blobları kapsayıcıya yükleme

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları, en sık kullanılan bloblardır. Bu bloblar metinleri ve ikili verileri depolamak için idealdir, bu nedenle hızlı başlangıçta kullanılırlar.

Bir bloba dosya yüklemek için [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile) metodunu kullanabilirsiniz. Bu işlemle, daha önce oluşturulmadıysa bir blob oluşturulur, blob zaten varsa blobun üzerine yazılır.

Örnek kod, karşıya yükleme ve indirme için kullanılacak yerel bir dosya oluşturur, karşıya yüklenecek dosyayı **localPath** olarak ve blob adını **localFileToUpload** olarak depolar. Aşağıdaki örnek, dosyayı **quickstartcontainer-** ile başlayan kapsayıcınıza yükler.

```javascript
console.log('2. Creating a file in ~/Documents folder to test the upload and download\n');
console.log('   Local File:', LOCAL_FILE_PATH, '\n');
fs.writeFileSync(LOCAL_FILE_PATH, 'Greetings from Microsoft!');

console.log('3. Uploading BlockBlob:', BLOCK_BLOB_NAME, '\n');
blobService.createBlockBlobFromLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, LOCAL_FILE_PATH, function (error) {
handleError(error);
console.log('   Uploaded Blob URL:', blobService.getUrl(CONTAINER_NAME, BLOCK_BLOB_NAME), '\n');
```

Blob depolamayla kullanabileceğiniz çeşitli karşıya yükleme yöntemleri vardır. Örneğin, bellek akışınız varsa [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile) yerine [createBlockBlobFromStream](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromStream) metodunu kullanabilirsiniz.

### <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

Sonraki aşamada, uygulama [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented) kullanılarak dosya listesi alır. Aşağıdaki kod blob listesini alır, ardından bu bloblarda döngü yapar ve bulunan blobların URI değerlerini gösterir. Dosyayı görüntülemek için URI değerini komut penceresinden kopyalayıp tarayıcıya yapıştırabilirsiniz.

Kapsayıcıda 5.000 veya daha az blobunuz varsa tüm blob adları tek bir [ListBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented) çağrısında alınır. Kapsayıcıda 5.000'den fazla blobunuz varsa, hizmet tüm blob adları alınana kadar listeyi 5.000 blobluk kümeler halinde alır. Dolayısıyla, bu API ilk kez çağrıldığında, ilk 5.000 blob adını ve bir de devamlılık belirteci döndürür. İkinci seferde, siz belirteci sağlarsınız ve hizmet bir sonraki blob adları kümesini alır; tüm blob adlarının alındığını gösterecek şekilde devamlılık belirteci null değerine sahip olana kadar bu şekilde devam eder.

```javascript
console.log('4. Listing blobs in container\n');
blobService.listBlobsSegmented(CONTAINER_NAME, null, function (error, data) {
    handleError(error);

    for (var i = 0; i < data.entries.length; i++) {
    console.log(util.format('   - %s (type: %s)'), data.entries[i].name, data.entries[i].blobType);
    }
    console.log('\n');
```

### <a name="download-blobs"></a>Blob’ları indirme

[getBlobToLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_getBlobToLocalFile) komutunu kullanarak blobları yerel diskinize indirin.

Aşağıdaki kod önceki bir bölümde karşıya yüklenmiş olan blobu indirir; yerel diskinizde her iki dosyayı da görebilmeniz için indirilen blobun adına "_DOWNLOADED" son ekini koyar. 

```javascript
console.log('5. Downloading blob\n');
blobService.getBlobToLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, DOWNLOADED_FILE_PATH, function (error) {
handleError(error);
console.log('   Downloaded File:', DOWNLOADED_FILE_PATH, '\n');
```

### <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta karşıya yüklenen bloblara artık ihtiyacınız kalmadığında, [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteBlobIfExists) ve [deleteContainerIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteContainerIfExists) kullanarak kapsayıcının tamamını silebilirsiniz. Artık gerekmiyorsa oluşturulmuş olan dosyaları da silin. Uygulamadan çıkmak için Enter tuşuna bastığınızda, bu işlem uygulamada gerçekleştirilir.

```javascript
console.log('6. Deleting block Blob\n');
    blobService.deleteBlobIfExists(CONTAINER_NAME, BLOCK_BLOB_NAME, function (error) {
        handleError(error);

    console.log('7. Deleting container\n');
    blobService.deleteContainerIfExists(CONTAINER_NAME, function (error) {
        handleError(error);
            
        fs.unlinkSync(LOCAL_FILE_PATH);
        fs.unlinkSync(DOWNLOADED_FILE_PATH);
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dosyaları Node.js kullanarak yerel bir disk ve Azure Blob depolama arasında aktarmayı öğrendiniz. Blob depolamayla çalışma hakkında daha fazla bilgi edinmek için, Blob depolama nasıl yapılır öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Blob Depolama İşlemleri Nasıl Yapılır](storage-nodejs-how-to-use-blob-storage.md)

Azure Depolama’ya yönelik Node.js başvurusu için bkz. [azure-storage package](https://docs.microsoft.com/javascript/api/azure-storage/?view=azure-node-latest).