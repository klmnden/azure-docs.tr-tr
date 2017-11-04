---
title: "Azure hızlı başlangıç - aktarımı nesneleri/Node.js| kullanarak Azure Blob depolama biriminden Microsoft Docs"
description: "Öğesine/öğesinden Node.js kullanarak Azure Blob storage nesneleri aktarmak hızlı bir şekilde öğrenin"
services: storage
documentationcenter: storage
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/30/2017
ms.author: gwallace
ms.openlocfilehash: 4c3c4ec341a0e5f4f0e7415128479f6448f7db6b
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-nodejs"></a>Aktarım nesneleri/Node.js kullanarak Azure Blob depolama biriminden

Bu hızlı başlangıç Node.js karşıya yükleyin, indirin ve blok blobları Azure Blob Depolama birimindeki bir kapsayıcıda listelemek için nasıl kullanılacağını öğrenin.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* [Node.js](https://nodejs.org/en/)’yi yükleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

[Örnek uygulama](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git) Bu hızlı başlangıç kullanılan bir temel konsol uygulamasıdır. 

Kullanım [git](https://git-scm.com/) geliştirme ortamınızı uygulamaya bir kopyasını indirmek için.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

Bu komut, yerel git klasörünüze depoya klonlar. BLOB'lar düğümü quickstart depolama klasörü uygulama ara açmak için dosyayı açın ve index.js üzerinde çift tıklatın.

## <a name="configure-your-storage-connection-string"></a>Depolama bağlantı dizelerinizi yapılandırma

Uygulamada, depolama hesabınız için bağlantı dizesi belirtmeniz gerekir. Açık `index.js` dosya, Bul `connectionString` değişkeni. Azure portalından kaydedilen bir bağlantı dizesi, tüm değeriyle değerini değiştirin. Depolama bağlantı dizenizi aşağıdakine benzer görünmelidir:

```node
// Create a blob client for interacting with the blob service from connection string
// How to create a storage connection string - http://msdn.microsoft.com/library/azure/ee758697.aspx
var connectionString = '<Your connection string here>';
var blobService = storage.createBlobService(connectionString);
```

## <a name="install-required-packages"></a>Gerekli paketleri yüklemek

Çalıştırma uygulama dizinindeki `npm install` herhangi yüklemek için listelenen paketler gerekli `package.json` dosyası.

```node
npm install
```

## <a name="run-the-sample"></a>Örnek çalıştırın

Bu örnek Belgelerim bir test dosyası oluşturur, Blob depolama alanına yükler, kapsayıcıda BLOB'ları listeler ve sonra eski ve yeni dosyalar karşılaştırmak için dosyanın yeni bir adla yükler.

Örnek yazarak çalıştırırsınız `node index.js`. Aşağıdaki çıkış Windows sisteminden ' dir.  Uygun dosya yolları ile benzer bir çıktıya Linux kullanıyorsanız beklenebilir.

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

Devam etmeden önce MyDocuments için iki dosyalarına bakın. Ekleri açmak ve aynı görebilirsiniz.

Bir aracı gibi kullanabilir [Azure Storage Gezgini](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) Blob storage'da dosyaları görüntülemek için. Azure Depolama Gezgini, depolama hesabı bilgilerinizi erişmenize olanak sağlayan ücretsiz bir platformlar arası aracıdır.

Dosyaları doğrulandıktan sonra tanıtım ve test dosyalarını silmeniz herhangi bir tuşa basın. Örnek yaptığı bildiğinize göre kodu aramak için index.js dosyasını açın. 

## <a name="get-references-to-the-storage-objects"></a>Depolama nesneleri başvuruları alma

Yapılacak ilk şey referansı oluşturmaktır `BlobService` erişmek ve Blob Depolama yönetmek için kullanılır. Bu nesneler birbirine yapı--her listedeki bir sonraki tarafından kullanılır.

* Bir örneğini oluşturmak  **[BlobService](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService__ctor)**  depolama hesabınızdaki Blob hizmetine işaret nesnesi.

* Yeni bir kapsayıcı oluşturmak ve böylece BLOB genel ve yalnızca bir URL ile erişilebilir kapsayıcısında izinleri ayarlama. Kapsayıcı ile başlayan **quickstartcontainer -**.

Bu örnekte [createContainerCreateIfNotExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists) her zaman yeni bir kapsayıcı oluşturmak istiyoruz çünkü örneğini çalıştırın. Bir uygulama boyunca aynı kapsayıcı kullandığınız bir üretim ortamında, yalnızca bir kez CreateIfNotExists çağırmak için daha iyi uygulamadır. Alternatif olarak, kodda oluşturabilir gerek kalmaması önceden kapsayıcı oluşturabilirsiniz.

```node
// Create a container for organizing blobs within the storage account.
console.log('1. Creating a Container with Public Access:', blockBlobContainerName, '\n');
blobService.createContainerIfNotExists(blockBlobContainerName, { 'publicAccessLevel': 'blob' }, function (error) {
    if (error) return callback(error);
```

## <a name="upload-blobs-to-the-container"></a>BLOB kapsayıcıya karşıya yükle

Blob depolama blok blobları, ekleme bloblarını ve sayfa bloblarını destekler. Blok blobları en yaygın olarak kullanılır. Bunlar, metin ve ikili verileri bu quickstart kullanıldıkları neden olduğu depolanması için idealdir.

Bir blobu bir dosyayı karşıya yüklemek için kullandığınız [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile) yöntemi. Bu işlem zaten mevcut değil veya zaten mevcut değilse bu raporun üzerine blob oluşturur.

Örnek kod, yükleme ve indirme, olarak karşıya yüklenecek dosyayı depolamak için kullanılacak yerel bir dosya oluşturur **localPath** ve içinde blob adını **localFileToUpload**. Aşağıdaki örnek ile başlayın, kapsayıcıya dosya yüklemeleri **quickstartcontainer -**.

```node
console.log('2. Creating a file in ~/Documents folder to test the upload and download\n');
console.log('   Local File:', LOCAL_FILE_PATH, '\n');
fs.writeFileSync(LOCAL_FILE_PATH, 'Greetings from Microsoft!');

console.log('3. Uploading BlockBlob:', BLOCK_BLOB_NAME, '\n');
blobService.createBlockBlobFromLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, LOCAL_FILE_PATH, function (error) {
handleError(error);
console.log('   Uploaded Blob URL:', blobService.getUrl(CONTAINER_NAME, BLOCK_BLOB_NAME), '\n');
```

Blob storage ile kullanabileceğiniz çeşitli karşıya yükleme yöntemler vardır. Örneğin, bir bellek akış varsa, kullanabileceğiniz [createBlockBlobFromStream](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromStream) yöntemi yerine [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile).

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

Ardından, uygulama bir kapsayıcı kullanılarak, dosyaların bir listesini alır [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). Aşağıdaki kod BLOB'lar listesini alır ve ardından bunları bulunan blobların URI'ler gösteren döngü. Komut penceresinde URI'yi kopyalayın ve dosyayı görüntülemek için bir tarayıcıya yapıştırın.

5.000 veya daha az sayıda BLOB kapsayıcısında varsa, tüm blob adları bir çağrıda alınır [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). 5. 000'den fazla BLOB kapsayıcısında varsa, tüm blob adları alınmadı kadar hizmet 5.000 kümesi listesinde getirir. Bu API ilk kez adlı şekilde ilk 5.000 blob adları ve devamlılık belirteci döndürür. İkinci kez belirteç sağlar ve hizmet blob adları bir sonraki kümesini alır. ve devamlılık belirteci kadar tüm blob adları alınmadı olduğunu gösteren, vb. NULL'dur.

```node
console.log('4. Listing blobs in container\n');
blobService.listBlobsSegmented(CONTAINER_NAME, null, function (error, data) {
    handleError(error);

    for (var i = 0; i < data.entries.length; i++) {
    console.log(util.format('   - %s (type: %s)'), data.entries[i].name, data.entries[i].blobType);
    }
    console.log('\n');
```

## <a name="download-blobs"></a>Blob’ları indirme

Kullanarak yerel disk blobları indirmek [getBlobToLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_getBlobToLocalFile).

Aşağıdaki kod, önceki bölümde, yerel diskteki hem dosyaları görebilmeniz için blob adı "_DOWNLOADED" sonekine ekleme karşıya blob indirir. 

```node
console.log('5. Downloading blob\n');
blobService.getBlobToLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, DOWNLOADED_FILE_PATH, function (error) {
handleError(error);
console.log('   Downloaded File:', DOWNLOADED_FILE_PATH, '\n');
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıcı karşıya BLOB'ları artık ihtiyacınız varsa, tüm kapsayıcı kullanarak silebilirsiniz [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteBlobIfExists) ve [deleteContainerIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteContainerIfExists). Ayrıca artık gerekli değilse oluşturulan dosyaları silin. Bu uygulamada bastığınızda dikkate uygulamadan çıkmak için girin.

```node
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

Bu hızlı başlangıç yerel disk ve Node.js kullanarak Azure Blob storage arasında dosyaları aktarmak nasıl öğrendiniz. Blob storage ile çalışma hakkında daha fazla bilgi için nasıl yapılır Blob depolama alanına devam edin.

> [!div class="nextstepaction"]
> [BLOB Depolama işlemleri nasıl yapılır konuları](storage-nodejs-how-to-use-blob-storage.md)

BLOB'ları ve Depolama Gezgini hakkında daha fazla bilgi için bkz: [Depolama Gezgini ile yönetme Azure Blob storage kaynaklarını](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
