---
title: BLOB ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (ASP.NET Core) | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabı oluşturduktan sonra Azure Blob Depolama içinde bir Visual Studio ASP.NET Core projesi ile çalışmaya başlamak nasıl bağlı hizmetler
services: storage
author: ghogen
manager: douge
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ghogen
ms.openlocfilehash: 388c4d5f28e87f5cfe26336771d30fa44c6f9ef0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62123017"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Azure Blob ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (ASP.NET Core)

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

Bu makalede oluşturduğunuz veya Visual Studio kullanarak bir ASP.NET Core projesi bir Azure depolama hesabında başvurulan sonra Visual Studio'da Azure Blob Depolama kullanmaya başlama işlemini açıklamaktadır **bağlı hizmetler** özelliği. **Bağlı hizmetler** işlemi projenizde Azure depolamaya erişmek için uygun NuGet paketlerini yükler ve proje yapılandırma dosyalarınızı depolama hesabı için bağlantı dizesi ekler. (Bkz [depolama belgeleri](https://azure.microsoft.com/documentation/services/storage/) Azure depolama hakkında genel bilgiler.)

Azure Blob Depolama, büyük miktarlarda herhangi bir HTTP veya HTTPS aracılığıyla dünyanın erişilebilen yapılandırılmamış verileri depolamak için kullanılan bir hizmettir. Bir blobun herhangi bir boyutta olabilir. Görüntüleri, ses ve video dosyaları, ham verileri ve belge dosyaları gibi blobları olabilir. Bu makale, Visual Studio kullanarak bir Azure depolama hesabı oluşturduktan sonra blob depolama ile çalışmaya başlama işlemini açıklamaktadır **bağlı hizmetler** bir ASP.NET Core projesi içinde.

Dosyaları klasörler halinde yalnızca canlı olarak kapsayıcılarda depolama BLOB'ları dinamik. Bir blob oluşturduktan sonra bir blobun bir veya daha fazla kapsayıcı oluşturun. Örneğin, "Koleksiyon defteri" adlı bir blob, resimleri depolamak için "Görüntü" adı verilen kapsayıcıları oluşturabilirsiniz ve başka "ses dosyaları depolamak için ses" denir. Kapsayıcıları oluşturduktan sonra bunları tek tek dosyaları karşıya yükleyebilirsiniz. Bkz: [hızlı başlangıç: Karşıya yükleme, indirme ve .NET kullanarak blobları listeleme](../storage/blobs/storage-quickstart-blobs-dotnet.md) program aracılığıyla BLOB'ları düzenleme hakkında daha fazla bilgi.

Bazı Azure depolama API'leri uyumsuzdur ve bu makalede kod zaman uyumsuz yöntemler kullanıldığını varsayar. Bkz: [zaman uyumsuz programlama](https://docs.microsoft.com/dotnet/csharp/async) daha fazla bilgi için.

## <a name="access-blob-containers-in-code"></a>Kod erişim blob kapsayıcıları

ASP.NET Core projeleri blob'larda programlı olarak erişmek için aşağıdaki kodu ekleyin Aksi halde, zaten mevcut gerekir:

1. Gerekli Ekle `using` ifadeleri:

    ```cs
    using Microsoft.Extensions.Configuration;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;
    using LogLevel = Microsoft.Extensions.Logging.LogLevel;
    ```

1. Alma bir `CloudStorageAccount` depolama hesap bilgilerini temsil eden nesne. Depolama bağlantı dizesi ve depolama hesabı bilgileri Azure hizmet yapılandırmasından almak için aşağıdaki kodu kullanın:

    ```cs
     CloudStorageAccount storageAccount = new CloudStorageAccount(
        new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
        "<storage-account-name>",
        "<access-key>"), true);
    ```

1. Kullanım bir `CloudBlobClient` nesne almak için bir `CloudBlobContainer` depolama hesabınızda var olan bir kapsayıcı başvurusu:

    ```cs
    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "mycontainer."
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");
    ```

## <a name="create-a-container-in-code"></a>Kod içinde bir kapsayıcı oluşturma

Ayrıca `CloudBlobClient` çağırarak, depolama hesabında bir kapsayıcı oluşturmak için `CreateIfNotExistsAsync`:

```cs
// Create a blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Get a reference to a container named "my-new-container."
CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

// If "mycontainer" doesn't exist, create it.
await container.CreateIfNotExistsAsync();
```

Kapsayıcı içindeki dosyaların herkese kullanılabilmesi için kapsayıcıyı genel olarak ayarlayın:

```cs
await container.SetPermissionsAsync(new BlobContainerPermissions
{
    PublicAccess = BlobContainerPublicAccessType.Blob
});
```

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme

Bir kapsayıcıya bir blob dosya karşıya yüklemek için bir kapsayıcı başvurusu alın ve bir blob başvurusu almak için kullanın. Ardından herhangi bir veri akışı çağırarak bu referansı karşıya `UploadFromStreamAsync` yöntemi. Bu işlem henüz ve mevcut bir bloba üzerine yazar blob oluşturur. 

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

Bir kapsayıcıdaki blobları listelemek için bir kapsayıcı başvurusu ilk Al'ı çağırın, `ListBlobsSegmentedAsync` blobları ve/veya dizinleri içine almak için yöntemi. Zengin özellik ve yöntem dönen erişmeye `IListBlobItem`, bunu bir `CloudBlockBlob`, `CloudPageBlob`, veya `CloudBlobDirectory` nesne. Blob türü bilmiyorsanız, hangi yayınlayacağınızı belirlemek için bir tür denetimi kullanın.

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

Bkz: [hızlı başlangıç: Karşıya yükleme, indirme ve .NET kullanarak blobları listeleme](../storage/blobs/storage-quickstart-blobs-dotnet.md#list-the-blobs-in-a-container) bir blob kapsayıcı içeriğini listelemek için kullanabileceğiniz diğer yöntemler için.

## <a name="download-a-blob"></a>Blob indirme

Bir blobu, blob başvuru ilk get indirmek için ardından çağırın `DownloadToStreamAsync` yöntemi. Aşağıdaki örnekte `DownloadToStreamAsync` blob içeriklerini ardından yerel dosya olarak kaydedebilirsiniz bir akış nesnesine aktarmak için yöntemi.

```cs
// Get a reference to a blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save the blob contents to a file named "myfile".
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    await blockBlob.DownloadToStreamAsync(fileStream);
}
```

Bkz: [hızlı başlangıç: Karşıya yükleme, indirme ve .NET kullanarak blobları listeleme](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs) diğer yollarını blobları dosyaları olarak kaydedin.

## <a name="delete-a-blob"></a>Blob silme

Bir blobu, blob başvuru ilk get silmek için'ı çağırın `DeleteAsync` yöntemi:

```cs
// Get a reference to a blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete the blob.
await blockBlob.DeleteAsync();
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
