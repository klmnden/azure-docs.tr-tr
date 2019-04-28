---
title: BLOB ile çalışmaya başlama depolama ve Visual Studio bağlı hizmetler (bulut Hizmetleri) | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabına bağlandıktan sonra bir bulut hizmeti projesini Visual Studio'da, Azure Blob Depolama ile çalışmaya başlamak nasıl bağlı hizmetler
services: storage
author: ghogen
manager: douge
ms.assetid: 1144a958-f75a-4466-bb21-320b7ae8f304
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 9f1ef06e0275954343c548d0f6937b9c6fbcfd18
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122949"
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure Blob Depolama ve Visual Studio ile çalışmaya başlama bağlı hizmetler (bulut hizmeti projeleri)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede oluşturduğunuz veya Visual Studio kullanarak bir Azure depolama hesabı başvurulan sonra Azure Blob depolamayı kullanmaya başlama işlemini açıklamaktadır **bağlı hizmet Ekle** iletişim kutusunda Visual Studio cloud services projesi. Nasıl erişmek ve blob kapsayıcıları oluşturma ve karşıya yükleme, listeleme ve blobları indirme gibi yaygın görevlerin nasıl gerçekleştirileceğini göstereceğiz. Örnekler C dilinde yazılan\# ve [.NET için Microsoft Azure depolama istemci Kitaplığı](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Azure Blob Depolama, büyük miktarlarda herhangi bir HTTP veya HTTPS aracılığıyla dünyanın erişilebilen yapılandırılmamış verileri depolamak için kullanılan bir hizmettir. Bir blobun herhangi bir boyutta olabilir. Görüntüleri, ses ve video dosyaları, ham verileri ve belge dosyaları gibi blobları olabilir.

Dosyaları klasörler halinde yalnızca canlı olarak kapsayıcılarda depolama BLOB'ları dinamik. Bir depolama oluşturduktan sonra depolamada bir veya daha fazla kapsayıcı oluşturun. Örneğin, "Koleksiyon defteri" adlı bir depolama, resimleri depolamak için "Görüntü" adlı depolama kapsayıcıları oluşturabilirsiniz ve "ses dosyaları depolamak için ses" adlı bir tane. Kapsayıcıları oluşturduktan sonra bunları tek tek blob dosyalarını karşıya yükleyebilirsiniz.

* Program aracılığıyla BLOB'ları düzenleme hakkında daha fazla bilgi için bkz: [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../storage/blobs/storage-dotnet-how-to-use-blobs.md).
* Azure depolama hakkında genel bilgi için bkz. [depolama belgeleri](https://azure.microsoft.com/documentation/services/storage/).
* Azure bulut hizmetleri hakkında genel bilgi için bkz. [bulut Hizmetleri belgeleri](https://azure.microsoft.com/documentation/services/cloud-services/).
* ASP.NET uygulamalarını programlama hakkında daha fazla bilgi için bkz. [ASP.NET](https://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Kod erişim blob kapsayıcıları
Zaten mevcut değilse bulut hizmeti projeleri blob'larda programlı olarak erişmek için aşağıdaki öğeler eklemeniz gerekir.

1. Aşağıdaki kod ad alanı bildirimi programlı bir şekilde Azure Depolama'ya erişmek istediğiniz tüm C# dosyasının en üstüne ekleyin.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Alma bir **CloudStorageAccount** depolama hesap bilgilerini temsil eden nesne. Almak için aşağıdaki kodu kullanın. Azure hizmet yapılandırma depolama hesabı bilgilerini ve depolama bağlantı dizesi.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));
3. Alma bir **CloudBlobClient** depolama hesabınızda var olan bir kapsayıcı başvurmak için nesne.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
4. Alma bir **CloudBlobContainer** belirli bir blob kapsayıcısı başvurmak için nesne.
   
        // Get a reference to a container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [!NOTE]
> Aşağıdaki bölümlerde gösterilen kod önünde önceki yordamda gösterilen tüm kodları kullanın.
> 
> 

## <a name="create-a-container-in-code"></a>Kod içinde bir kapsayıcı oluşturma
> [!NOTE]
> ASP.NET'te Azure Storage'a göz çağrıları gerçekleştirmek bazı API'leri zaman uyumsuzdur. Bkz: [Async ve Await ile zaman uyumsuz programlama](https://msdn.microsoft.com/library/hh191443.aspx) daha fazla bilgi için. Aşağıdaki örnek kodda, zaman uyumsuz programlama yöntemler kullandığınızı varsayar.
> 
> 

Depolama hesabınızdaki bir kapsayıcı oluşturmak için yapmanız gereken tek şey bir çağrı ekleyin **Createıfnotexistsasync** şu kod gibi:

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


Kapsayıcı içindeki dosyaların herkese kullanılabilmesi için genel olarak aşağıdaki kodu kullanarak kapsayıcıyı ayarlayabilirsiniz.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Internet'teki herkes ortak bir kapsayıcıdaki blobları görebilir ancak değiştirdiğinizde ya da yalnızca uygun erişim anahtarınız varsa bunları silin.

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Azure Storage blok blobları ve sayfa bloblarını destekler. Çoğu durumda kullanılması önerilen blob türü blok blobudur.

Bir dosyayı bir blok blobuna yüklemek için bir kapsayıcı başvurusu alın ve blok blob başvurusu almak için kullanın. Bir blob başvurusu edindiğinizde **UploadFromStream** yöntemini çağırarak istediğiniz veri akışını yükleyebilirsiniz. Bu işlemle, eğer önceden oluşturulmadıysa bir blob oluşturulacaktır, aksi takdirde üzerine yazılacaktır. Aşağıdaki örnek kapsayıcının önceden oluşturulduğunu varsayarak bir blobun bir kapsayıcıya nasıl yükleneceğini gösterir.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme
Blob’ları bir kapsayıcıda listelemek için ilk olarak bir kapsayıcı başvurusu edinin. Ardından içindeki blobları ve/veya dizinleri almak için kapsayıcının **ListBlobs** yöntemini kullanabilirsiniz. Zengin özellik ve yöntem dönen erişmeye **Ilistblobıtem**, kendisine dönüştürmelisiniz bir **CloudBlockBlob**, **CloudPageBlob**, veya  **CloudBlobDirectory** nesne. Tür bilinmiyorsa, hangisine yayınlayacağınızı belirlemek için bir tür denetimi kullanabilirsiniz. Aşağıdaki kod, **resimler** kapsayıcısındaki her nesnenin URI’sinin nasıl alınacağını ve çıkacağını gösterir:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
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

Önceki kod örneğinde gösterildiği gibi blob hizmeti kapsayıcıları da içindeki dizinlerin kavramı vardır. Bu, böylelikle daha fazla klasör gibi bir yapı bloblarınızın düzenleyebilirsiniz. Örneğin bir kapsayıcıda yer alan ve **resimler** olarak adlandırılan aşağıdaki blok blobları kümesine göz atın:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Çağırdığınızda **ListBlobs** kapsayıcısında (Yukarıdaki örnek) olduğu gibi döndürülen koleksiyon içeren **CloudBlobDirectory** ve **CloudBlockBlob** nesneleri dizinleri ve blobları en üst düzeyinde bulunan temsil eden. Sonuç çıktısı aşağıdaki gibidir:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


İsteğe bağlı olarak **ListBlobs** yönteminin **UseFlatBlobListing** parametresini **true** olarak ayarlayabilirsiniz. Her blob olarak döndürülen sonuçlanır bir **CloudBlockBlob**dizin ne olursa olsun. Çağrı işte **ListBlobs**:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

ve sonuçları şunlardır:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Daha fazla bilgi için [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Blob’ları indirme
Blob’ları indirmek için ilk olarak bir blob başvurusu alın ve ardından **DownloadToStream** yöntemini çağırın. Aşağıdaki örnek, blob içeriklerini bir akış nesnesine aktarmak ve ardından yerel bir dosyaya kalıcı olarak almak için **DownloadToStream** yöntemini kullanır:

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Bunun yanında bir blobun içeriklerini metin dizesi olarak indirmek için **DownloadToStream** yöntemini kullanabilirsiniz.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Blob’ları silme
Bir blobu silmek için önce bir blob başvurusu alın ve sonra çağrı **Sil** yöntemi.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Blob’ları sayfalarda zaman uyumsuz olarak listeleme
Çok sayıda blob listeliyorsanız veya bir listeleme işlemi ile dönen sonuç sayısını denetlemek isterseniz sonuç sayfalarında blobları listeleyebilirsiniz. Bu örnek, geniş bir sonuç kümesinin dönmesini beklerken çalıştırmanın engellenmemesi için sayfalardaki sonuçların zaman uyumsuz olarak nasıl döneceğini gösterir.

Bu örnek düz bir blob listesi gösterir, ancak **ListBlobsSegmentedAsync** metodunun **useFlatBlobListing** parametresini **false** olarak ayarlayarak hiyerarşik bir listeleme gerçekleştirebilirsiniz.

Örnek metot, zaman uyumsuz bir metot çağırdığı için **async** anahtar sözcüğüyle başlamalı ve bir **Görev** nesnesi döndürmelidir. Belirtilen **ListBlobsSegmentedAsync** yöntemi için belirlenen bekleme anahtar kelimesi, listeleme görevi tamamlanana kadar örnek yöntemin çalıştırılmasını askıya alır.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

