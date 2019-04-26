---
title: Kapsayıcıları ve blobları Azure Blob Depolama için genel okuma erişimi etkinleştir | Microsoft Docs
description: Kapsayıcılara ve blob'lara anonim erişim için kullanılabilir hale getirme ve programlı olarak erişmek nasıl öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/26/2017
ms.author: tamram
ms.openlocfilehash: 3996f22db2f5dc597939995a2699c4fe228821e3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60392572"
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Kapsayıcılara ve blob’lara anonim okuma erişimini yönetme
Bir kapsayıcı ve bloblarını Azure Blob Depolama alanında anonim, genel okuma erişimi etkinleştirebilirsiniz. Bunu yaptığınızda, hesap anahtarınız paylaşımı ve paylaşılan erişim imzası (SAS) gerek olmadan bu kaynaklara salt okunur erişim verebilirsiniz.

Genel okuma erişimi belirli bloblar her zaman için anonim okuma erişimi kullanılabilir olmasını istediğiniz senaryolar için idealdir. Daha ayrıntılı denetim için paylaşılan erişim imzası oluşturabilirsiniz. Paylaşılan erişim imzaları, belirli bir süre için farklı izinlere kullanarak kısıtlı erişim sağlamanıza olanak sağlar. Paylaşılan oluşturma hakkında daha fazla bilgi için erişim imzaları, bkz: [kullanarak paylaşılan erişim imzaları (SAS) Azure Depolama'daki](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Kapsayıcılara ve blob'lara anonim kullanıcılara izin verin
Varsayılan olarak, bir kapsayıcı ve içerdiği tüm blobları yalnızca depolama hesabı sahibi tarafından erişilebilir. Anonim kullanıcılar için bir kapsayıcı ve bloblarını Okuma izinleri vermek için genel erişime izin vermek için kapsayıcı izinlerini ayarlayabilirsiniz. Anonim kullanıcılar, istek kimliği doğrulanmadan ortak olarak erişilebilen bir kapsayıcı içindeki blobları okuyabilir.

Şu izinlere sahip bir kapsayıcı yapılandırabilirsiniz:

* **Genel okuma erişimi yok:** Kapsayıcı ve bloblarını yalnızca depolama hesabı sahibi tarafından erişilebilir. Bu, tüm yeni kapsayıcılar için varsayılandır.
* **Yalnızca BLOB'lar için genel okuma erişimi:** Kapsayıcı içindeki blobları anonim istek tarafından okunabilir ancak kapsayıcı verileri mevcut değil. Kapsayıcı içindeki blobları anonim istemciler listelenemiyor.
* **Tam genel okuma erişimi:** Tüm kapsayıcı ve blob verilerini anonim istek tarafından okunabilir. İstemcileri, kapsayıcı içindeki blobları anonim istek göre sıralayabilirsiniz, ancak depolama hesabında kapsayıcıları numaralandırılamıyor.

Kapsayıcı izinlerini ayarlamak için şunları kullanabilirsiniz:

* [Azure portal](https://portal.azure.com)
* [Azure PowerShell](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure CLI](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* Programlı olarak bir depolama istemci kitaplıkları veya REST API'yi kullanarak

### <a name="set-container-permissions-in-the-azure-portal"></a>Azure portalında kapsayıcı izinleri ayarlama
Kapsayıcı izinlerini ayarlamak için [Azure portalında](https://portal.azure.com), şu adımları izleyin:

1. Açık, **depolama hesabı** portaldaki dikey pencere. Depolama hesabınızı seçerek bulabilirsiniz **depolama hesapları** ana portal menü dikey penceresinde.
1. Altında **BLOB hizmeti** menü dikey penceresinde seçin **Blobları**.
1. Kapsayıcı satıra sağ tıklayın veya kapsayıcının açmak için üç noktayı seçin **bağlam menüsü**.
1. Seçin **erişim ilkesi** bağlam menüsünde.
1. Seçin bir **erişim türü** açılan menüden.

    ![Kapsayıcı meta verileri iletişim Düzenle](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a>.NET ile kapsayıcı izinleri ayarlama
.NET için C# ve depolama istemci kitaplığı kullanarak kapsayıcı izinlerini ayarlamak için öncelikle kapsayıcının mevcut izinleri çağırarak alma **GetPermissions** yöntemi. Ardından **PublicAccess** özelliği **BlobContainerPermissions** tarafından döndürülen nesne **GetPermissions** yöntemi. Son olarak, çağrı **izinleri ayarla** güncelleştirilmiş izinler ile yöntemi.

Aşağıdaki örnek, tam genel okuma erişimini kapsayıcının izinleri ayarlar. Yalnızca BLOB'lar için genel okuma erişimini izinleri ayarlamak için ayarlanmış **PublicAccess** özelliğini **BlobContainerPublicAccessType.Blob**. Özelliğin anonim kullanıcılar için tüm izinleri kaldırmak için kümesine **BlobContainerPublicAccessType.Off**.

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Kapsayıcılara ve blob'lara anonim olarak erişim
Kapsayıcılara ve blob'lara anonim olarak erişen bir istemci kimlik bilgileri gerektirmeyen oluşturucular kullanabilirsiniz. Aşağıdaki örnekler, Blob hizmeti kaynaklarına anonim olarak başvurmak için birkaç farklı yolunu gösterir.

### <a name="create-an-anonymous-client-object"></a>Anonim istemci nesne oluşturma
Blob Hizmeti uç noktası için hesap sağlayarak anonim erişim için yeni bir hizmet istemci nesnesi oluşturabilirsiniz. Ancak, bu hesaptaki anonim erişim için kullanılabilir olan bir kapsayıcının adını bilmeniz gerekir.

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob service endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a>Anonim olarak bir kapsayıcı başvurusu
Anonim olarak kullanılabilir bir kapsayıcıya URL'yi varsa, kapsayıcıyı doğrudan başvurmak için kullanabilirsiniz.

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in the container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a>Anonim olarak bir blob başvurusu
Anonim erişim için kullanılabilir bir bloba URL varsa, bu URL'yi kullanarak doğrudan blob başvurabilirsiniz:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-to-anonymous-users"></a>Anonim kullanıcılar için kullanılabilir özellikler
Aşağıdaki tabloda gösterilen bir kapsayıcının ACL erişimine izin verecek şekilde ayarlandığında anonim kullanıcılar tarafından hangi işlemler çağrılabilir.

| İşlem REST | Tam genel okuma erişimi izni | Yalnızca BLOB'lar için genel okuma erişimi izni |
| --- | --- | --- |
| Liste kapsayıcıları |Yalnızca sahibi |Yalnızca sahibi |
| Kapsayıcı oluşturma |Yalnızca sahibi |Yalnızca sahibi |
| Kapsayıcı özellikleri alma |Tümü |Yalnızca sahibi |
| Kapsayıcı meta verilerini al |Tümü |Yalnızca sahibi |
| Kapsayıcı meta verileri ayarlama |Yalnızca sahibi |Yalnızca sahibi |
| Kapsayıcı ACL alın |Yalnızca sahibi |Yalnızca sahibi |
| Kapsayıcı ACL ayarlayın |Yalnızca sahibi |Yalnızca sahibi |
| Kapsayıcıyı Sil |Yalnızca sahibi |Yalnızca sahibi |
| Blobları Listele |Tümü |Yalnızca sahibi |
| Put Blob |Yalnızca sahibi |Yalnızca sahibi |
| Get Blob |Tümü |Tümü |
| BLOB özelliklerini alma |Tümü |Tümü |
| Blob özelliklerini ayarlama |Yalnızca sahibi |Yalnızca sahibi |
| Blob Meta Verilerini al |Tümü |Tümü |
| BLOB meta verileri ayarlama |Yalnızca sahibi |Yalnızca sahibi |
| Blok yerleştirme |Yalnızca sahibi |Yalnızca sahibi |
| Engelleme listesi (yalnızca kaydedilmiş blokları) alma |Tümü |Tümü |
| Engelleme listesi (yalnızca işlenmemiş blokları veya tüm blokları) alma |Yalnızca sahibi |Yalnızca sahibi |
| Engelleme listesi yerleştirin |Yalnızca sahibi |Yalnızca sahibi |
| BLOB silme |Yalnızca sahibi |Yalnızca sahibi |
| Kopya blob'u |Yalnızca sahibi |Yalnızca sahibi |
| Blob anlık görüntüsü |Yalnızca sahibi |Yalnızca sahibi |
| Kira blob'u |Yalnızca sahibi |Yalnızca sahibi |
| Sayfa yerleştirin |Yalnızca sahibi |Yalnızca sahibi |
| Alma sayfası aralıkları |Tümü |Tümü |
| Ekleme Blobu |Yalnızca sahibi |Yalnızca sahibi |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Storage Hizmetleri için kimlik doğrulaması](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [Paylaşılan erişim imzaları (SAS) kullanma](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Paylaşılan Erişim İmzası ile Erişim için Temsilci Seçme](https://msdn.microsoft.com/library/azure/ee395415.aspx)
