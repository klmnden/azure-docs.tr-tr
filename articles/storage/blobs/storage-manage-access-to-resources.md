---
title: Kapsayıcılar ve Azure Blob depolamada BLOB'lar için herkese okuma erişimi etkinleştir | Microsoft Docs
description: Kapsayıcılar ve bloblar anonim erişim için nasıl oluşturulacağı ve programlı olarak erişmek nasıl öğrenin.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 04/26/2017
ms.author: tamram
ms.openlocfilehash: 4ddafb095816b5be82a18faa9c60869094e5e4c6
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
ms.locfileid: "29557072"
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Kapsayıcılara ve blob’lara anonim okuma erişimini yönetme
Bir kapsayıcı ve bloblarını Azure Blob depolamada anonim, ortak okuma erişimi etkinleştirebilirsiniz. Bunu yaparak, hesap anahtarınızı paylaşımı ve paylaşılan erişim imzası (SAS) gerek olmadan bu kaynaklara salt okunur erişim verebilirsiniz.

Ortak okuma erişimi belirli BLOB'ları her zaman anonim okuma erişimi için kullanılabilir olmasını istediğiniz senaryolar için uygundur. Daha ayrıntılı denetim için bir paylaşılan erişim imzası oluşturabilirsiniz. Paylaşılan erişim imzalar, belirli bir süre için farklı izinleri kullanarak kısıtlı erişim sağlamak etkinleştirin. Paylaşılan oluşturma hakkında daha fazla bilgi için erişim imzalar, bkz: [kullanarak paylaşılan erişim imzaları (SAS) Azure storage'da](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Kapsayıcılar ve bloblar için anonim kullanıcı izinleri
Varsayılan olarak, bir kapsayıcı ve içindeki tüm BLOB'ları yalnızca depolama hesabı sahibi tarafından erişilebilir. Anonim kullanıcılar bir kapsayıcı ve bloblarını için Okuma izinleri vermek için genel erişime izin vermek için kapsayıcı izinlerini ayarlayabilirsiniz. Anonim kullanıcılar istek doğrulanmadan genel olarak erişilebilir bir kapsayıcıdaki blobları okuyabilir.

Şu izinlere sahip bir kapsayıcı yapılandırabilirsiniz:

* **Hiçbir public okuma erişimi:** kapsayıcı ve bloblarını yalnızca depolama hesabı sahibi tarafından erişilebilir. Bu, tüm yeni kapsayıcıları için varsayılan değerdir.
* **Genel erişim için BLOB'ları yalnızca okuma:** kapsayıcıdaki Blobları anonim istek tarafından okunabilir ancak kapsayıcı verileri kullanılabilir değil. Anonim istemcileri kapsayıcısı içinde BLOB'ları numaralandırılamıyor.
* **Tam herkese okuma erişimi:** tüm kapsayıcı ve blob verilerini anonim istek tarafından okunabilir. İstemcileri kapsayıcıdaki blobları anonim istek göre sıralayabilirsiniz, ancak depolama hesabı kapsayıcılara numaralandırılamıyor.

Kapsayıcı izinlerini ayarlamak için aşağıdakileri kullanın:

* [Azure portalı](https://portal.azure.com)
* [Azure PowerShell](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Azure CLI 2.0](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* Program aracılığıyla, depolama istemcisi kitaplıklarını veya REST API'yi birini kullanarak

### <a name="set-container-permissions-in-the-azure-portal"></a>Azure portalında kapsayıcı izinleri ayarlama
Kapsayıcı izinleri ayarlamak için [Azure portal](https://portal.azure.com), şu adımları izleyin:

1. Açık, **depolama hesabı** portaldaki dikey pencere. Depolama hesabınızı seçerek bulabileceğiniz **depolama hesapları** ana portal menü dikey penceresinde.
1. Altında **BLOB hizmeti** menü dikey penceresinde, seçin **kapsayıcıları**.
1. Kapsayıcının açmak için üç nokta seçin veya kapsayıcı satırındaki sağ **bağlam menüsü**.
1. Seçin **erişim ilkesi** bağlam menüsünde.
1. Seçin bir **erişim türüne** açılan menüden.

    ![Kapsayıcı meta verileri iletişim Düzenle](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a>.NET ile kapsayıcı izinleri ayarlama
C# ve depolama istemci kitaplığı için .NET kullanarak bir kapsayıcı izinlerini ayarlamak için ilk kapsayıcının varolan izinlerini çağırarak almaya **GetPermissions** yöntemi. Ardından **PublicAccess** özelliği için **BlobContainerPermissions** tarafından döndürülen nesne **GetPermissions** yöntemi. Son olarak, arama **izinleri ayarla** güncelleştirilmiş izinlerle yöntemi.

Aşağıdaki örnek kapsayıcının izinleri için tam ortak okuma erişimi ayarlar. Yalnızca BLOB'lar için herkese okuma erişimi izinlerini ayarlamak için ayarlanmış **PublicAccess** özelliğine **BlobContainerPublicAccessType.Blob**. Özelliğin anonim kullanıcılar için tüm izinleri kaldırmak için kümesine **BlobContainerPublicAccessType.Off**.

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Kapsayıcılar ve bloblar anonim erişim
Kapsayıcılar ve bloblar anonim olarak erişen istemci kimlik bilgileri gerektirmeyecek oluşturucular kullanabilir. Aşağıdaki örnekler Blob hizmeti kaynaklarını anonim olarak başvurmak için birkaç farklı yolu gösterilmektedir.

### <a name="create-an-anonymous-client-object"></a>Anonim istemci nesnesi oluşturun
Hesap için Blob Hizmeti uç noktası sağlayarak anonim erişim için yeni bir hizmet istemci nesnesi oluşturabilirsiniz. Ancak, bu hesaptaki anonim erişim için kullanılabilir bir kapsayıcının adını bilmeniz gerekir.

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
Anonim olarak kullanılabilir bir kapsayıcı URL'si varsa, kapsayıcı doğrudan başvurmak için kullanabilirsiniz.

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
Anonim erişim için kullanılabilir bir blob URL'si varsa, bu URL'yi kullanarak doğrudan blob başvurusu yapabilir:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-to-anonymous-users"></a>Anonim kullanıcılar için kullanılabilir özellikler
Bir kapsayıcının ACL genel erişime izin verecek şekilde ayarladığınızda, anonim kullanıcılar tarafından hangi işlemleri çağrılabilir aşağıdaki tabloda gösterilmektedir.

| REST işlemi | Tam ortak okuma erişimi izni | Yalnızca BLOB'lar için herkese okuma erişimi izniyle |
| --- | --- | --- |
| Liste kapsayıcıları |Yalnızca sahibi |Yalnızca sahibi |
| Kapsayıcı oluşturma |Yalnızca sahibi |Yalnızca sahibi |
| Kapsayıcı özellikleri Al |Tümü |Yalnızca sahibi |
| Kapsayıcı meta verileri alma |Tümü |Yalnızca sahibi |
| Kapsayıcı meta verileri ayarlama |Yalnızca sahibi |Yalnızca sahibi |
| Kapsayıcı ACL Al |Yalnızca sahibi |Yalnızca sahibi |
| Kapsayıcı ACL ayarlayın |Yalnızca sahibi |Yalnızca sahibi |
| Kapsayıcısını silmek |Yalnızca sahibi |Yalnızca sahibi |
| Liste BLOB'ları |Tümü |Yalnızca sahibi |
| Put Blob |Yalnızca sahibi |Yalnızca sahibi |
| BLOB alma |Tümü |Tümü |
| BLOB özelliklerini alma |Tümü |Tümü |
| Blob özelliklerini ayarlama |Yalnızca sahibi |Yalnızca sahibi |
| Get Blob Metadata |Tümü |Tümü |
| Set Blob Metadata |Yalnızca sahibi |Yalnızca sahibi |
| Blok yerleştirme |Yalnızca sahibi |Yalnızca sahibi |
| Engelleme listesi (yalnızca kaydedilmiş engeller) Al |Tümü |Tümü |
| Engelleme listesi (yalnızca kaydedilmemiş blokları veya tüm blokları) Al |Yalnızca sahibi |Yalnızca sahibi |
| Engelleme listesi yerleştirme |Yalnızca sahibi |Yalnızca sahibi |
| BLOB Sil |Yalnızca sahibi |Yalnızca sahibi |
| BLOB kopyalama |Yalnızca sahibi |Yalnızca sahibi |
| Snapshot Blob |Yalnızca sahibi |Yalnızca sahibi |
| Kira blob'u |Yalnızca sahibi |Yalnızca sahibi |
| Sayfa yerleştirin |Yalnızca sahibi |Yalnızca sahibi |
| Sayfa aralıklarını alma |Tümü |Tümü |
| BLOB ekleme |Yalnızca sahibi |Yalnızca sahibi |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Storage Hizmetleri için kimlik doğrulaması](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [Paylaşılan erişim imzaları (SAS) kullanma](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Paylaşılan Erişim İmzası ile Erişim için Temsilci Seçme](https://msdn.microsoft.com/library/azure/ee395415.aspx)
