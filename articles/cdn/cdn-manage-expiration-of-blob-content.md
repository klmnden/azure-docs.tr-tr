---
title: "Azure içerik teslim ağı'nda Azure Blob storage'nın süresinin dolmasını yönetme | Microsoft Docs"
description: "Azure CDN önbelleğe alma işleminde, BLOB'ları için yaşam süresi denetleme seçenekleri hakkında bilgi edinin."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/10/2017
ms.author: mazha
ms.openlocfilehash: c2b49058ec7dd52b5063e815447697fa17ddb53a
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-content-delivery-network"></a>Azure içerik teslim ağı'nda Azure Blob storage'nın bitiş tarihini Yönet
> [!div class="op_single_selector"]
> * [Azure web içeriği](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Depolama](cdn-manage-expiration-of-blob-content.md)
> 
> 

[Blob Depolama hizmeti](../storage/common/storage-introduction.md#blob-storage) Azure depolama alanına birkaç Azure tabanlı kaynakları birini Azure içerik teslim ağı (CDN) ile tümleşiktir. Kendi yaşam süresi (TTL) geçen kadar herhangi bir genel olarak erişilebilir blob içerik Azure CDN'de önbelleğe alınabilir. TTL değeri tarafından belirlenir `Cache-Control` kaynak sunucudan HTTP yanıt üstbilgisi. Bu makalede ayarlayabileceğiniz çeşitli yollardan `Cache-Control` Azure storage'da bir blob üstbilgisi.

> [!TIP]
> Blob üzerindeki hiçbir TTL ayarlamayı da seçebilirsiniz. Bu durumda, Azure CDN varsayılan TTL yedi gün otomatik olarak uygular.
> 
> BLOB'ları ve diğer dosyalara erişimi hızlandırmak için Azure CDN nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure içerik teslim ağı'ne genel bakış](cdn-overview.md).
> 
> Azure Blob Depolama hakkında daha fazla bilgi için bkz: [Blob Storage'a giriş](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-introduction).
 

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) Azure hizmetlerinizi yönetmeyi hızlı ve etkili yollarından biri. Kullanım `Get-AzureStorageBlob` blob başvurusu almak için cmdlet ayarlamaktır `.ICloudBlob.Properties.CacheControl` özelliği. 

Örneğin:

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> PowerShell için de kullanabilirsiniz [CDN profili ve uç noktaları yönetmek](cdn-manage-powershell.md).
> 
>

## <a name="azure-storage-client-library-for-net"></a>.NET için Azure Storage istemci kitaplığı
Bir blob'un ayarlamak için `Cache-Control` .NET, kullanım kullanarak üstbilgi [.NET için Azure Storage istemci Kitaplığı](../storage/blobs/storage-dotnet-how-to-use-blobs.md) ayarlamak için [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) özelliği.

Örneğin:

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> Daha fazla .NET kod örnekleri kullanılabilir [.NET için Azure Blob Depolama örnek](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 
> 

## <a name="other-methods"></a>Diğer yöntemleri
* [Azure Komut Satırı Arabirimi](../cli-install-nodejs.md)
  
    Bir blob karşıya yüklediğinizde, ayarlayabileceğiniz *cacheControl* kullanarak özellik `-p` geçiş Azure komut satırı arabirimi. Aşağıdaki örnek, bir saat (3600 saniye) TTL ayarlar:
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    Açık olarak ayarlanıp *x-ms-blob-cache-control* özelliği bir [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put engelleme listesi](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), veya [Blob özelliklerini ayarla](https://msdn.microsoft.com/library/azure/ee691966.aspx) isteği.

* Üçüncü taraf Depolama Yönetimi Araçları
  
    Bazı üçüncü taraf Azure Depolama Yönetimi Araçları ayarlamanıza izin **CacheControl** BLOB'lar özelliği. 

## <a name="testing-the-cache-control-header"></a>Cache-Control üstbilgisinin test etme
Bloblarınızın TTL ayarlarını kolayca doğrulayabilirsiniz. Tarayıcınızın ile [Geliştirici Araçları](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), blob içeren test `Cache-Control` yanıtı üstbilgisi. Bir aracı gibi kullanabilir **wget**, [Postman](https://www.getpostman.com/), veya [Fiddler](http://www.telerik.com/fiddler) yanıt üstbilgileri incelemek için.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure CDN içinde bulut hizmeti içeriğinin kullanım süresini yönetme öğrenin](cdn-manage-expiration-of-cloud-service-content.md)

