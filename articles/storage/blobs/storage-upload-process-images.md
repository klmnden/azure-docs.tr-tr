---
title: Azure Depolama ile buluta görüntü verileri yükleme | Microsoft Docs
description: Uygulama verilerini depolamak üzere bir web uygulaması ile Azure Blob depolamayı kullanma
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: 2692d174a0f959384a9dd3d587107e5f726e28cb
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53606121"
---
# <a name="tutorial-upload-image-data-in-the-cloud-with-azure-storage"></a>Öğretici: Azure Depolama ile buluta görüntü verileri yükleme

Bu öğretici, bir dizinin birinci bölümüdür. Bu öğreticide, bir depolama hesabına görüntü yüklemek için Azure depolama istemci kitaplığı kullanan web uygulamasının nasıl dağıtılacağını öğreneceksiniz. İşlemi tamamladığınızda, depolar ve Azure depolamadaki görüntüleri görüntüleyen bir web uygulaması oluşturmuş olacaksınız.

# <a name="nettabnet"></a>[\.NET](#tab/net)
![Görüntüler kapsayıcı görünümü](media/storage-upload-process-images/figure2.png)

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
![Görüntüler kapsayıcı görünümü](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Kapsayıcı oluşturma ve izinleri ayarlama
> * Erişim anahtarı alma
> * Bir web uygulamasını Azure'a dağıtma
> * Uygulama ayarlarını yapılandırma
> * Web uygulaması ile etkileşim kurma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliğinizin olması gerekir. Oluşturma bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) başlamadan önce.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı için Bu öğretici, Azure CLI 2.0.4 sürümü çalıştırmanızı gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma 

[az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.  

Aşağıdaki örnek `myResourceGroup` adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location southeastasia 
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Örnek, görüntüleri bir Azure Depolama Hesabındaki blob kapsayıcısına yükler. Depolama hesabı, Azure Storage veri nesnelerinizi depolamak ve bunlara erişmek için benzersiz ad alanı sağlar. Oluşturduğunuz kaynak grubunda [az storage account create](/cli/azure/storage/account#az_storage_account_create) komutunu kullanarak bir depolama hesabı oluşturun.

> [!IMPORTANT]
> Öğreticinin 2 Blob Depolama ile Azure Event grid'i kullanın. Event Grid destekleyen bir Azure bölgesinde depolama hesabınızı oluşturduğunuzdan emin olun. Desteklenen bölgelerin bir listesi için bkz. [bölgelere göre Azure ürünleri](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all).

Aşağıdaki komutta, gördüğünüz Blob Depolama hesabına ilişkin kendi genel benzersiz adınızı değiştirin `<blob_storage_account>` yer tutucu.  

```azurecli-interactive
az storage account create --name <blob_storage_account> \
--location southeastasia --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
```

## <a name="create-blob-storage-containers"></a>BLOB Depolama kapsayıcıları oluşturma

Uygulama, Blob depolama hesabında iki kapsayıcı kullanır. Kapsayıcılar klasörlere benzer ve blobları depolamak. *Images* kapsayıcısı, uygulamanın tam çözünürlüklü görüntüleri yüklediği yerdir. Serinin sonraki bölümlerinde bir Azure işlev uygulaması, yeniden boyutlandırılan küçük resimleri *thumbnails* kapsayıcısına yükler.

[az storage account keys list](/cli/azure/storage/account/keys#az_storage_account_keys_list) komutunu kullanarak depolama hesabı anahtarını alın. Ardından, iki kapsayıcı içeren oluşturmak için bu anahtarı kullanırsınız [az depolama kapsayıcısı oluşturma](/cli/azure/storage/container#az_storage_container_create) komutu.  

Bu örnekte `<blob_storage_account>`, oluşturduğunuz Blob depolama hesabının adıdır. *Görüntüleri* kapsayıcının genel erişim ayarı `off`. *Küçük resimleri* kapsayıcının genel erişim ayarı `container`. `container` Genel erişim ayarı küçük resimleri görüntülemek için web sayfasını ziyaret eden kullanıcılara izin verir.

```azurecli-interactive
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv)

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off

az storage container create -n thumbnails --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your Blob storage account key..."
echo $blobStorageAccountKey
```

Blob Depolama hesabı adı ve anahtarı not edin. Örnek uygulama, görüntüleri karşıya yüklemek için depolama hesabına bağlanmak için bu ayarları kullanır. 

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma

[App Service planı](../../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), uygulamanızı barındıran web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirtir.

[az appservice plan create](/cli/azure/appservice/plan#az_appservice_plan_create) komutu ile bir App Service planı oluşturun.

Aşağıdaki örnekte, **Ücretsiz** fiyatlandırma katmanı kullanılarak `myAppServicePlan` adlı bir App Service planı oluşturulmaktadır:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku Free
```

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Web uygulaması, GitHub örnek deposundan dağıtılan örnek uygulama kodu için bir barındırma alanı sağlar. [az webapp create](/cli/azure/webapp#az_webapp_create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../../app-service/overview.md) oluşturun.  

Aşağıdaki komutta `<web_app>` benzersiz bir ada sahip. Geçerli karakterler: `a-z`, `0-9`, ve `-`. Varsa `<web_app>` olan benzersiz değilse, hata iletisiyle karşılaşırsınız: _Verilen ada sahip Web sitesi `<web_app>` zaten mevcut._ Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir.  

```azurecli-interactive
az webapp create --name <web_app> --resource-group myResourceGroup --plan myAppServicePlan
```

## <a name="deploy-the-sample-app-from-the-github-repository"></a>GitHub deposundan örnek uygulamayı dağıtma

# <a name="nettabnet"></a>[\.NET](#tab/net)

App Service bir web uygulamasına içerik dağıtmanın birkaç yolunu destekler. Bu öğreticide, web uygulamasını bir [genel GitHub örnek deposundan](https://github.com/Azure-Samples/storage-blob-upload-from-webapp) dağıtırsınız. [az webapp deployment source config](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) komutuyla Git dağıtımını web uygulamasında gerçekleşecek şekilde yapılandırın. `<web_app>` değerini önceki adımda oluşturduğunuz web uygulamasının adıyla değiştirin.

Örnek Proje içeren bir [ASP.NET MVC](https://www.asp.net/mvc) uygulama. Uygulama görüntüyü kabul eden, bir depolama hesabına kaydeden ve küçük resim kapsayıcısından görüntüleri görüntüler. Web uygulaması kullandığı [Microsoft.WindowsAzure.Storage](/dotnet/api/microsoft.windowsazure.storage?view=azure-dotnet), [Microsoft.WindowsAzure.Storage.Blob](/dotnet/api/microsoft.windowsazure.storage.blob?view=azure-dotnet)ve [Microsoft.WindowsAzure.Storage.Auth](/dotnet/api/microsoft.windowsazure.storage.auth?view=azure-dotnet) ad alanlarını Azure depolama istemci kitaplığı, Azure depolama ile etkileşimde bulunmak için.

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
App Service bir web uygulamasına içerik dağıtmanın birkaç yolunu destekler. Bu öğreticide, web uygulamasını bir [genel GitHub örnek deposundan](https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node) dağıtırsınız. [az webapp deployment source config](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) komutuyla Git dağıtımını web uygulamasında gerçekleşecek şekilde yapılandırın. `<web_app>` değerini önceki adımda oluşturduğunuz web uygulamasının adıyla değiştirin.

---

```azurecli-interactive
az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
```

## <a name="configure-web-app-settings"></a>Web uygulaması ayarlarını yapılandırma

Örnek web uygulaması, görüntüleri karşıya yüklemek için kullanılan erişim belirteçlerini istemek için [Azure Depolama İstemci Kitaplığı](/dotnet/api/overview/azure/storage?view=azure-dotnet)’nı kullanır. Depolama SDK'sı tarafından kullanılan depolama hesabı kimlik bilgileri, web uygulaması için uygulama ayarları olarak ayarlanır. Uygulama ayarlarını dağıtılmış uygulamaya ekleyin [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) komutu.

Aşağıdaki komutta `<blob_storage_account>` Blob Depolama hesabınızın adıyla ve `<blob_storage_key>` ile ilişkili anahtar. `<web_app>` değerini önceki adımda oluşturduğunuz web uygulamasının adıyla değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <web_app> --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=<blob_storage_account> \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbnails \
AzureStorageConfig__AccountKey=<blob_storage_key>  
```

Dağıtma ve web uygulaması'ı yapılandırdıktan sonra uygulamada görüntüyü karşıya yükleme işlevini test edebilirsiniz.

## <a name="upload-an-image"></a>Bir görüntüyü karşıya yükleme

Web uygulamasını test etmek için, yayımlanan uygulamanızın URL'sine gidin. Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir.
Seçin **fotoğrafları karşıya yükle** bölgeyi seçin ve bir dosyayı karşıya yükleyin veya bir dosyayı bölgenin sürükleyin. Görüntü başarıyla karşıya yüklenirse kaybolur.

# <a name="nettabnet"></a>[\.NET](#tab/net)

![ImageResizer uygulaması](media/storage-upload-process-images/figure1.png)

Örnek kodda `UploadFiletoStorage` görevi *Storagehelper.cs* görüntüleri karşıya yüklemek için kullanılan dosya *görüntüleri* kullanarak depolama hesabı içinde kapsayıcı [ UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) yöntemi. Aşağıdaki kod örneği `UploadFiletoStorage` görevini içerir.

```csharp
public static async Task<bool> UploadFileToStorage(Stream fileStream, string fileName, AzureStorageConfig _storageConfig)
{
    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the blob container by passing the name by reading the value from the configuration (appsettings.json)
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ImageContainer);

    // Get the reference to the block blob from the container
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

    // Upload the file
    await blockBlob.UploadFromStreamAsync(fileStream);

    return await Task.FromResult(true);
}
```

Aşağıdaki sınıflar ve yöntemler, yukarıdaki görevde kullanılır:

|Sınıf  |Yöntem  |
|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)     |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)    |  [CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)       |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)    | [GetBlockBlobReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.getblockblobreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_GetBlockBlobReference_System_String_)        |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)     | [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet)        |

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

![Görüntü Karşıya Yükleme Uygulaması](media/storage-upload-process-images/upload-app-nodejs.png)

Örnek kodda `post` yolu, görüntüyü blob kapsayıcısına yüklemeden sorumludur. Yol, modülleri karşıya yükleme işlemine yardımcı olması için kullanır:

- [multer](https://github.com/expressjs/multer) rota işleyiciye için karşıya yükleme stratejisi uygular.
- [Akış içine](https://github.com/sindresorhus/into-stream) arabellek [Createblockblobfromlocalfile] gerektirdiği gibi bir akışa dönüştürür. ()http://azure.github.io/azure-sdk-for-node/azure-storage-legacy/latest/BlobService.html#createBlockBlobFromStream)

Dosya yolu gönderilir gibi dosyanın içeriğini dosyayı blob kapsayıcısına yüklenir kadar bellekte kalır.

> [!IMPORTANT]
> Büyük dosyaları, belleğe, web uygulamanızın performansı üzerinde olumsuz bir etkiye sahip olabilir. Büyük dosyaları gönderileceği kullanıcıların bekliyorsanız, web sunucusunun dosya sistemindeki dosyaları hazırlama ve ardından Blob depolamaya karşıya zamanlama dikkate alınması gereken isteyebilirsiniz. Dosyaları Blob Depolama alanında olduğunda, sunucu dosya sisteminden kaldırabilirsiniz.

```javascript
const
      express = require('express')
    , router = express.Router()

    , multer = require('multer')
    , inMemoryStorage = multer.memoryStorage()
    , uploadStrategy = multer({ storage: inMemoryStorage }).single('image')

    , azureStorage = require('azure-storage')
    , blobService = azureStorage.createBlobService()

    , getStream = require('into-stream')
    , containerName = 'images'
;

const handleError = (err, res) => {
    res.status(500);
    res.render('error', { error: err });
};

const getBlobName = originalName => {
    const identifier = Math.random().toString().replace(/0\./, ''); // remove "0." from start of string
    return `${originalName}-${identifier}`;
};

router.post('/', uploadStrategy, (req, res) => {

    const
          blobName = getBlobName(req.file.originalname)
        , stream = getStream(req.file.buffer)
        , streamLength = req.file.buffer.length
    ;

    blobService.createBlockBlobFromStream(containerName, blobName, stream, streamLength, err => {

        if(err) {
            handleError(err);
            return;
        }

        res.render('success', { 
            message: 'File uploaded to Azure Blob storage.' 
        });
    });
});
```
---

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Depolama hesabında görüntünün gösterildiğini doğrulayın

[Azure Portal](https://portal.azure.com) oturum açın. Soldaki menüden **Depolama hesapları**’nı ve sonra depolama hesabınızın adını seçin. Altında **Blob hizmeti**seçin **Blobları**, ardından **görüntüleri** kapsayıcı.

Kapsayıcıda görüntünün gösterildiğini doğrulayın.

![Görüntüler kapsayıcı görünümü](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Küçük resim görüntülemeyi test etme

Küçük resim görüntülemeyi test etmek için bir görüntü için karşıya yükleyelim **küçük resimleri** uygulamanın okuyabileceği olup olmadığını denetlemek için kapsayıcı **küçük resimleri** kapsayıcı.

[Azure Portal](https://portal.azure.com) oturum açın. Soldaki menüden **Depolama hesapları**’nı ve sonra depolama hesabınızın adını seçin. Altında **Blob hizmeti**seçin **Blobları**, ardından **küçük resimleri** kapsayıcı. **Karşıya Yükle**’yi seçerek **Blobu karşıya yükle** bölmesini açın.

Dosya Seçici ile bir dosya seçip seçin **karşıya**.

**thumbnails** kapsayıcısına yüklenen görüntünün görünür olduğunu doğrulamak için uygulamanıza geri gidin.

# <a name="nettabnet"></a>[\.NET](#tab/net)
![Görüntüler kapsayıcı görünümü](media/storage-upload-process-images/figure2.png)

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
![Görüntüler kapsayıcı görünümü](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

Serinin iki küçük resim oluşturma, bu yansıma böylece otomatikleştirin. Azure portalındaki **thumbnails** kapsayıcısında, karşıya yüklediğiniz görüntüyü seçin ve **Sil**’i seçerek görüntüyü silin. 

Azure depolama hesabınızdan içeriği önbelleğe almak için CDN etkinleştirebilirsiniz. Azure depolama hesabınızla CDN'yi etkinleştirme hakkında daha fazla bilgi için bkz. [bir Azure depolama hesabını Azure CDN ile tümleştirme](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde bir dizi depolama ile etkileşimde bulunmak için bir web uygulamasını yapılandırma hakkında bilgi edindiniz.

Serinin iki Event Grid kullanarak bir görüntüyü yeniden boyutlandırmaya yönelik Azure işlevini tetikleme hakkında bilgi edinmek için oturum gidin.

> [!div class="nextstepaction"]
> [Karşıya yüklenen bir görüntüyü yeniden boyutlandırmak üzere bir Azure İşlevi’ni tetiklemek için Event Grid kullanma](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
