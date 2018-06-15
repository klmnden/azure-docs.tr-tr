---
title: BLOB ile çalışmaya başlama depolama ve Visual Studio bağlı Hizmetleri (ASP.NET Core) | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabı oluşturduktan sonra Azure Blob Depolama Visual Studio ASP.NET Core projede kullanmaya başlamak nasıl bağlı Hizmetleri
services: storage
author: ghogen
manager: douge
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ghogen
ms.openlocfilehash: 566e2edc0157ccd02e0b44ae7df86c4b484858b0
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31793962"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Azure Blob ile çalışmaya başlama depolama ve Visual Studio bağlı Hizmetleri (ASP.NET çekirdek)

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

Bu makale, oluşturduğunuz veya Visual Studio kullanarak bir ASP.NET Core projesini bir Azure depolama hesabında başvurulan sonra Visual Studio'da Azure Blob storage ile çalışmaya başlamak açıklamaktadır **bağlantılı Hizmetler** özelliği. **Bağlantılı Hizmetler** işlemi Azure depolama projenize erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler. (Bkz [Storage belgeleri](https://azure.microsoft.com/documentation/services/storage/) Azure Storage hakkında genel bilgi için.)

Azure Blob Depolama büyük miktarda gelen yerden HTTP veya HTTPS aracılığıyla erişilebilir yapılandırılmamış veriyi depolamak için bir hizmettir. Tek bir blob herhangi bir boyutta olabilir. BLOB'ları görüntüleri, ses ve video dosyaları, ham verileri ve belge dosyaları gibi olabilir. Bu makalede, Visual Studio kullanarak Azure storage hesabı oluşturduktan sonra blob storage'ı kullanmaya başlamak açıklar **bağlantılı Hizmetler** ASP.NET Core projesinde.

Dosyaları klasörlerde yalnızca dinamik olarak kapsayıcılarında depolama BLOB'ları dinamik. Bir blob oluşturduktan sonra bir veya daha fazla kapsayıcı, blob oluşturun. Örneğin, "Koleksiyon defteri" adlı bir blob resimleri depolamak için "görüntüleri" olarak adlandırılan kapsayıcılar oluşturabilirsiniz ve başka "ses dosyalarını depolamak için ses" denir. Kapsayıcılar oluşturduktan sonra bunları tek tek dosyaları karşıya yükleyebilir. Bkz: [hızlı başlangıç: karşıya yükleme, indirme ve .NET kullanarak listesi BLOB'ları](../storage/blobs/storage-quickstart-blobs-dotnet.md) program aracılığıyla BLOB'lar düzenleme hakkında daha fazla bilgi.

Bazı Azure depolama API'leri zaman uyumsuz ve zaman uyumsuz yöntemleri kullanıldığı bu makaledeki kod varsayar. Bkz: [zaman uyumsuz programlama](https://docs.microsoft.com/dotnet/csharp/async) daha fazla bilgi için.

## <a name="access-blob-containers-in-code"></a>Kod erişim blob kapsayıcıları

BLOB'ları ASP.NET Core projelerinde programlı olarak erişmek için aşağıdaki kodu ekleyin Aksi takdirde zaten var aktarmanız gerekir:

1. Gerekli eklemek `using` deyimleri:

    ```cs
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Extensions.Logging.LogLevel;
    ```

1. Alma bir `CloudStorageAccount` depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın:

    ```cs
     CloudStorageAccount storageAccount = new CloudStorageAccount(
        new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
        "<storage-account-name>",
        "<access-key>"), true);
    ```

1. Kullanım bir `CloudBlobClient` nesnesi bir `CloudBlobContainer` depolama hesabınızdaki var olan bir kapsayıcı başvurusu:

    ```cs
    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "mycontainer."
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");
    ```

## <a name="create-a-container-in-code"></a>Kod içinde bir kapsayıcı oluşturma

Aynı zamanda `CloudBlobClient` çağırarak depolama hesabınızdaki bir kapsayıcı oluşturmak için `CreateIfNotExistsAsync`:

```cs
// Create a blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Get a reference to a container named "my-new-container."
CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

// If "mycontainer" doesn't exist, create it.
await container.CreateIfNotExistsAsync();
```

Kapsayıcı içindeki dosyaların herkese kullanılabilmesi için herkese açık olmasını kapsayıcıya ayarlayın:

```cs
await container.SetPermissionsAsync(new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
});
```

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme

Bir kapsayıcıya bir blob dosya karşıya yükleme için bir kapsayıcı başvurusu alın ve bir blob başvurusu almak için kullanın. Çağırarak bu veri, başvuru karşıya `UploadFromStreamAsync` yöntemi. Bu işlem henüz ve bir blob üzerine yazar blob oluşturur. 

```cs
// Get a reference to a blob named "myblob".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

// Create or overwrite the "myblob" blob with the contents of a local file
// named "myfile".
using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
{
    await blockBlob.UploadFromStreamAsync(fileStream);
}
```

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

BLOB'ları bir kapsayıcıda listelemek için ilk get bir kapsayıcı Başvurusu'ı çağırın kendi `ListBlobsSegmentedAsync` blobları ve/veya dizinleri içindeki alma yöntemi. Zengin özellik ve yöntem erişmek için `IListBlobItem`, hangisine bir `CloudBlockBlob`, `CloudPageBlob`, veya `CloudBlobDirectory` nesnesi. Blob türü bilmiyorsanız, hangisine yayınlayacağınızı belirlemek için bir tür denetimi kullanın.

```cs
BlobContinuationToken token = null;
do
{
    BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
    token = resultSegment.ContinuationToken;

    foreach (IListBlobItem item in resultSegment.Results)
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);
        }

        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);
        }

        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }
} while (token != null);
```

Bkz: [hızlı başlangıç: karşıya yükleme, indirme ve .NET kullanarak listesi BLOB'ları](../storage/blobs/storage-quickstart-blobs-dotnet.md#list-the-blobs-in-a-container) bir blob kapsayıcı içeriğini listelemek diğer yolları için.

## <a name="download-a-blob"></a>Blob indirme

Bir blob, blob başvuru ilk get indirmek için ' ı çağırın `DownloadToStreamAsync` yöntemi. Aşağıdaki örnek kullanır `DownloadToStreamAsync` blob içeriklerini sonra yerel bir dosya olarak kaydedin bir akış nesnesine aktarmak için yöntem.

```cs
// Get a reference to a blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save the blob contents to a file named "myfile".
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    await blockBlob.DownloadToStreamAsync(fileStream);
}
```

Bkz: [hızlı başlangıç: karşıya yükleme, indirme ve .NET kullanarak listesi blob'lara](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs) blobları dosya olarak kaydetmek diğer yolları için.

## <a name="delete-a-blob"></a>Blob silme

Bir blob, blob başvuru ilk get silmek için ' ı çağırın `DeleteAsync` yöntemi:

```cs
// Get a reference to a blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete the blob.
await blockBlob.DeleteAsync();
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
