---
title: "Azure Storage ile bulutta bir uygulamanın verilerinin güvenli | Microsoft Docs"
description: "Bulutta uygulamanızın veri güvenliğini sağlamak için SAS belirteçler, şifreleme ve HTTPS kullanın"
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
ms.openlocfilehash: c43165e230a00b6a4408637fd2290a21800d07b9
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="secure-access-to-an-applications-data-in-the-cloud"></a>Bulutta bir uygulamanın verilere güvenli erişim

Bu öğretici üç serinin bir parçasıdır. Güvenli Depolama hesabı erişim öğrenin. 

Bölümünde dizisinin üç bilgi nasıl yapılır:

> [!div class="checklist"]
> * Küçük resimler erişmek için SAS belirteci kullanın
> * Sunucu Tarafı Şifrelemesi özelliğini Aç
> * Yalnızca HTTPS taşıma etkinleştir

[Azure blob depolama](../common/storage-introduction.md#blob-storage) uygulamaları için dosyalarını depolamak için sağlam bir hizmet sunar. Bu öğretici genişletir [önceki konu] [ previous-tutorial] bir web uygulamasından depolama hesabınıza güvenli erişim göstermek için. İşiniz bittiğinde görüntüleri şifrelenir ve küçük resmini erişmek için web uygulaması güvenli SAS belirteçlerini kullanır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için önceki depolama öğretici tamamlamış olmanız gerekir: [otomatikleştirme yeniden boyutlandırma karşıya olay kılavuz kullanarak görüntüleri][previous-tutorial]. 

## <a name="set-container-public-access"></a>Set kapsayıcısı genel erişim

Bu öğretici serisi bölümünde, SAS belirteci küçük resimleri erişmek için kullanılır. Bu adımda ayarladığınız ortak erişimini _başparmak_ kapsayıcıya `off`.

```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container set-permission \ --account-name $blobStorageAccount \ --account-key $blobStorageAccountKey \ --name thumbs  \
--public-access off
``` 

## <a name="configure-sas-tokens-for-thumbnails"></a>Küçük resimleri için SAS belirteci yapılandırın

Bölümünde Bu öğretici serilerinden biri, web uygulaması bir ortak kapsayıcı görüntülerden gösteren oluştu. Bu seriyi bölümünde kullandığınız [güvenli erişim imzası (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md#what-is-a-shared-access-signature) küçük resim görüntüleri almak için belirteçleri. SAS belirteci, bir kapsayıcı veya blob tabanlı IP, protokol, zaman aralığı veya izin verilen hakları kısıtlı erişim sağlamak izin verir.

Bu örnekte, kaynak kodu deposu kullanır `sasTokens` güncelleştirilmiş kod örneği sahip şube. Var olan GitHub dağıtımı Sil [az webapp dağıtım kaynağı silme](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_delete). Ardından, GitHub dağıtımı ile web uygulaması için yapılandırma [az webapp dağıtım kaynağı config](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) komutu.  

Aşağıdaki komutta `<web-app>` web uygulamanızın adıdır.  

```azurecli-interactive 
az webapp deployment source delete --name <web-app> --resource-group myResourceGroup

az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch sasTokens --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

`sasTokens` Depo güncelleştirmeleri dalı `StorageHelper.cs` dosya. Yerini `GetThumbNailUrls` aşağıdaki kod örneği olan görev. Küçük resim güncelleştirilmiş görev alır ayarlayarak URL'leri bir [SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet) başlangıç zamanı, bitiş saati ve SAS belirteci izinlerini belirtmek için. Artık web uygulaması dağıtıldığında küçük bir SAS belirteci kullanarak bir URL ile alır. Güncelleştirilmiş görev aşağıdaki örnekte gösterilmiştir:
    
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

    // Set the permission of the container to public
    await container.SetPermissionsAsync(new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

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

Aşağıdaki sınıfları, özellikleri ve yöntemleri önceki görevde kullanılır:

|Sınıf  |Özellikler| Yöntemler  |
|---------|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)    |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)     | |[CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)        |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     | |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)     | |[SetPermissionsAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.setpermissionsasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_SetPermissionsAsync_Microsoft_WindowsAzure_Storage_Blob_BlobContainerPermissions_) <br> [ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmentedAsync_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_)       |
|[BlobContinuationToken](/dotnet/api/microsoft.windowsazure.storage.blob.blobcontinuationtoken?view=azure-dotnet)     |         |
|[BlobResultSegment](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment?view=azure-dotnet)    | [Sonuçları](/dotnet/api/microsoft.windowsazure.storage.blob.blobresultsegment.results?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobResultSegment_Results)         |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)    |         | [GetSharedAccessSignature](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_GetSharedAccessSignature_Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_)
|[SharedAccessBlobPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy?view=azure-dotnet)     | [SharedAccessStartTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessstarttime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessStartTime)<br>[SharedAccessExpiryTime](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.sharedaccessexpirytime?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_SharedAccessExpiryTime)<br>[İzinler](/dotnet/api/microsoft.windowsazure.storage.blob.sharedaccessblobpolicy.permissions?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_SharedAccessBlobPolicy_Permissions) |        |

## <a name="server-side-encryption"></a>Sunucu tarafı şifreleme

[Azure Storage hizmeti şifreleme (SSE)](../common/storage-service-encryption.md) korumak ve verilerinizi korumaya yardımcı olur. SSE şifreleme, şifre çözme ve anahtar yönetimi işleme rest, verileri şifreler. Tüm veriler, 256 bit kullanılarak şifrelenir [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), güçlü blok birini şifrelemeleri kullanılabilir.

Aşağıdaki örnekte BLOB'lar için şifrelemeyi etkinleştirir. Mevcut BLOB'ları şifreleme etkinleştirmeden önce oluşturulan şifrelenmez. `x-ms-server-encrypted` Bir blob için bir istek başlığında blob şifreleme durumunu gösterir.

```azurecli-interactive
az storage account update --encryption-services blob --name <storage-account-name> --resource-group myResourceGroup
```

Şifreleme etkin göre yeni bir resim web uygulamasını yükleyin.

Kullanarak `curl` anahtarıyla `-I` yalnızca üstbilgileri almak için kendi değerlerinizi yerleştirin `<storage-account-name>`, `<container>`, ve `<blob-name>`.  

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<blob-name>?$sasToken -I
```

Yanıtta, Not `x-ms-server-encrypted` üstbilgi gösterir `true`. Bu üst verileri SSE ile artık şifrelenmiş olduğunu tanımlar.

```
HTTP/1.1 200 OK
Content-Length: 209489
Content-Type: image/png
Last-Modified: Mon, 11 Sep 2017 19:27:42 GMT
Accept-Ranges: bytes
ETag: "0x8D4F94B2BE76D45"
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 57047db3-001e-0050-3e34-2ba769000000
x-ms-version: 2017-04-17
x-ms-lease-status: unlocked
x-ms-lease-state: available
x-ms-blob-type: BlockBlob
x-ms-server-encrypted: true
Date: Mon, 11 Sep 2017 19:27:46 GMT
```

## <a name="enable-https-only"></a>Yalnızca HTTPS etkinleştirme

Veri istekleri için ve bir depolama hesabından güvenli olduğundan emin olmak için yalnızca HTTPS isteklerine sınırlayabilirsiniz. Depolama hesabı gerekli protokolü kullanarak güncelleştirme [az depolama hesabı güncelleştirme](/cli/azure/storage/account#az_storage_account_update) komutu.

```azurecli-interactive
az storage account update --resource-group myresourcegroup --name <storage-account-name> --https-only true
```

Bağlantı kullanarak test `curl` kullanarak `HTTP` protokolü.

```azurecli-interactive
curl http://<storage-account-name>.blob.core.windows.net/<container>/<blob-name> -I
```

Güvenli aktarımı gereklidir, şu iletiyi alırsınız:

```
HTTP/1.1 400 The account being accessed does not support http.
```

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde seri üç, güvenli nasıl gibi depolama hesabı erişim öğrendiniz:

> [!div class="checklist"]
> * Küçük resimler erişmek için SAS belirteci kullanın
> * Sunucu Tarafı Şifrelemesi özelliğini Aç
> * Yalnızca HTTPS taşıma etkinleştir

İzleme ve bulut depolama uygulama sorunlarını giderme konusunda bilgi almak için dizisinin dört bölümü için ilerleyin.

> [!div class="nextstepaction"]
> [İzleme ve uygulama bulut uygulama depolama sorun giderme](storage-monitor-troubleshoot-storage-application.md)

[previous-tutorial]: ../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json