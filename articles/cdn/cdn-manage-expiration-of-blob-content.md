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
ms.openlocfilehash: 6f82ae396a17f903a522c716f73a5f7d2de660e7
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-content-delivery-network"></a>Azure içerik teslim ağı'nda Azure Blob storage'nın bitiş tarihini Yönet
> [!div class="op_single_selector"]
> * [Azure web içeriği](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Depolama](cdn-manage-expiration-of-blob-content.md)
> 
> 

[Blob Depolama hizmeti](../storage/common/storage-introduction.md#blob-storage) Azure depolama alanına birkaç Azure tabanlı kaynakları birini Azure içerik teslim ağı (CDN) ile tümleşiktir. Kendi yaşam süresi (TTL) geçen kadar herhangi bir genel olarak erişilebilir blob içerik Azure CDN'de önbelleğe alınabilir. TTL değeri tarafından belirlenir `Cache-Control` kaynak sunucudan HTTP yanıt üstbilgisi. Bu makalede ayarlayabileceğiniz çeşitli yollardan `Cache-Control` Azure storage'da bir blob üstbilgisi.

Önbellek ayarları Azure portalından ayarlayarak da kontrol edebilirsiniz [kuralları önbelleğe alma CDN](cdn-caching-rules.md). Bir veya daha fazla önbelleğe alma kurallarını ve önbelleğe alma davranışlarını kümesine **geçersiz kılma** veya **atlama önbellek**, bu makalede ele alınan kaynak tarafından sağlanan önbelleğe alma ayarlarını göz ardı edilir. Genel önbelleğe alma kavramları hakkında daha fazla bilgi için bkz: [önbelleğe alma nasıl çalışır](cdn-how-caching-works.md).

> [!TIP]
> Blob üzerindeki hiçbir TTL ayarlamayı da seçebilirsiniz. Bu durumda, Azure portalında kurallar önbelleğe almayı kurmak ayarlamazsanız Azure CDN varsayılan TTL yedi gün otomatik olarak uygular. Bu varsayılan TTL yalnızca genel web teslim iyileştirmeler için geçerlidir. Büyük dosya en iyi duruma getirme, varsayılan TTL bir gündür ve en iyi duruma getirme akış medya için TTL bir yıl varsayılandır.
> 
> BLOB'ları ve diğer dosyalara erişimi hızlandırmak için Azure CDN nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure içerik teslim ağı'ne genel bakış](cdn-overview.md).
> 
> Azure Blob Depolama hakkında daha fazla bilgi için bkz: [Blob Storage'a giriş](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).
 

## <a name="setting-cache-control-headers-by-using-azure-powershell"></a>Cache-Control üstbilgileri Azure PowerShell kullanarak ayarlama
[Azure PowerShell](/powershell/azure/overview) Azure hizmetlerinizi yönetmeyi hızlı ve etkili yollarından biri. Kullanım `Get-AzureStorageBlob` blob başvurusu almak için cmdlet ayarlamaktır `.ICloudBlob.Properties.CacheControl` özelliği. 

Örneğin:

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> PowerShell için de kullanabilirsiniz [CDN profili ve uç noktaları yönetmek](cdn-manage-powershell.md).
> 
>

## <a name="setting-cache-control-headers-by-using-net"></a>Cache-Control üstbilgileri .NET kullanarak ayarlama
Bir blob'un ayarlamak için `Cache-Control` .NET kod, kullanım kullanarak üstbilgi [.NET için Azure Storage istemci Kitaplığı](../storage/blobs/storage-dotnet-how-to-use-blobs.md) ayarlamak için [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) özelliği.

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
        blob.Properties.CacheControl = "max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> Daha fazla .NET kod örnekleri kullanılabilir [.NET için Azure Blob Depolama örnek](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 

## <a name="setting-cache-control-headers-by-using-other-methods"></a>Cache-Control üstbilgileri diğer yöntemleri kullanarak ayarlama

### <a name="azure-storage-explorer"></a>Azure Depolama Gezgini
İle [Azure Storage Gezgini](https://azure.microsoft.com/en-us/features/storage-explorer/), görüntüleyin ve özellikleri gibi dahil, blob storage kaynaklarını düzenleyin *CacheControl* özelliği. 

Güncelleştirilecek *CacheControl* Azure Depolama Gezgini ile bir blob özelliği:
   1. Bir blob seçin ve ardından **özellikleri** ve bağlam menüsünden. 
   2. Ekranı aşağı kaydırarak *CacheControl* özelliği.
   3. Bir değer girin ve ardından **kaydetmek**.


![Azure Storage Gezgini özellikleri](./media/cdn-manage-expiration-of-blob-content/cdn-storage-explorer-properties.png)

### <a name="azure-command-line-interface"></a>Azure Komut Satırı Arabirimi
İle [Azure komut satırı arabirimi](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest) (CLI), komut satırından Azure blob kaynakları yönetebilir. Azure CLI ile bir blob karşıya yüklediğinizde cache-control üstbilgisinin ayarlamak için ayarlayın *cacheControl* kullanarak özellik `-p` geçin. Aşağıdaki örnekte, TTL bir saat (3600 saniye) ayarlamak gösterilmektedir:
  
```azurecli
azure storage blob upload -c <connectionstring> -p cacheControl="max-age=3600" .\test.txt myContainer test.txt
```

### <a name="azure-storage-services-rest-api"></a>Azure storage services REST API'si
Kullanabileceğiniz [Azure storage Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx) açıkça ayarlamak için *x-ms-blob-cache-control* istek üzerine aşağıdaki işlemleri kullanarak özelliği:
  
   - [BLOB yerleştirme](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx)
   - [Engelleme listesi yerleştirme](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)
   - [Blob özelliklerini ayarlama](https://msdn.microsoft.com/library/azure/ee691966.aspx)

## <a name="testing-the-cache-control-header"></a>Cache-Control üstbilgisinin test etme
Bloblarınızın TTL ayarlarını kolayca doğrulayabilirsiniz. Tarayıcınızın ile [Geliştirici Araçları](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), blob içeren test `Cache-Control` yanıtı üstbilgisi. Bir aracı gibi kullanabilir [Wget](https://www.gnu.org/software/wget/), [Postman](https://www.getpostman.com/), veya [Fiddler](http://www.telerik.com/fiddler) yanıt üstbilgileri incelemek için.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure CDN içinde bulut hizmeti içeriğinin kullanım süresini yönetme öğrenin](cdn-manage-expiration-of-cloud-service-content.md)
* [Kavramları önbelleğe alma hakkında bilgi edinin](cdn-how-caching-works.md)

