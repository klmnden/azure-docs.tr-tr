---
title: "Azure Storage ile bulutta görüntü veri yükleme | Microsoft Docs"
description: "Uygulama verilerini depolamak için bir web uygulaması ile Azure blob storage kullanma"
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 8d187e51cbb391ee1f34fb5934c8ae1868bb6244
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="upload-image-data-in-the-cloud-with-azure-storage"></a>Azure Storage ile bulutta görüntü verilerini karşıya yükleme

Bu öğretici bir dizi birini bir parçasıdır. Bu öğretici, bir depolama hesabına görüntüleri yüklemek için Azure Storage istemci kitaplığı kullanan bir web uygulaması dağıtma gösterir. İşlemi tamamladığınızda, depolama ve Azure depolama biriminden görüntüleri görüntüleme bir web uygulamasına sahip.

![Görüntüleri kapsayıcı görünümü](media/storage-upload-process-images/figure2.png)

Bölümünde bir dizi öğrenin nasıl yapılır:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Bir kapsayıcı oluşturmak ve izinleri ayarlama
> * Erişim anahtarı alma
> * Uygulama ayarlarını yapılandırın
> * Azure için Web uygulaması dağıtma
> * Web uygulaması ile etkileşim

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma 

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.
 
Aşağıdaki örnek `myResourceGroup` adlı bir kaynak grubu oluşturur.
 
```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus 
``` 

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma
 
Örnek bir Azure depolama hesabındaki bir blob kapsayıcısını görüntüleri yükler. Bir depolama hesabı depolamak ve Azure storage veri nesnelerinizi erişmek için benzersiz bir ad alanı sağlar. Oluşturduğunuz kaynak grubunda [az storage account create](/cli/azure/storage/account#az_storage_account_create) komutunu kullanarak bir depolama hesabı oluşturun. 

> [!IMPORTANT] 
> Öğretici 2. parçası olay abonelikleri blob depolama alanını kullanın. Olay abonelikleri olan şu anda yalnızca Batı Orta ABD ve Batı ABD 2 Blob storage hesapları için desteklenir. Bu sınırlama nedeniyle örnek uygulama tarafından görüntüler ve küçük resimleri depolamak için kullanılan bir Blob storage hesabı oluşturmanız gerekir.   

Aşağıdaki komutta, kendi gördüğünüz Blob storage hesabı için genel olarak benzersiz bir ad yerine `<blob_storage_account>` yer tutucu.  

```azurecli-interactive 
az storage account create --name <blob_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
``` 
 
## <a name="create-blob-storage-containers"></a>BLOB storage kapsayıcıları oluşturma
 
Uygulama Blob Depolama hesabında iki kapsayıcı kullanır. Kapsayıcıları klasörlere benzer ve blobları depolamak için kullanılır. _Görüntüleri_ kapsayıcıdır burada tam çözünürlüklü görüntüleri uygulamayı yükler. Serinin sonraki bir parçası yeniden boyutlandırılan resim küçük resimleri için bir Azure işlevi uygulama yükler _başparmak_ kapsayıcı. 

Depolama hesabı anahtarı kullanarak alma [az depolama hesabı anahtarları listesi](/cli/azure/storage/account/keys#list) komutu. Ardından bu anahtarı kullanarak iki kapsayıcı oluşturmak için kullandığınız [az depolama kapsayıcısı oluşturmak](/cli/azure/storage/container#az_storage_container_create) komutu.  
 
Bu durumda, `<blob_storage_account>` oluşturduğunuz Blob Depolama hesabının adıdır. _Görüntüleri_ kapsayıcıları genel erişim ayarlanmış `off`, _başparmak_ kapsayıcıları genel erişim ayarlanmış `container`. `container` Genel erişim ayarı web sayfasını ziyaret edin kişilere görüntülenebilmesi küçük resimleri sağlar.
 
```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off 

az storage container create -n thumbs --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your blob storage account key..." 
echo $blobStorageAccountKey 
``` 

Blob depolama hesabı adı ve anahtarı not edin. Örnek uygulaması, resimleri karşıya yüklemek için depolama hesabına bağlanmak için bu ayarları kullanır. 

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma 

[App Service planı](../../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), uygulamanızı barındıran web sunucusu grubunun konumunu, boyutunu ve özelliklerini belirtir. 

[az appservice plan create](/cli/azure/appservice/plan#az_appservice_plan_create) komutu ile bir App Service planı oluşturun. 

Aşağıdaki örnekte, **Ücretsiz** fiyatlandırma katmanı kullanılarak `myAppServicePlan` adlı bir App Service planı oluşturulmaktadır: 

```azurecli-interactive 
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE 
``` 

## <a name="create-a-web-app"></a>Web uygulaması oluşturma 

Web uygulaması GitHub örnek depodan dağıtılan örnek uygulama kodu için bir barındırma alanı sağlar. [az webapp create](/cli/azure/webapp#az_webapp_create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../../app-service/app-service-web-overview.md) oluşturun.  
 
Aşağıdaki komutta, `<web_app>` benzersiz bir ad ile (geçerli karakterler `a-z`, `0-9`, ve `-`). Varsa `<web_app>` olan benzersiz değil, hata iletisini alırsınız: _verilen ada sahip Web sitesi `<web_app>` zaten mevcut._ Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir.  

```azurecli-interactive 
az webapp create --name <web_app> --resource-group myResourceGroup --plan myAppServicePlan 
``` 

## <a name="deploy-the-sample-app-from-the-github-repository"></a>GitHub deposuna örnek uygulamasını dağıtın 

Uygulama hizmeti bir web uygulaması için içerik dağıtmak için çeşitli yollar destekler. Bu öğreticide dağıttığınız web uygulamasından bir [genel GitHub örnek depo](https://github.com/Azure-Samples/storage-blob-upload-from-webapp). GitHub dağıtımı ile web uygulaması için yapılandırma [az webapp dağıtım kaynağı config](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) komutu. Değiştir `<web_app>` önceki adımda oluşturduğunuz web uygulaması adı.

Örnek Proje içeren bir [ASP.NET MVC](https://www.asp.net/mvc) bir görüntü kabul uygulama bir depolama hesabına kaydeder ve bir küçük resim kapsayıcı görüntülerden görüntüler. Web uygulaması kullanır [Microsoft.WindowsAzure.Storage](/dotnet/api/microsoft.windowsazure.storage?view=azure-dotnet), [Microsoft.WindowsAzure.Storage.Blob](/dotnet/api/microsoft.windowsazure.storage.blob?view=azure-dotnet)ve [Microsoft.WindowsAzure.Storage.Auth](/dotnet/api/microsoft.windowsazure.storage.auth?view=azure-dotnet) Azure storage istemci kitaplığı Azure storage ile etkileşim kurmak için ad alanlarını. 

```azurecli-interactive 
az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

## <a name="configure-web-app-settings"></a>Web uygulaması ayarlarını yapılandır 

Örnek web uygulamasını kullandığı [Azure Storage istemci Kitaplığı](/dotnet/api/overview/azure/storage?view=azure-dotnet) resimleri karşıya yüklemek için kullanılan istek erişim belirteçleri için. Depolama SDK'sı tarafından kullanılan depolama hesabı kimlik bilgileri, web uygulaması için uygulama ayarları'nda ayarlanır. Dağıtılmış uygulama için uygulama ayarları ekleyin [az webapp config appsettings kümesi](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) komutu. 

Aşağıdaki komutta `<blob_storage_account>` Blob Depolama hesabı adı ve `<blob_storage_key>` ilişkili anahtar. Değiştir `<web_app>` önceki adımda oluşturduğunuz web uygulaması adı.     

```azurecli-interactive 
az webapp config appsettings set --name <web_app> --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=<blob_storage_account> \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbs \
AzureStorageConfig__AccountKey=<blob_storage_key>  
``` 

Web uygulaması dağıtılan yapılandırıldıktan sonra görüntüyü karşıya yükleme işlevselliği uygulamada test edebilirsiniz.   

## <a name="upload-an-image"></a>Bir görüntüyü karşıya yükleme 

Web uygulaması test etmek için yayımlanan uygulamanızın URL'ye gidin. Web uygulamasının varsayılan URL'si `https://<web_app>.azurewebsites.net` şeklindedir. Seçin **karşıya fotoğraf** bölgeyi seçin ve bir dosyayı karşıya yüklemeyi veya sürükleyip bölge dosyası. Görüntüyü başarıyla karşıya yüklediyseniz kaybolur.

![ImageResizer uygulama](media/storage-upload-process-images/figure1.png)

Örnek kodda `UploadFiletoStorage` içinde görev `Storagehelper.cs` dosya görüntülere karşıya yüklemek için kullanılan `images` depolama hesabı kullanarak içinde kapsayıcı [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) yöntemi. Aşağıdaki kod örneği içeren `UploadFiletoStorage` görev. 

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

Aşağıdaki sınıflar ve yöntemler önceki görevde kullanılır:

|Sınıf  |Yöntem  |
|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)     |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)    |  [CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)       |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)    | [GetBlockBlobReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.getblockblobreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_GetBlockBlobReference_System_String_)        |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)     | [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet)        |

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Görüntü depolama hesabında gösterilen doğrulayın

[Azure Portal](https://portal.azure.com) oturum açın. Sol menüden seçin **depolama hesapları**, depolama hesabınızın adını seçin. Altında **genel bakış**seçin **görüntüleri** kapsayıcı.

Görüntü kapsayıcısında gösterilen doğrulayın.

![Görüntüleri kapsayıcı görünümü](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Test küçük resim görüntüleme

Küçük resim görüntüleme sınamak için uygulama küçük resim kapsayıcı okuyabilir sağlamak için küçük resim kapsayıcıya görüntüyü yükleyeceksiniz.

[Azure Portal](https://portal.azure.com) oturum açın. Sol menüden seçin **depolama hesapları**, depolama hesabınızın adını seçin. Seçin **kapsayıcıları** altında **Blob hizmeti** seçip **başparmak** kapsayıcı. Seçin **karşıya** açmak için **karşıya yükleme blob** bölmesi.

Dosya seçicisini kullanarak bir dosya seçip **karşıya**.

Görüntüyü karşıya yüklenen doğrulamak için uygulamanızın geri gidin **başparmak** kapsayıcıdır görünür.

![Görüntüleri kapsayıcı görünümü](media/storage-upload-process-images/figure2.png)

İçinde **başparmak** Azure portalında kapsayıcısı seçin görüntünün karşıya ve seçin **silmek** görüntüyü silmek için. Bu test görüntüsü gerekli olmadığı için bölümünde seri iki, küçük resimler oluşturma otomatikleştirdiğiniz.

CDN önbellek içeriği için Azure storage hesabınızdan etkinleştirilebilir. Bu öğreticide açıklanmayan olsa da, Azure storage hesabınızla CDN'yi etkinleştirme hakkında bilgi edinmek için ziyaret edebilirsiniz: [Azure storage hesabı Azure CDN ile tümleştirmek](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde bir dizi depolama nasıl gibi etkileşimde bir web uygulaması yapılandırma hakkında öğrenilen:

> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Bir kapsayıcı oluşturmak ve izinleri ayarlama
> * Erişim anahtarı alma
> * Uygulama ayarlarını yapılandırın
> * Azure için Web uygulaması dağıtma
> * Web uygulaması ile etkileşim

Görüntüyü yeniden boyutlandırmak için bir Azure işlevi tetiklemek için olay kılavuz kullanma hakkında bilgi edinmek için seriyi iki kısmına ilerleyin.

> [!div class="nextstepaction"]
> [Karşıya yüklenen görüntü yeniden boyutlandırmak için bir Azure işlevi tetiklemek için olay kılavuzunu kullanın](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
