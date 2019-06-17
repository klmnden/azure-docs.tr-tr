---
title: Azure Content Delivery Network Azure Blob storage'da kullanım süresini yönetme | Microsoft Docs
description: Azure CDN önbelleğe alma, BLOB'ları için yaşam süresi'ı denetleme seçenekleri hakkında bilgi edinin.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 02/1/2018
ms.author: mazha
ms.openlocfilehash: 89f821398f2bccf19a8be090de0e8788090670fb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080825"
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-cdn"></a>Azure Blob Depolama alanında Azure CDN kullanım süresini yönetme
> [!div class="op_single_selector"]
> * [Azure web içeriği](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Depolama](cdn-manage-expiration-of-blob-content.md)
> 
> 

[Blob Depolama hizmeti](../storage/common/storage-introduction.md#blob-storage) Azure depolama üzerinde Azure tabanlı çıkış noktaları birini Azure Content Delivery Network (CDN) ile tümleşiktir. Sona erdiğinde, yaşam süresi (TTL) kadar tüm genel olarak erişilebilir blob içeriği Azure CDN'de önbelleğe alınabilir. TTL değeri tarafından belirlenir `Cache-Control` kaynak sunucusundan gelen HTTP yanıt üst bilgisi. Bu makalede, ayarlayabileceğiniz birkaç şekilde `Cache-Control` Azure Depolama'daki bir blob üstbilgisi.

Önbellek ayarları Azure portalından ayarı CDN önbelleğe alma kuralları tarafından da kontrol edebilirsiniz. Önbelleğe alma kuralı oluşturun ve önbelleğe alma davranışını ayarlayın **geçersiz kılma** veya **önbelleği atla**, bu makalede ele alınan kaynak tarafından sağlanan önbelleğe alma ayarları göz ardı edilir. Genel önbelleğe alma kavramları hakkında daha fazla bilgi için bkz. [önbelleğe alma nasıl işler](cdn-how-caching-works.md).

> [!TIP]
> Hiçbir TTL bir bloba ayarlamak seçebilirsiniz. Bu durumda, Azure CDN, otomatik olarak önbelleğe alma kuralları Azure portalında ayarladığınız sürece varsayılan TTL yedi gün geçerlidir. Bu varsayılan TTL yalnızca genel web teslimatı iyileştirmeler için geçerlidir. Büyük dosya iyileştirmeler için varsayılan TTL bir gündür ve en iyi duruma getirme akış medya için varsayılan TTL bir yıldır.
> 
> Azure CDN'blob ' ları ve diğer dosyalara erişimi hızlandırmak için nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Content Delivery Network'e genel bakış](cdn-overview.md).
> 
> Azure Blob Depolama hakkında daha fazla bilgi için bkz. [Blob depolamaya giriş](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).
 

## <a name="setting-cache-control-headers-by-using-cdn-caching-rules"></a>CDN önbelleğe alma kuralları kullanarak Cache-Control üst bilgileri ayarlama
Bir blobun ayarlamak için tercih edilen yöntem `Cache-Control` başlığıdır Azure portalında önbelleğe alma kuralları kullanılacak. CDN önbelleğe alma kuralları hakkında daha fazla bilgi için bkz. [denetimi Azure CDN önbelleğe alma kuralları ile önbelleğe alma davranışını](cdn-caching-rules.md).

> [!NOTE] 
> Önbelleğe alma kuralları, yalnızca kullanılabilir **verizon'dan Azure CDN standart** ve **akamai'den Azure CDN standart** profilleri. İçin **verizon'dan Azure CDN Premium** profilleri kullanmalıdır [Azure CDN kurallar altyapısı](cdn-rules-engine.md) içinde **Yönet** benzer işlevselliği için portal.

**CDN önbelleğe alma kuralları sayfasına gitmek için**:

1. Azure portalında bir CDN profili seçin ve ardından blob için uç nokta seçin.

2. Ayarların altındaki sol bölmede **Önbelleğe alma kuralları**’nı seçin.

   ![CDN önbelleğe alma kuralları düğmesi](./media/cdn-manage-expiration-of-blob-content/cdn-caching-rules-btn.png)

   **Önbelleğe alma kuralları** sayfası görüntülenir.

   ![CDN önbelleğe alma sayfası](./media/cdn-manage-expiration-of-blob-content/cdn-caching-page.png)


**Genel önbelleğe alma kuralları kullanarak bir Blob Depolama hizmetinin Cache-Control üst bilgilerini ayarlamak için:**

1. Altında **genel önbelleğe alma kuralları**ayarlayın **sorgu dizesini önbelleğe alma davranışı** için **sorgu dizelerini Yoksay** ayarlayıp **önbelleğe alma davranışını** için **Geçersiz kılma**.
      
2. İçin **önbellek sona erme süresi**, 3600 içinde girin **saniye** kutusu veya 1'de **saat** kutusu. 

   ![CDN genel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-blob-content/cdn-global-caching-rules-example.png)

   Bu genel önbelleğe alma kuralı, bir önbellek süresi bir saat ayarlar ve uç noktaya yönelik tüm istekler etkiler. Tüm geçersiz kılmaları `Cache-Control` veya `Expires` bitiş noktası tarafından belirtilen kaynak sunucu tarafından gönderilen HTTP üstbilgileri.   

3. **Kaydet**’i seçin.
 
**Özel önbelleğe alma kuralları kullanarak blob dosyanın Cache-Control üst bilgilerini ayarlamak için:**

1. Altında **özel önbelleğe alma kuralları**, iki eşleşme koşul oluşturun:

     A. İlk eşleşme koşulu için **eşleşen koşul** için **yolu** girin `/blobcontainer1/*` için **eşleşecek değer**. Ayarlama **önbelleğe alma davranışını** için **geçersiz kılma** ve 4'te girin **saat** kutusu.

    B. İkinci eşleşme koşulu için **eşleşen koşul** için **yolu** girin `/blobcontainer1/blob1.txt` için **eşleşecek değer**. Ayarlama **önbelleğe alma davranışını** için **geçersiz kılma** ve 2'de girin **saat** kutusu.

    ![CDN özel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-blob-content/cdn-custom-caching-rules-example.png)

    İlk özel önbelleğe alma kuralı dört saat içinde blob dosyalar için önbellek süresi ayarlar `/blobcontainer1` , uç noktası tarafından belirtilen kaynak sunucusunda klasör. İlk kural ikinci kuralı geçersiz kılar `blob1.txt` yalnızca blob dosyası ve önbelleğe alma süresi, için iki saatlik bir ayarlar.

2. **Kaydet**’i seçin.


## <a name="setting-cache-control-headers-by-using-azure-powershell"></a>Cache-Control üst bilgiler, Azure PowerShell kullanarak ayarlama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[Azure PowerShell](/powershell/azure/overview) Azure hizmetlerinizi yönetmek için en hızlı ve en güçlü yollarından biridir. Kullanım `Get-AzStorageBlob` blob başvurusu almak için cmdlet ayarlayın `.ICloudBlob.Properties.CacheControl` özelliği. 

Örneğin:

```powershell
# Create a storage context
$context = New-AzStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> PowerShell de kullanabilirsiniz [CDN profili ve uç noktaları yönetme](cdn-manage-powershell.md).
> 
>

## <a name="setting-cache-control-headers-by-using-net"></a>.NET kullanarak Cache-Control üst bilgileri ayarlama
Bir blobun belirtmek için `Cache-Control` .NET kodu, kullanım kullanarak üstbilgi [.NET için Azure depolama istemci Kitaplığı](../storage/blobs/storage-dotnet-how-to-use-blobs.md) ayarlanacak [CloudBlob.Properties.CacheControl](/dotnet/api/microsoft.azure.storage.blob.blobproperties.cachecontrol) özelliği.

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
        CloudBlobContainer <container name> = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob <blob name> = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> Daha fazla .NET kod örneği kullanılabilir [.NET için Azure Blob Depolama örneklerini](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 

## <a name="setting-cache-control-headers-by-using-other-methods"></a>Başka yöntemler kullanarak Cache-Control üst bilgileri ayarlama

### <a name="azure-storage-explorer"></a>Azure Depolama Gezgini
İle [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/), görüntüleyebilir ve gibi özellikler dahil olmak üzere blob depolama kaynaklarını düzenleme *CacheControl* özelliği. 

Güncelleştirilecek *CacheControl* Azure Depolama Gezgini ile blob özelliği:
   1. Bir blobu seçin ve ardından **özellikleri** bağlam menüsünden. 
   2. Ekranı aşağı kaydırarak *CacheControl* özelliği.
   3. Bir değer girin ve ardından **Kaydet**.


![Azure Depolama Gezgini özellikleri](./media/cdn-manage-expiration-of-blob-content/cdn-storage-explorer-properties.png)

### <a name="azure-command-line-interface"></a>Azure Komut Satırı Arabirimi
İle [Azure komut satırı arabirimi](https://docs.microsoft.com/cli/azure) (CLI), Azure blob kaynaklarını komut satırından yönetebilirsiniz. Azure CLI ile bir blob karşıya yüklediğinizde, cache-control üst bilgisi ayarlamak için ayarlayın *cacheControl* kullanarak özellik `-p` geçin. Aşağıdaki örnekte, TTL değerini bir saat (3600 saniye) ayarlamak gösterilmektedir:
  
```azurecli
azure storage blob upload -c <connectionstring> -p cacheControl="max-age=3600" .\<blob name> <container name> <blob name>
```

### <a name="azure-storage-services-rest-api"></a>Azure depolama hizmetleri REST API'si
Kullanabileceğiniz [Azure depolama hizmetleri REST API'si](/rest/api/storageservices/) açıkça ayarlamak için *x-ms-blob-cache-control* istek üzerine aşağıdaki işlemleri kullanarak özelliği:
  
   - [Put Blob](/rest/api/storageservices/Put-Blob)
   - [Engelleme listesi yerleştirin](/rest/api/storageservices/Put-Block-List)
   - [Blob özelliklerini ayarlama](/rest/api/storageservices/Set-Blob-Properties)

## <a name="testing-the-cache-control-header"></a>Cache-Control üst bilgisi test etme
Bloblarınızın TTL ayarlarını kolayca doğrulayabilirsiniz. Tarayıcınızın ile [Geliştirici Araçları](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), blobunuza içeren test `Cache-Control` yanıtı üstbilgisi. Gibi bir araç kullanabilirsiniz [Wget](https://www.gnu.org/software/wget/), [Postman](https://www.getpostman.com/), veya [Fiddler](https://www.telerik.com/fiddler) yanıt üstbilgileri incelemek üzere.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure CDN bulut hizmeti içeriğinin kullanım süresini yönetme hakkında bilgi edinin](cdn-manage-expiration-of-cloud-service-content.md)
* [Önbelleğe alma kavramları hakkında bilgi edinin](cdn-how-caching-works.md)

