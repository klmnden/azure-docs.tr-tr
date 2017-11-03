---
title: "Azure CDN Azure Storage bloblarında sona erme tarihi yönetme | Microsoft Docs"
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
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d4741921806e443d92c385a04b781cec296c2ae8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-expiration-of-azure-storage-blobs-in-azure-cdn"></a>Azure CDN Azure Storage bloblarında, bitiş tarihini Yönet
> [!div class="op_single_selector"]
> * [Azure Web Apps/bulut Hizmetleri, ASP.NET ve IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure depolama blob hizmeti](cdn-manage-expiration-of-blob-content.md)
> 
> 

[Blob hizmeti](../storage/common/storage-introduction.md#blob-storage) içinde [Azure Storage](../storage/common/storage-introduction.md) birkaç Azure tabanlı kaynakları birini Azure CDN ile tümleşiktir.  Kendi yaşam süresi (TTL) geçen kadar herhangi bir genel olarak erişilebilir blob içerik Azure CDN'de önbelleğe alınabilir.  TTL değeri tarafından belirlenir [ *Cache-Control* üstbilgi](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) Azure depolama biriminden HTTP yanıt.

> [!TIP]
> Blob üzerindeki hiçbir TTL ayarlamak tercih edebilirsiniz.  Bu durumda, Azure CDN varsayılan TTL yedi gün otomatik olarak uygular.
> 
> BLOB'ları ve diğer dosyalara erişimi hızlandırmak için Azure CDN nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure CDN'ye genel bakış](cdn-overview.md).
> 
> Azure Storage blobu hizmeti hakkında daha fazla bilgi için bkz: [Blob hizmeti kavramları](https://msdn.microsoft.com/library/dd179376.aspx). 
> 
> 

Bu öğretici, Azure storage'da bir blob TTL ayarlayabilirsiniz birkaç yol gösterir.  

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) Azure hizmetlerinizi yönetmeyi hızlı ve en güçlü yollarından biri.  Kullanım `Get-AzureStorageBlob` blob başvurusu almak için cmdlet ayarlamaktır `.ICloudBlob.Properties.CacheControl` özelliği. 

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

## <a name="azure-storage-client-library-for-net"></a>.NET için Depolama İstemci Kitaplığı
.NET kullanarak bir blob'un TTL ayarlamak için kullanın [.NET için Azure Storage istemci Kitaplığı](../storage/blobs/storage-dotnet-how-to-use-blobs.md) ayarlamak için [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) özelliği.

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
> Daha fazla sayıda .NET kod örnekleri kullanılabilir [.NET için Azure Blob Depolama örnek](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).
> 
> 

## <a name="other-methods"></a>Diğer yöntemleri
* [Azure Komut Satırı Arabirimi](../cli-install-nodejs.md)
  
    Blob karşıya yüklenirken ayarlamak *cacheControl* özelliğini kullanarak `-p` geçin.  Bu örnekte, TTL bir saat (3600 saniye) ayarlar.
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [Azure Depolama Hizmetleri REST API'si](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    Açık olarak ayarlanıp *x-ms-blob-cache-control* özelliği bir [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put engelleme listesi](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx), veya [Blob özelliklerini ayarla](https://msdn.microsoft.com/library/azure/ee691966.aspx) isteği.
* Üçüncü taraf Depolama Yönetimi Araçları
  
    Bazı üçüncü taraf Azure Depolama Yönetimi araçlarını ayarlamanıza izin *CacheControl* BLOB'lar özelliği. 

## <a name="testing-the-cache-control-header"></a>Sınama *Cache-Control* üstbilgisi
Bloblarınızın TTL kolayca doğrulayabilirsiniz.  Tarayıcınızın kullanarak [Geliştirici Araçları](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), blob de dahil olmak üzere test *Cache-Control* yanıtı üstbilgisi.  Gibi bir araç de kullanabilirsiniz **wget**, [Postman](https://www.getpostman.com/), veya [Fiddler](http://www.telerik.com/fiddler) yanıt üstbilgileri incelemek için.

## <a name="next-steps"></a>Sonraki Adımlar
* [Hakkında bilgi edinin *Cache-Control* üstbilgisi](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Azure CDN içinde bulut hizmeti içeriğinin kullanım süresini yönetme öğrenin](cdn-manage-expiration-of-cloud-service-content.md)

