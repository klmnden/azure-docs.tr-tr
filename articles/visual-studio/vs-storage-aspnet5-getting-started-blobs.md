---
title: "BLOB ile çalışmaya başlama depolama ve Visual Studio bağlı Hizmetleri (ASP.NET Core) | Microsoft Docs"
description: "Visual Studio kullanarak bir depolama hesabı oluşturduktan sonra Azure Blob Depolama Visual Studio ASP.NET Core projede kullanmaya başlamak nasıl bağlı Hizmetleri"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 2e8060b44c395ad7c24e7778c0ef65148a3a45de
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Azure Blob ile çalışmaya başlama depolama ve Visual Studio bağlı Hizmetleri (ASP.NET çekirdek)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, oluşturduğunuz veya Visual Studio bağlı Hizmetleri Ekle iletişim kutusunu kullanarak bir ASP.NET Core projesini bir Azure depolama hesabında başvurulan sonra Visual Studio'da Azure Blob storage ile çalışmaya başlamak açıklar.

Azure Blob Depolama büyük miktarda gelen yerden HTTP veya HTTPS aracılığıyla erişilebilir yapılandırılmamış veriyi depolamak için bir hizmettir. Tek bir blob herhangi bir boyutta olabilir. BLOB'ları görüntüleri, ses ve video dosyaları, ham verileri ve belge dosyaları gibi olabilir. Bu makalede, Visual Studio kullanarak Azure storage hesabı oluşturduktan sonra blob storage'ı kullanmaya başlamak açıklar **bağlı Hizmetleri Ekle** ASP.NET Core projesinde iletişim.

Dosyaları klasörlerde yalnızca dinamik olarak kapsayıcılarında depolama BLOB'ları dinamik. Bir depolama oluşturduktan sonra depolama alanında bir veya daha fazla kapsayıcı oluşturun. Örneğin, "Koleksiyon defteri" adlı bir depolama resimleri depolamak için "görüntüleri" adlı depolama kapsayıcıları oluşturabilirsiniz ve başka bir "ses dosyalarını depolamak için ses" olarak adlandırılan. Kapsayıcılar oluşturduktan sonra bunları tek tek blob dosyaları karşıya yükleyebilir. Bkz: [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md) program aracılığıyla BLOB'lar düzenleme hakkında daha fazla bilgi.

## <a name="access-blob-containers-in-code"></a>Kod erişim blob kapsayıcıları
Zaten mevcut değillerse ASP.NET Core projelerinde BLOB'lar programlı olarak erişmek için aşağıdaki öğeleri eklemeniz gerekir.

1. Aşağıdaki kod ad alanı bildirimleri Azure depolama programlı olarak erişmek istediğiniz tüm C# dosyasının üstüne ekleyin.
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. Alma bir **CloudStorageAccount** depolama hesabı bilgilerini temsil eden nesne. Azure hizmet yapılandırmasından depolama bağlantı dizesi ve depolama hesabı bilgilerini almak için aşağıdaki kodu kullanın.
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    **Not:** tüm kod önünde Yukarıdaki kod aşağıdaki bölümleri kullanın.
3. Kullanım bir **CloudBlobClient** nesnesi bir **CloudBlobContainer** depolama hesabınızdaki var olan bir kapsayıcı başvurusu.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference to a container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a>Kod içinde bir kapsayıcı oluşturma
Aynı zamanda **CloudBlobClient** depolama hesabınızdaki bir kapsayıcı oluşturmak için. Tüm yapmanız gereken bir çağrı ekleyin etmektir **CreateIfNotExistsAsync** aşağıdaki kodu olduğu gibi:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


**Not:** ASP.NET Core Azure depolama çağrıları gerçekleştirdiğinizde API'leri zaman uyumsuzdur. Bkz: [uyumsuz ve bekleme ile zaman uyumsuz programlama](http://msdn.microsoft.com/library/hh191443.aspx) daha fazla bilgi için. Aşağıdaki kodu, zaman uyumsuz programlama yöntemleri kullanıldığı varsayılmaktadır.

Kapsayıcı içindeki dosyaların herkese kullanılabilmesi için genel olarak aşağıdaki kodu kullanarak kapsayıcıyı ayarlayabilirsiniz.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Bir kapsayıcıya bir blob dosya karşıya yükleme için bir kapsayıcı başvurusu alın ve bir blob başvurusu almak için kullanın. Bir blob başvurusu aldıktan sonra veri kendisine çağırarak karşıya yükleyebilirsiniz **UploadFromStreamAsync** yöntemi. Bu işlem henüz veya mevcut değilse bu raporun üzerine blob oluşturur. Aşağıdaki örnek kapsayıcının önceden oluşturulduğunu varsayarak bir blobun bir kapsayıcıya nasıl yükleneceğini gösterir.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme
Blob’ları bir kapsayıcıda listelemek için ilk olarak bir kapsayıcı başvurusu edinin. Ardından kapsayıcının çağırabilirsiniz **ListBlobsSegmentedAsync** blobları ve/veya dizinleri içindeki alma yöntemi. Dönen **IListBlobItem** için zengin özellik ve yöntem kümesine erişmek için **CloudBlockBlob**, **CloudPageBlob** veya **CloudBlobDirectory** nesnesine yayınlamanız gerekir. Blob türü bilmiyorsanız, hangisine yayınlayacağınızı belirlemek için bir tür denetimi kullanabilirsiniz. Aşağıdaki kod, almak ve her bir kapsayıcı öğe URI'sini çıkış gösterilmiştir.

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

Diğer bir blob kapsayıcı içeriğini listele yolu vardır. Bkz: [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) daha fazla bilgi için.

## <a name="download-a-blob"></a>Blob indirme
Bir blob indirmek için ilk blob başvurusu alın ve ardından arama **DownloadToStreamAsync** yöntemi. Aşağıdaki örnek kullanır **DownloadToStreamAsync** blob içeriklerini sonra yerel bir dosya olarak kaydedin bir akış nesnesine aktarmak için yöntem.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Blobları dosya olarak kaydetmek için başka yolları vardır. Bkz: [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) daha fazla bilgi için.

## <a name="delete-a-blob"></a>Blob silme
Bir blobu silmek için önce bir blob başvurusu alın ve ardından çağırın **DeleteAsync** yöntemini.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

