---
title: Azure içerik teslim ağı'nda Azure Blob storage'nın süresinin dolmasını yönetme | Microsoft Docs
description: Azure CDN önbelleğe alma işleminde, BLOB'ları için yaşam süresi denetleme seçenekleri hakkında bilgi edinin.
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
ms.openlocfilehash: a0f89a272fa300f6acced2de02ba5465ab282079
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33765645"
---
# <a name="manage-expiration-of-azure-blob-storage-in-azure-cdn"></a>Azure CDN Azure Blob storage'da, bitiş tarihini Yönet
> [!div class="op_single_selector"]
> * [Azure web içeriği](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob Depolama](cdn-manage-expiration-of-blob-content.md)
> 
> 

[Blob Depolama hizmeti](../storage/common/storage-introduction.md#blob-storage) Azure depolama alanına birkaç Azure tabanlı kaynakları birini Azure içerik teslim ağı (CDN) ile tümleşiktir. Kendi yaşam süresi (TTL) geçen kadar herhangi bir genel olarak erişilebilir blob içerik Azure CDN'de önbelleğe alınabilir. TTL değeri tarafından belirlenir `Cache-Control` kaynak sunucudan HTTP yanıt üstbilgisi. Bu makalede ayarlayabileceğiniz çeşitli yollardan `Cache-Control` Azure storage'da bir blob üstbilgisi.

Önbellek ayarları Azure portalından ayarlayarak da kontrol edebilirsiniz [kuralları önbelleğe alma CDN](#setting-cache-control-headers-by-using-caching-rules). Bir önbellek kuralı oluşturabilir ve önbelleğe alma davranışını ayarlamak **geçersiz kılma** veya **atlama önbellek**, bu makalede ele alınan kaynak tarafından sağlanan önbelleğe alma ayarlarını göz ardı edilir. Genel önbelleğe alma kavramları hakkında daha fazla bilgi için bkz: [önbelleğe alma nasıl çalışır](cdn-how-caching-works.md).

> [!TIP]
> Blob üzerindeki hiçbir TTL ayarlamayı da seçebilirsiniz. Bu durumda, Azure portalında kurallar önbelleğe almayı kurmak ayarlamazsanız Azure CDN varsayılan TTL yedi gün otomatik olarak uygular. Bu varsayılan TTL yalnızca genel web teslim iyileştirmeler için geçerlidir. Büyük dosya en iyi duruma getirme, varsayılan TTL bir gündür ve en iyi duruma getirme akış medya için TTL bir yıl varsayılandır.
> 
> BLOB'ları ve diğer dosyalara erişimi hızlandırmak için Azure CDN nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure içerik teslim ağı'ne genel bakış](cdn-overview.md).
> 
> Azure Blob Depolama hakkında daha fazla bilgi için bkz: [Blob Storage'a giriş](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).
 

## <a name="setting-cache-control-headers-by-using-cdn-caching-rules"></a>Cache-Control üstbilgileri kuralları önbelleğe alma CDN kullanarak ayarlama
Bir blob'un ayar için tercih edilen yöntem `Cache-Control` başlığıdır Azure portalında önbelleğe alma kurallarını kullanmak için. CDN kuralları önbelleğe alma hakkında daha fazla bilgi için bkz: [denetim Azure CDN kuralları önbelleğe alma ile önbelleğe alma davranışı](cdn-caching-rules.md).

> [!NOTE] 
> Önbelleğe alma kuralları yalnızca kullanılabilir **verizon'dan Azure CDN standart** ve **akamai'den Azure CDN standart** profilleri. İçin **verizon'dan Azure CDN Premium** profilleri kullanmalıdır [Azure CDN kurallar altyapısı](cdn-rules-engine.md) içinde **Yönet** benzer işlevselliği için portal.

**CDN önbelleğe alma Kurallar sayfasına gitmek için**:

1. Azure portalında bir CDN profili seçin, sonra blob için uç noktaya seçin.

2. Ayarları altındaki sol bölmede seçin **kuralları önbelleğe alma**.

   ![CDN önbelleğe alma kurallarını düğmesi](./media/cdn-manage-expiration-of-blob-content/cdn-caching-rules-btn.png)

   **Kuralları önbelleğe alma** sayfası görüntülenir.

   ![CDN önbelleğe alma sayfası](./media/cdn-manage-expiration-of-blob-content/cdn-caching-page.png)


**Genel önbelleğe alma kurallarını kullanarak bir Blob Depolama hizmetinin Cache-Control üstbilgileri ayarlamak için:**

1. Altında **genel kurallar önbelleğe alma**ayarlayın **sorgu dizesini önbelleğe alma davranışı** için **sorgu dizelerini yoksayabilir** ve **önbelleğe alma davranışı** için **Geçersiz kılma**.
      
2. İçin **önbelleğe sona erme süresi**, 3600 içinde girin **saniye** kutusu veya 1'de **saatleri** kutusu. 

   ![CDN genel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-blob-content/cdn-global-caching-rules-example.png)

   Bu genel önbellek kuralını önbellek süresi bir saat ayarlar ve uç nokta için tüm istekleri etkiler. Tüm geçersiz kılmaları `Cache-Control` veya `Expires` bitiş noktası tarafından belirtilen kaynak sunucu tarafından gönderilen HTTP üstbilgileri.   

3. **Kaydet**’i seçin.
 
**Bir blob özel önbelleğe alma kurallarını kullanarak dosyanın Cache-Control üstbilgileri ayarlamak için:**

1. Altında **özel kurallar önbelleğe alma**, iki eşleşme koşullar oluşturun:

     A. İlk eşleşme koşulu için **eşleşen koşulu** için **yolu** ve girin `/blobcontainer1/*` için **eşleşen değeri**. Ayarlama **önbelleğe alma davranışı** için **geçersiz kılma** ve 4'te girin **saatleri** kutusu.

    B. İkinci eşleşme koşulu için **eşleşen koşulu** için **yolu** ve girin `/blobcontainer1/blob1.txt` için **eşleşen değeri**. Ayarlama **önbelleğe alma davranışı** için **geçersiz kılma** ve 2'de girin **saatleri** kutusu.

    ![CDN özel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-blob-content/cdn-custom-caching-rules-example.png)

    İlk özel önbellek kuralını dört saat blob dosyalar için önbellek süresini ayarlar `/blobcontainer1` , bitiş noktası tarafından belirtilen kaynak sunucuda klasör. İlk kural için ikinci kuralı geçersiz kılar `blob1.txt` yalnızca blob dosyası ve bir önbellek süre için bu iki saate ayarlar.

2. **Kaydet**’i seçin.


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
Bir blob'un belirtmek için `Cache-Control` .NET kod, kullanım kullanarak üstbilgi [.NET için Azure Storage istemci Kitaplığı](../storage/blobs/storage-dotnet-how-to-use-blobs.md) ayarlamak için [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) özelliği.

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
> Daha fazla .NET kod örnekleri kullanılabilir [.NET için Azure Blob Depolama örnek](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 

## <a name="setting-cache-control-headers-by-using-other-methods"></a>Cache-Control üstbilgileri diğer yöntemleri kullanarak ayarlama

### <a name="azure-storage-explorer"></a>Azure Depolama Gezgini
İle [Azure Storage Gezgini](https://azure.microsoft.com/features/storage-explorer/), görüntüleyin ve özellikleri gibi dahil, blob storage kaynaklarını düzenleyin *CacheControl* özelliği. 

Güncelleştirilecek *CacheControl* Azure Depolama Gezgini ile bir blob özelliği:
   1. Bir blob seçin ve ardından **özellikleri** ve bağlam menüsünden. 
   2. Ekranı aşağı kaydırarak *CacheControl* özelliği.
   3. Bir değer girin ve ardından **kaydetmek**.


![Azure Storage Gezgini özellikleri](./media/cdn-manage-expiration-of-blob-content/cdn-storage-explorer-properties.png)

### <a name="azure-command-line-interface"></a>Azure Komut Satırı Arabirimi
İle [Azure komut satırı arabirimi](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) (CLI), komut satırından Azure blob kaynakları yönetebilir. Azure CLI ile bir blob karşıya yüklediğinizde cache-control üstbilgisinin ayarlamak için ayarlayın *cacheControl* kullanarak özellik `-p` geçin. Aşağıdaki örnekte, TTL bir saat (3600 saniye) ayarlamak gösterilmektedir:
  
```azurecli
azure storage blob upload -c <connectionstring> -p cacheControl="max-age=3600" .\<blob name> <container name> <blob name>
```

### <a name="azure-storage-services-rest-api"></a>Azure storage services REST API'si
Kullanabileceğiniz [Azure storage Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx) açıkça ayarlamak için *x-ms-blob-cache-control* istek üzerine aşağıdaki işlemleri kullanarak özelliği:
  
   - [BLOB yerleştirme](https://msdn.microsoft.com/library/azure/dd179451.aspx)
   - [Engelleme listesi yerleştirme](https://msdn.microsoft.com/library/azure/dd179467.aspx)
   - [Blob özelliklerini ayarlama](https://msdn.microsoft.com/library/azure/ee691966.aspx)

## <a name="testing-the-cache-control-header"></a>Cache-Control üstbilgisinin test etme
Bloblarınızın TTL ayarlarını kolayca doğrulayabilirsiniz. Tarayıcınızın ile [Geliştirici Araçları](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), blob içeren test `Cache-Control` yanıtı üstbilgisi. Bir aracı gibi kullanabilir [Wget](https://www.gnu.org/software/wget/), [Postman](https://www.getpostman.com/), veya [Fiddler](http://www.telerik.com/fiddler) yanıt üstbilgileri incelemek için.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure CDN içinde bulut hizmeti içeriğinin kullanım süresini yönetme öğrenin](cdn-manage-expiration-of-cloud-service-content.md)
* [Kavramları önbelleğe alma hakkında bilgi edinin](cdn-how-caching-works.md)

