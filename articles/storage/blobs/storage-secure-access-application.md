---
title: Azure Depolama ile bulutta uygulama verilerine erişimin güvenliğini sağlama | Microsoft Docs
description: Bulutta uygulamanızın veri güvenliğini sağlamak için SAS belirteçlerini, şifrelemeyi ve HTTPS’yi kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 05/30/2018
ms.author: tamram
ms.reviewer: cbrooks
ms.custom: mvc
ms.openlocfilehash: 8e56b02b84c0324f723ead1bbf156c847edbbeb5
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787982"
---
# <a name="secure-access-to-an-applications-data-in-the-cloud"></a>Bulutta uygulama verilerine erişimin güvenliğini sağlama

Bu öğretici, bir serinin üçüncü bölümüdür. Depolama hesabına erişim güvenliğinin nasıl sağlanacağını öğrenirsiniz. 

Serinin üçüncü bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Küçük resimlere erişmek için SAS belirteçlerini kullanma
> * Sunucu tarafı şifrelemesini açma
> * Yalnızca HTTPS taşımasını etkinleştirme

[Azure blob depolama](../common/storage-introduction.md#blob-storage), uygulamalara ilişkin dosyaları depolamak için sağlam bir hizmet sağlar. Bu öğretici, bir web uygulamasından depolama hesabınıza erişim güvenliğinin nasıl sağlanacağını göstermek için [önceki konuyu][previous-tutorial] genişletir. İşiniz bittiğinde görüntüler şifrelenir ve web uygulaması, küçük resimlere erişmek için güvenli SAS belirteçlerini kullanır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için önceki şu depolama öğreticisini tamamlamış olmanız gerekir: [Event Grid kullanarak karşıya yüklenen görüntüleri yeniden boyutlandırmayı otomatikleştirme][previous-tutorial]. 

## <a name="set-container-public-access"></a>Kapsayıcı genel erişimini ayarlama

Öğretici serisinin bu kısmında, küçük resimlere erişmek için SAS belirteçleri kullanılır. Bu adımda, _thumbnails_ kapsayıcısının genel erişimini `off` olarak ayarlarsınız.

```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container set-permission \ --account-name $blobStorageAccount \ --account-key $blobStorageAccountKey \ --name thumbnails  \
--public-access off
``` 

## <a name="configure-sas-tokens-for-thumbnails"></a>Küçük resimler için SAS belirteçlerini yapılandırma

Bu öğretici serisinin birinci kısmında web uygulaması, bir genel kapsayıcıdaki görüntüleri gösteriyordu. Serinin bu kısmında, küçük resimleri almak için [Paylaşılan Erişim İmzası (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md#what-is-a-shared-access-signature) belirteçlerini kullanırsınız. SAS belirteçleri; IP, protokol, zaman aralığı veya izin verilen haklar temelinde bir kapsayıcıya ya da bloba kısıtlı erişim sağlamanıza olanak verir.

Bu örnekte kaynak kod deposu, güncelleştirilmiş bir kod örneği içeren `sasTokens` dalını kullanır. [az webapp deployment source delete](/cli/azure/webapp/deployment/source) komutuyla mevcut GitHub dağıtımını silin. Sonra [az webapp deployment source config](/cli/azure/webapp/deployment/source) komutuyla web uygulamasına GitHub dağıtımını yapılandırın.  

Aşağıdaki komutta `<web-app>`, web uygulamanızın adıdır.  

```azurecli-interactive 
az webapp deployment source delete --name <web-app> --resource-group myResourceGroup

az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch sasTokens --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

Deponun `sasTokens` dalı, `StorageHelper.cs` dosyasını güncelleştirir. `GetThumbNailUrls` görevini, aşağıdaki kod örneğiyle değiştirir. Güncelleştirilmiş görev, SAS belirtecine yönelik izinleri, başlangıç zamanını ve süre sonunu belirtmek için bir [SharedAccessBlobPolicy](/dotnet/api/microsoft.azure.storage.blob.sharedaccessblobpolicy) ayarlayarak küçük resim URL’lerini alır. Dağıtıldıktan sonra web uygulaması artık bir SAS belirteci kullanarak URL ile küçük resimleri alır. Aşağıdaki örnekte güncelleştirilmiş görev gösterilmektedir:
    
```csharp
public static async Task<List<string>> GetThumbNailUrls(AzureStorageConfig _storageConfig)
{
    List<string> thumbnailUrls = new List<string>();

    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create blob client
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the container
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ThumbnailContainer);

    BlobContinuationToken continuationToken = null;

    BlobResultSegment resultSegment = null;

    //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
    //When the continuation token is null, the last page has been returned and execution can exit the loop.
    do
    {
        //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
        //or by calling a different overload.
        resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);

        foreach (var blobItem in resultSegment.Results)
        {
            CloudBlockBlob blob = blobItem as CloudBlockBlob;
            //Set the expiry time and permissions for the blob.
            //In this case, the start time is specified as a few minutes in the past, to mitigate clock skew.
            //The shared access signature will be valid immediately.
            SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

            sasConstraints.SharedAccessStartTime = DateTimeOffset.UtcNow.AddMinutes(-5);

            sasConstraints.SharedAccessExpiryTime = DateTimeOffset.UtcNow.AddHours(24);

            sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

            //Generate the shared access signature on the blob, setting the constraints directly on the signature.
            string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

            //Return the URI string for the container, including the SAS token.
            thumbnailUrls.Add(blob.Uri + sasBlobToken);

        }

        //Get the continuation token.
        continuationToken = resultSegment.ContinuationToken;
    }

    while (continuationToken != null);

    return await Task.FromResult(thumbnailUrls);
}
```

Yukarıdaki görevde, aşağıdaki sınıflar, özellikler ve yöntemler kullanılır:

|Sınıf  |Özellikler| Yöntemler  |
|---------|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.azure.cosmos.table.storagecredentials)    |         |
|[CloudStorageAccount](/dotnet/api/microsoft.azure.cosmos.table.cloudstorageaccount)     | |[CreateCloudBlobClient](/dotnet/api/microsoft.azure.storage.blob.blobaccountextensions.createcloudblobclient)        |
|[CloudBlobClient](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient)     | |[GetContainerReference](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getcontainerreference)         |
|[CloudBlobContainer](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer)     | |[SetPermissionsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setpermissionsasync) <br> [ListBlobsSegmentedAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobssegmentedasync)       |
|[BlobContinuationToken](/dotnet/api/microsoft.azure.storage.blob.blobcontinuationtoken)     |         |
|[BlobResultSegment](/dotnet/api/microsoft.azure.storage.blob.blobresultsegment)    | [Sonuçlar](/dotnet/api/microsoft.azure.storage.blob.blobresultsegment.results)         |
|[CloudBlockBlob](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob)    |         | [GetSharedAccessSignature](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getsharedaccesssignature)
|[SharedAccessBlobPolicy](/dotnet/api/microsoft.azure.storage.blob.sharedaccessblobpolicy)     | [SharedAccessStartTime](/dotnet/api/microsoft.azure.storage.blob.sharedaccessblobpolicy.sharedaccessstarttime)<br>[SharedAccessExpiryTime](/dotnet/api/microsoft.azure.storage.blob.sharedaccessblobpolicy.sharedaccessexpirytime)<br>[İzinler](/dotnet/api/microsoft.azure.storage.blob.sharedaccessblobpolicy.permissions) |        |

## <a name="server-side-encryption"></a>Sunucu tarafı şifrelemesi

[Azure Depolama Hizmeti Şifrelemesi (SSE)](../common/storage-service-encryption.md), verilerinizi korumanıza ve muhafaza etmenize yardımcı olur. SSE, bekleyen verileri şifreleyerek şifreleme, şifre çözme ve anahtar yönetimini işler. Verilerin tamamı, mevcut en güçlü blok şifreleme özelliklerinden biri olan 256 bit [AES şifrelemesi](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) ile şifrelenir.

SSE tüm performans katmanları (Standart ve Premium), tüm dağıtım modelleri (Azure Resource Manager ve Klasik) ve tüm Azure Depolama hizmetlerinde (Blob, Kuyruk, Tablo ve Dosya) verileri otomatik olarak şifreler. 

## <a name="enable-https-only"></a>Yalnızca HTTPS'yi etkinleştirme

Depolama hesabına gelen ve depolama hesabından giden isteklerin güvenli olduğundan emin olmak için istekleri yalnızca HTTPS ile sınırlayabilirsiniz. [az storage account update](/cli/azure/storage/account) komutunu kullanarak depolama hesabı için gerekli protokolü güncelleştirin.

```azurecli-interactive
az storage account update --resource-group myresourcegroup --name <storage-account-name> --https-only true
```

`HTTP` protokolünü kullanarak `curl` kullanan bağlantıyı test edin.

```azurecli-interactive
curl http://<storage-account-name>.blob.core.windows.net/<container>/<blob-name> -I
```

Artık güvenli aktarım gerekli olduğundan şu iletiyi alırsınız:

```
HTTP/1.1 400 The account being accessed does not support http.
```

## <a name="next-steps"></a>Sonraki adımlar

Serinin üçüncü kısmında, aşağıda örnekleri verilen eylemlerle birlikte depolama hesabına erişim güvenliğinin nasıl sağlanacağını öğrendiniz:

> [!div class="checklist"]
> * Küçük resimlere erişmek için SAS belirteçlerini kullanma
> * Sunucu tarafı şifrelemesini açma
> * Yalnızca HTTPS taşımasını etkinleştirme

Bulut depolama uygulamasının nasıl izleneceğini ve sorunlarının nasıl giderileceğini öğrenmek için serinin dördüncü kısmına ilerleyin.

> [!div class="nextstepaction"]
> [Uygulama bulut uygulama depolamasını izleme ve sorunlarını giderme](storage-monitor-troubleshoot-storage-application.md)

[previous-tutorial]: ../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json
