---
title: "Azure Depolama ile buluta görüntü verileri yükleme | Microsoft Docs"
description: "Uygulama verilerini depolamak için bir web uygulaması ile Azure blob depolama kullanma"
services: storage
documentationcenter: 
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: web
ms.devlang: csharp
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: e3c40d0f3db1a33a405a341a714a7ce199908ca4
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="upload-image-data-in-the-cloud-with-azure-storage"></a>Azure Depolama ile buluta görüntü verileri yükleme

Bu öğretici, bir dizinin birinci bölümüdür. Bu öğreticide bir depolama hesabına görüntü yüklemek için Azure Depolama İstemci Kitaplığı kullanan bir web uygulamasını dağıtma işlemi gösterilmektedir. İşiniz bittiğinde, Azure depolamadaki görüntüleri depolayan ve gösteren bir web uygulamanız olur.

![Görüntüler kapsayıcı görünümü](media/storage-upload-process-images/figure2.png)

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Kapsayıcı oluşturma ve izinleri ayarlama
> * Erişim anahtarı alma
> * Uygulama ayarlarını yapılandırma
> * Bir Web Uygulamasını Azure’a Dağıtma
> * Web uygulaması ile etkileşimde bulunma

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma 

[az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.
 
Aşağıdaki örnek `myResourceGroup` adlı bir kaynak grubu oluşturur.
 
```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus 
``` 

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
 
Örnek, görüntüleri bir Azure Depolama Hesabındaki blob kapsayıcısına yükler. Depolama hesabı, Azure Storage veri nesnelerinizi depolamak ve bunlara erişmek için benzersiz ad alanı sağlar. Oluşturduğunuz kaynak grubunda [az storage account create](/cli/azure/storage/account#az_storage_account_create) komutunu kullanarak bir depolama hesabı oluşturun. 

> [!IMPORTANT] 
> Öğreticinin 2. bölümünde blob depolama için Olay aboneliklerini kullanırsınız. Olay abonelikleri şu anda yalnızca Batı Orta ABD ve Batı ABD 2 bölgelerindeki Blob depolama hesapları için desteklenmektedir. Bu kısıtlama nedeniyle, görüntüleri ve küçük resimleri depolamak için örnek uygulama tarafından kullanılan bir Blob depolama hesabı oluşturmanız gerekir.   

Aşağıdaki komutta, Blob depolama hesabına ilişkin kendi genel benzersiz adınızı `<blob_storage_account>` yer tutucusunu gördüğünüz yere yerleştirin.  

```azurecli-interactive 
az storage account create --name <blob_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
``` 
 
## <a name="create-blob-storage-containers"></a>Blob depolama kapsayıcıları oluşturma
 
Uygulama, Blob depolama hesabında iki kapsayıcı kullanır. Kapsayıcılar klasörlere benzer ve blobları depolamak için kullanılır. _Images_ kapsayıcısı, uygulamanın tam çözünürlüklü görüntüleri yüklediği yerdir. Serinin sonraki bölümlerinde bir Azure işlev uygulaması, yeniden boyutlandırılan küçük resimleri _thumbnails_ kapsayıcısına yükler. 

[az storage account keys list](/cli/azure/storage/account/keys#az_storage_account_keys_list) komutunu kullanarak depolama hesabı anahtarını alın. Daha sonra bu anahtar ile [az storage container create](/cli/azure/storage/container#az_storage_container_create) komutunu kullanarak iki kapsayıcı oluşturun.  
 
Bu örnekte `<blob_storage_account>`, oluşturduğunuz Blob depolama hesabının adıdır. _Images_ kapsayıcısının genel erişimi `off`, _thumbnails_ kapsayıcısının genel erişimi ise `container` olarak ayarlanır. `container` genel erişim ayarı, web sayfasını ziyaret eden kişilerin küçük resimleri görüntüleyebilmesine olanak tanır.
 
```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off 

az storage container create -n thumbnails --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your blob storage account key..." 
echo $blobStorageAccountKey 
``` 

Blob depolama hesabının adını ve anahtarını not edin. Örnek uygulama, görüntüleri karşıya yüklemek üzere depolama hesabına bağlanmak için bu ayarları kullanır. 

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma 

[App Service planı](../../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), uygulamanızı barındıran web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirtir. 

[az appservice plan create](/cli/azure/appservice/plan#az_appservice_plan_create) komutu ile bir App Service planı oluşturun. 

Aşağıdaki örnekte, **Ücretsiz** fiyatlandırma katmanı kullanılarak `myAppServicePlan` adlı bir App Service planı oluşturulmaktadır: 

```azurecli-interactive 
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE 
``` 

## <a name="create-a-web-app"></a>Web uygulaması oluşturma 

Web uygulaması, GitHub örnek deposundan dağıtılan örnek uygulama kodu için bir barındırma alanı sağlar. [az webapp create](/cli/azure/webapp#az_webapp_create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../../app-service/app-service-web-overview.md) oluşturun.  
 
Aşağıdaki komutta `<web_app>` kısmını benzersiz bir adla değiştirin (geçerli karakterler `a-z`, `0-9` ve `-` şeklindedir). `<web_app>` benzersiz değilse _Belirtilen `<web_app>` adına sahip web sitesi zaten var_ hata iletisiyle karşılaşırsınız. Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir.  

```azurecli-interactive 
az webapp create --name <web_app> --resource-group myResourceGroup --plan myAppServicePlan 
``` 

## <a name="deploy-the-sample-app-from-the-github-repository"></a>GitHub deposundan örnek uygulamayı dağıtma 

App Service bir web uygulamasına içerik dağıtmanın birkaç yolunu destekler. Bu öğreticide, web uygulamasını bir [genel GitHub örnek deposundan](https://github.com/Azure-Samples/storage-blob-upload-from-webapp) dağıtırsınız. [az webapp deployment source config](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) komutuyla Git dağıtımını web uygulamasında gerçekleşecek şekilde yapılandırın. `<web_app>` değerini önceki adımda oluşturduğunuz web uygulamasının adıyla değiştirin.

Örnek proje; görüntüyü kabul eden, depolama hesabına kaydeden ve küçük resim kapsayıcısından görüntüleri gösteren bir [ASP.NET MVC](https://www.asp.net/mvc) uygulaması içerir. Web uygulaması, Azure depolama ile etkileşimde bulunmak üzere Azure depolama İstemci Kitaplığından [Microsoft.WindowsAzure.Storage](/dotnet/api/microsoft.windowsazure.storage?view=azure-dotnet), [Microsoft.WindowsAzure.Storage.Blob](/dotnet/api/microsoft.windowsazure.storage.blob?view=azure-dotnet) ve [Microsoft.WindowsAzure.Storage.Auth](/dotnet/api/microsoft.windowsazure.storage.auth?view=azure-dotnet) ad alanlarını kullanır. 

```azurecli-interactive 
az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

## <a name="configure-web-app-settings"></a>Web uygulaması ayarlarını yapılandırma 

Örnek web uygulaması, görüntüleri karşıya yüklemek için kullanılan erişim belirteçlerini istemek için [Azure Depolama İstemci Kitaplığı](/dotnet/api/overview/azure/storage?view=azure-dotnet)’nı kullanır. Depolama SDK’sı tarafından kullanılan depolama hesabı kimlik bilgileri, web uygulaması için uygulama ayarlarında düzenlenir. [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) komutuyla, uygulama ayarlarını dağıtılmış uygulamaya ekleyin. 

Aşağıdaki komutta `<blob_storage_account>`, Blob depolama hesabınızın adı ve `<blob_storage_key>`, ilişkili anahtardır. `<web_app>` değerini önceki adımda oluşturduğunuz web uygulamasının adıyla değiştirin.     

```azurecli-interactive 
az webapp config appsettings set --name <web_app> --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=<blob_storage_account> \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbnails \
AzureStorageConfig__AccountKey=<blob_storage_key>  
``` 

Web uygulaması dağıtılıp yapılandırıldıktan sonra, uygulamadaki görüntüyü karşıya yükleme işlevini test edebilirsiniz.   

## <a name="upload-an-image"></a>Bir görüntüyü karşıya yükleme 

Web uygulamasını test etmek için, yayımlanan uygulamanızın URL'sine gidin. Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir. **Fotoğrafları karşıya yükle** bölgesini seçerek bir dosya seçip yükleyin ya da bölgenin üzerine bir dosya sürükleyip bırakın. Görüntü başarıyla karşıya yüklenirse kaybolur.

![ImageResizer uygulaması](media/storage-upload-process-images/figure1.png)

Örnek kodda `Storagehelper.cs` dosyasındaki `UploadFiletoStorage` görevi, görüntüleri [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) yöntemini kullanarak depolama hesabı içindeki `images` kapsayıcısına yüklemek için kullanılır. Aşağıdaki kod örneği `UploadFiletoStorage` görevini içerir. 

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

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Depolama hesabında görüntünün gösterildiğini doğrulayın

[Azure Portal](https://portal.azure.com) oturum açın. Soldaki menüden **Depolama hesapları**’nı ve sonra depolama hesabınızın adını seçin. **Genel Bakış** altında **images** kapsayıcısını seçin.

Kapsayıcıda görüntünün gösterildiğini doğrulayın.

![Görüntüler kapsayıcı görünümü](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Küçük resim görüntülemeyi test etme

Küçük resim görüntülemeyi test etmek için, küçük resim kapsayıcısına bir görüntü yükleyerek uygulamanın küçük resim kapsayıcısını okuyabildiğinden emin olmanız gerekir.

[Azure Portal](https://portal.azure.com) oturum açın. Soldaki menüden **Depolama hesapları**’nı ve sonra depolama hesabınızın adını seçin. **Blob Hizmeti** altında **Kapsayıcılar**’ı ve **thumbnails** kapsayıcısını seçin. **Karşıya Yükle**’yi seçerek **Blobu karşıya yükle** bölmesini açın.

Dosya seçicisini kullanarak bir dosya belirleyip **Karşıya Yükle**’yi seçin.

**thumbnails** kapsayıcısına yüklenen görüntünün görünür olduğunu doğrulamak için uygulamanıza geri gidin.

![Görüntüler kapsayıcı görünümü](media/storage-upload-process-images/figure2.png)

Azure portalındaki **thumbnails** kapsayıcısında, karşıya yüklediğiniz görüntüyü seçin ve **Sil**’i seçerek görüntüyü silin. Serinin ikinci bölümünde, küçük resim görüntülerini oluşturma işlemini otomatik hale getireceksiniz; bu nedenle bu test görüntüsü gerekli değildir.

CDN, Azure depolama hesabınızdan içeriği önbelleğe almak için etkinleştirilebilir. Bu öğreticide açıklanmamasına rağmen, Azure depolama hesabınızla CDN’yi etkinleştirme hakkında bilgi almak için şu makaleyi ziyaret edebilirsiniz: [Azure depolama hesabını Azure CDN ile tümleştirme](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Sonraki adımlar

Serinin birinci bölümünde, depolama ile etkileşimde bulunan bir web uygulamasını yapılandırma hakkında bilgi edindiniz, örneğin:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Kapsayıcı oluşturma ve izinleri ayarlama
> * Erişim anahtarı alma
> * Uygulama ayarlarını yapılandırma
> * Bir Web Uygulamasını Azure’a Dağıtma
> * Web uygulaması ile etkileşimde bulunma

Event Grid kullanarak bir görüntüyü yeniden boyutlandırmaya yönelik Azure işlevini tetikleme hakkında bilgi almak için serinin ikinci bölümüne ilerleyin.

> [!div class="nextstepaction"]
> [Karşıya yüklenen bir görüntüyü yeniden boyutlandırmak üzere bir Azure İşlevi’ni tetiklemek için Event Grid kullanma](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
