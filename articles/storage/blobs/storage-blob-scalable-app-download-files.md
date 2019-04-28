---
title: Azure Depolama’dan büyük miktarda rastgele verileri indirme | Microsoft Docs
description: Azure Depolama hesabından büyük miktarda rastgele verileri indirmek için Azure SDK’nın nasıl kullanılacağını öğrenin
services: storage
author: WenJason
ms.service: storage
ms.devlang: dotnet
ms.topic: tutorial
origin.date: 02/20/2018
ms.date: 04/08/2019
ms.author: v-jay
ms.custom: mvc
ms.subservice: blobs
ms.openlocfilehash: b2c5e2d54c0323e984a48f2540a7df9a3b64b49b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61427566"
---
# <a name="download-large-amounts-of-random-data-from-azure-storage"></a>Azure Depolama’dan büyük miktarda rastgele verileri indirme

Bu öğretici, bir serinin üçüncü bölümüdür. Bu öğretici, Azure Depolama’dan büyük miktarda verileri nasıl indireceğinizi gösterir.

Serinin üçüncü bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uygulamayı güncelleştirme
> * Uygulamayı çalıştırma
> * Bağlantı sayısını doğrulama

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için önceki şu depolama öğreticisini tamamlamış olmanız gerekir: [Büyük miktarda rastgele verileri paralel şekilde Azure Depolama'ya yükleme][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Sanal makinenize uzaktan bağlanma

 Sanal makine ile bir uzak masaüstü oturumu oluşturmak için yerel makinenizde aşağıdaki komutu kullanın. IP adresini, sanal makinenizin publicIPAddress değeriyle değiştirin. İstendiğinde, sanal makine oluşturulurken kullanılan kimlik bilgilerini girin.

```
mstsc /v:<publicIpAddress>
```

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Önceki öğreticide, yalnızca depolama hesabına dosyaları yüklediniz. `D:\git\storage-dotnet-perf-scale-app\Program.cs` dosyasını bir metin düzenleyicisinde açın. `Main` yöntemini aşağıdaki örnekle değiştirin. Bu örnek, karşıya yükleme görevini açıklama satırı yapar ve tamamlandığında indirme görevinin ve depolama hesabındaki içeriği silme görevinin açıklamasını kaldırır.

```csharp
public static void Main(string[] args)
{
    Console.WriteLine("Azure Blob storage performance and scalability sample");
    // Set threading and default connection limit to 100 to ensure multiple threads and connections can be opened.
    // This is in addition to parallelism with the storage client library that is defined in the functions below.
    ThreadPool.SetMinThreads(100, 4);
    ServicePointManager.DefaultConnectionLimit = 100; // (Or More)

    bool exception = false;
    try
    {
        // Call the UploadFilesAsync function.
        UploadFilesAsync().GetAwaiter().GetResult();

        // Uncomment the following line to enable downloading of files from the storage account.  This is commented out
        // initially to support the tutorial at https://docs.azure.cn/storage/blobs/storage-blob-scaleable-app-download-files.
        // DownloadFilesAsync().GetAwaiter().GetResult();
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
        exception = true;
    }
    finally
    {
        // The following function will delete the container and all files contained in them.  This is commented out initially
        // As the tutorial at https://docs.azure.cn/storage/blobs/storage-blob-scaleable-app-download-files has you upload only for one tutorial and download for the other. 
        if (!exception)
        {
            // DeleteExistingContainersAsync().GetAwaiter().GetResult();
        }
        Console.WriteLine("Press any key to exit the application");
        Console.ReadKey();
    }
}
```

Uygulama güncelleştirildikten sonra uygulamayı tekrar derlemeniz gerekir. Bir `Command Prompt` açın ve `D:\git\storage-dotnet-perf-scale-app` dizinine gidin. Aşağıdaki örnekte görüldüğü gibi `dotnet build` çalıştırarak uygulamayı yeniden derleyin:

```
dotnet build
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Şimdi uygulama yeniden derlendiğine göre güncelleştirilmiş kod ile uygulamayı çalıştırmaya sıra gelir. Açık değilse bir `Command Prompt` açın ve `D:\git\storage-dotnet-perf-scale-app` dizinine gidin.

Uygulamayı çalıştırmak için `dotnet run` yazın.

```
dotnet run
```

Uygulama, **storageconnectionstring** içinde belirtilen depolama hesabında bulunan kapsayıcıları okur. Kapsayıcılarda [ListBlobsSegmented](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmented?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmented_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) yöntemini kullanarak aynı anda 10 blobda gezinir ve [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_DownloadToFileAsync_System_String_System_IO_FileMode_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) yöntemini kullanarak bunları yerel makineye indirir.
Aşağıdaki tabloda, indirilen her blob için tanımlanan [BlobRequestOptions](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions?view=azure-dotnet) gösterilir.

|Özellik|Değer|Açıklama|
|---|---|---|
|[DisableContentMD5Validation](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.disablecontentmd5validation?view=azure-dotnet)| true| Bu özellik, karşıya yüklenen içeriğin MD5 karmasının denetimini devre dışı bırakır. MD5 doğrulaması devre dışı bırakıldığında daha hızlı bir aktarım üretilir. Ancak aktarılan dosyaların geçerliliği veya bütünlüğü onaylanmaz. |
|[StoreBlobContentMD5](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.storeblobcontentmd5?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_StoreBlobContentMD5)| false| Bu özellik, bir MD5 karmasının hesaplanıp hesaplanmayacağını ve depolanıp depolanmayacağını belirler.   |

Aşağıdaki örnekte `DownloadFilesAsync` görevi gösterilmektedir:

```csharp
private static async Task DownloadFilesAsync()
{
    CloudBlobClient blobClient = GetCloudBlobClient();

    // Define the BlobRequestOptions on the download, including disabling MD5 hash validation for this example, this improves the download speed.
    BlobRequestOptions options = new BlobRequestOptions
    {
        DisableContentMD5Validation = true,
        StoreBlobContentMD5 = false
    };

    // Retrieve the list of containers in the storage account.  Create a directory and configure variables for use later.
    BlobContinuationToken continuationToken = null;
    List<CloudBlobContainer> containers = new List<CloudBlobContainer>();
    do
    {
        var listingResult = await blobClient.ListContainersSegmentedAsync(continuationToken);
        continuationToken = listingResult.ContinuationToken;
        containers.AddRange(listingResult.Results);
    }
    while (continuationToken != null);

    var directory = Directory.CreateDirectory("download");
    BlobResultSegment resultSegment = null;
    Stopwatch time = Stopwatch.StartNew();

    // Download the blobs
    try
    {
        List<Task> tasks = new List<Task>();
        int max_outstanding = 100;
        int completed_count = 0;

        // Create a new instance of the SemaphoreSlim class to define the number of threads to use in the application.
        SemaphoreSlim sem = new SemaphoreSlim(max_outstanding, max_outstanding);

        // Iterate through the containers
        foreach (CloudBlobContainer container in containers)
        {
            do
            {
                // Return the blobs from the container lazily 10 at a time.
                resultSegment = await container.ListBlobsSegmentedAsync(null, true, BlobListingDetails.All, 10, continuationToken, null, null);
                continuationToken = resultSegment.ContinuationToken;
                {
                    foreach (var blobItem in resultSegment.Results)
                    {

                        if (((CloudBlob)blobItem).Properties.BlobType == BlobType.BlockBlob)
                        {
                            // Get the blob and add a task to download the blob asynchronously from the storage account.
                            CloudBlockBlob blockBlob = container.GetBlockBlobReference(((CloudBlockBlob)blobItem).Name);
                            Console.WriteLine("Downloading {0} from container {1}", blockBlob.Name, container.Name);
                            await sem.WaitAsync();
                            tasks.Add(blockBlob.DownloadToFileAsync(directory.FullName + "\\" + blockBlob.Name, FileMode.Create, null, options, null).ContinueWith((t) =>
                            {
                                sem.Release();
                                Interlocked.Increment(ref completed_count);
                            }));

                        }
                    }
                }
            }
            while (continuationToken != null);
        }

        // Creates an asynchronous task that completes when all the downloads complete.
        await Task.WhenAll(tasks);
    }
    catch (Exception e)
    {
        Console.WriteLine("\nError encountered during transfer: {0}", e.Message);
    }

    time.Stop();
    Console.WriteLine("Download has been completed in {0} seconds. Press any key to continue", time.Elapsed.TotalSeconds.ToString());
    Console.ReadLine();
}
```

### <a name="validate-the-connections"></a>Bağlantıları doğrulama

Dosyalar indirilirken, depolama hesabınıza yönelik eş zamanlı bağlantı sayısını doğrulayabilirsiniz. Bir `Command Prompt` açın ve `netstat -a | find /c "blob:https"` yazın. Bu komut şu anda `netstat` kullanılarak açılan bağlantı sayısını gösterir. Aşağıdaki örnek, öğreticiyi kendiniz çalıştırırken gördüğünüze benzer bir çıktıyı gösterir. Örnekte görebileceğiniz gibi, depolama hesabından rastgele dosyalar indirilirken 280’den fazla bağlantı açıktı.

```
C:\>netstat -a | find /c "blob:https"
289

C:\>
```

## <a name="next-steps"></a>Sonraki adımlar

Serinin üçüncü kısmında, aşağıda örnekleri verilen eylemlerle birlikte bir depolama hesabından büyük miktarlarda rastgele verilerin indirilmesiyle ilgili bilgi edindiniz:

> [!div class="checklist"]
> * Uygulamayı çalıştırma
> * Bağlantı sayısını doğrulama

Portalda aktarım hızı ve gecikme süresi ölçümlerini doğrulamak için serinin dördüncü kısmına ilerleyin.

> [!div class="nextstepaction"]
> [Portalda aktarım hızı ve gecikme süresi ölçümlerini doğrulama](storage-blob-scalable-app-verify-metrics.md)

[previous-tutorial]: storage-blob-scalable-app-upload-files.md