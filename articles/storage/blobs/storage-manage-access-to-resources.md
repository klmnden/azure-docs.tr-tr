---
title: Kapsayıcıları ve blobları Azure Blob Depolama için genel okuma erişimi etkinleştir | Microsoft Docs
description: Kapsayıcılara ve blob'lara anonim erişim için kullanılabilir hale getirme ve programlı olarak erişmek nasıl öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/30/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.openlocfilehash: e0f93b0a95a228b26fae266129aea4b595b05e0f
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148371"
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Kapsayıcılara ve blob’lara anonim okuma erişimini yönetme

Bir kapsayıcı ve bloblarını Azure Blob Depolama alanında anonim, genel okuma erişimi etkinleştirebilirsiniz. Bunu yaptığınızda, hesap anahtarınız paylaşımı ve paylaşılan erişim imzası (SAS) gerek olmadan bu kaynaklara salt okunur erişim verebilirsiniz.

Genel okuma erişimi belirli bloblar her zaman için anonim okuma erişimi kullanılabilir olmasını istediğiniz senaryolar için idealdir. Daha ayrıntılı denetim için paylaşılan erişim imzası oluşturabilirsiniz. Paylaşılan erişim imzaları, belirli bir süre için farklı izinlere kullanarak kısıtlı erişim sağlamanıza olanak sağlar. Paylaşılan oluşturma hakkında daha fazla bilgi için erişim imzaları, bkz: [kullanarak paylaşılan erişim imzaları (SAS) Azure Depolama'daki](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Kapsayıcılara ve blob'lara anonim kullanıcılara izin verin

Varsayılan olarak, bir kapsayıcı ve BLOB'ları içindeki uygun izinleri verilen yalnızca bir kullanıcı tarafından erişilebilir. Bir kapsayıcı ve bloblarını anonim kullanıcılara Okuma erişimi vermek için kapsayıcı genel erişim düzeyi ayarlayabilirsiniz. Ardından bir kapsayıcıya genel erişim verdiğinizde, anonim kullanıcılar istek yetkilendirme olmadan genel olarak erişilebilen bir kapsayıcı içindeki blobları okuyabilirsiniz.

Şu izinlere sahip bir kapsayıcı yapılandırabilirsiniz:

* **Genel okuma erişimi yok:** Kapsayıcı ve bloblarını yalnızca depolama hesabı sahibi tarafından erişilebilir. Bu, tüm yeni kapsayıcılar için varsayılandır.
* **Yalnızca BLOB'lar için genel okuma erişimi:** Kapsayıcı içindeki blobları anonim istek tarafından okunabilir ancak kapsayıcı verileri mevcut değil. Kapsayıcı içindeki blobları anonim istemciler listelenemiyor.
* **Genel okuma erişimi kapsayıcı ve bloblarını için:** Tüm kapsayıcı ve blob verilerini anonim istek tarafından okunabilir. İstemcileri, kapsayıcı içindeki blobları anonim istek göre sıralayabilirsiniz, ancak depolama hesabında kapsayıcıları numaralandırılamıyor.

Kapsayıcı izinlerini ayarlamak için şunları kullanabilirsiniz:

* [Azure portal](https://portal.azure.com)
* [Azure PowerShell](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure CLI](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* Programlı olarak bir depolama istemci kitaplıkları veya REST API'yi kullanarak

### <a name="set-container-public-access-level-in-the-azure-portal"></a>Azure portalında kapsayıcı genel erişim düzeyini ayarlayın

Gelen [Azure portalında](https://portal.azure.com), bir veya daha fazla kapsayıcılar için genel erişim düzeyi güncelleştirebilirsiniz:

1. Azure portalda depolama hesabınıza gidin.
1. Altında **Blob hizmeti** menü dikey penceresinde seçin **Blobları**.
1. Genel erişim düzeyi ayarlamak istediğiniz kapsayıcılar'ı seçin.
1. Kullanım **erişim düzeyini Değiştir** genel erişim ayarlarını görüntülemek için düğme.
1. İstenen genel erişim düzeyi seçin **genel erişim düzeyi** açılır ve seçili kapsayıcıları için değişikliği uygulamak için Tamam düğmesine tıklayın.

Aşağıdaki ekran görüntüsünde, seçili kapsayıcıları için genel erişim düzeyini değiştirme işlemi gösterilmektedir.

![Portalda genel erişim düzeyi ayarlama işlemini gösteren ekran görüntüsü](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

> [!NOTE]
> Tek bir blob için genel erişim düzeyi değiştirilemez. Genel erişim düzeyi yalnızca kapsayıcı düzeyinde ayarlanır.

### <a name="set-container-public-access-level-with-net"></a>.NET ile kapsayıcısı genel erişim düzeyini ayarlayın

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

Kapsayıcılara ve blob'lara anonim olarak erişen bir istemci kimlik bilgileri gerektirmeyen oluşturucular kullanabilirsiniz. Aşağıdaki örnekler, kapsayıcılara ve blob'lara anonim olarak başvurmak için birkaç farklı yolunu gösterir.

### <a name="create-an-anonymous-client-object"></a>Anonim istemci nesne oluşturma

Hesabı için Blob Depolama uç sağlayarak anonim erişim için yeni bir hizmet istemci nesnesi oluşturabilirsiniz. Ancak, bu hesaptaki anonim erişim için kullanılabilir olan bir kapsayıcının adını bilmeniz gerekir.

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob storage endpoint.
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

Aşağıdaki tabloda gösterilen bir kapsayıcı yapılandırıldığında hangi işlemlerin anonim olarak genel erişim için çağrılabilir.

| İşlem REST | Kapsayıcı için genel okuma erişimi | Yalnızca bloblara genel okuma erişimi |
| --- | --- | --- |
| Liste kapsayıcıları | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Kapsayıcı oluşturma | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Kapsayıcı özellikleri alma | Anonim isteklere izin | Yalnızca yetkili istekleri |
| Kapsayıcı meta verilerini al | Anonim isteklere izin | Yalnızca yetkili istekleri |
| Kapsayıcı meta verileri ayarlama | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Kapsayıcı ACL alın | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Kapsayıcı ACL ayarlayın | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Kapsayıcıyı Sil | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Blobları Listele | Anonim isteklere izin | Yalnızca yetkili istekleri |
| Put Blob | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Get Blob | Anonim isteklere izin | Anonim isteklere izin |
| BLOB özelliklerini alma | Anonim isteklere izin | Anonim isteklere izin |
| Blob özelliklerini ayarlama | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Blob Meta Verilerini al | Anonim isteklere izin | Anonim isteklere izin |
| BLOB meta verileri ayarlama | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Blok yerleştirme | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Engelleme listesi (yalnızca kaydedilmiş blokları) alma | Anonim isteklere izin | Anonim isteklere izin |
| Engelleme listesi (yalnızca işlenmemiş blokları veya tüm blokları) alma | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Engelleme listesi yerleştirin | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| BLOB silme | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Kopya blob'u | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Blob anlık görüntüsü | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Kira blob'u | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Sayfa yerleştirin | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |
| Alma sayfası aralıkları | Anonim isteklere izin | Anonim isteklere izin |
| Ekleme Blobu | Yalnızca yetkili istekleri | Yalnızca yetkili istekleri |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Storage Hizmetleri için yetkilendirme](https://docs.microsoft.com/rest/api/storageservices/authorization-for-the-azure-storage-services)
* [Paylaşılan erişim imzaları (SAS) kullanma](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)