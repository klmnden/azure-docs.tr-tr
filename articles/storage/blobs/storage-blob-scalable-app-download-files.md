---
title: "Azure depolama biriminden büyük miktarlarda rastgele veri indirme | Microsoft Docs"
description: "Bir Azure depolama hesabından büyük miktarlarda rastgele veri indirmek için Azure SDK'sını kullanmayı öğrenin"
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 3842860acb1c0fdd9e07f6d2f678ac5d5304003b
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="download-large-amounts-of-random-data-from-azure-storage"></a>Azure depolama biriminden büyük miktarlarda rastgele veri indirme

Bu öğretici üç serinin bir parçasıdır. Bu öğretici Azure depolama biriminden büyük miktarlarda verinin indirmeyi gösterir.

Bölümünde dizisinin üç bilgi nasıl yapılır:

> [!div class="checklist"]
> * Uygulamayı güncelleştirme
> * Uygulamayı çalıştırma
> * Bağlantı sayısını doğrula

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için önceki depolama öğretici tamamlamış olmanız gerekir: [karşıya rastgele verileri Azure depolama paralel büyük miktarlarda][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Sanal makineniz içine uzaktan

 Sanal makineyle bir Uzak Masaüstü oturumu oluşturmak için yerel makinenizde aşağıdaki komutu kullanın. IP adresi, sanal makinenize Publicıpaddress ile değiştirin. İstendiğinde, sanal makine oluşturulurken kullanılan kimlik bilgilerini girin.

```
mstsc /v:<publicIpAddress>
```

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Önceki öğreticide, yalnızca depolama hesabına dosya karşıya. Açık `D:\git\storage-dotnet-perf-scale-app\Program.cs` bir metin düzenleyicisinde. Değiştir `Main` aşağıdaki örnekle yöntemi. Bu örnek açıklamalar karşıya yükleme görev out ve indirme görev ve görev tamamlandığında depolama hesabındaki içerik silmek için uncomments.

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
        // initially to support the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scaleable-app-download-files.
        // DownloadFilesAsync().GetAwaiter().GetResult();
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
        exception = true;
    }
    finally
    {
        // The following function will delete the container and all files contained in them.  This is commented out initialy
        // As the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scaleable-app-download-files has you upload only for one tutorial and download for the other. 
        if (!exception)
        {
            // DeleteExistingContainersAsync().GetAwaiter().GetResult();
        }
        Console.WriteLine("Press any key to exit the application");
        Console.ReadKey();
    }
}
```

Uygulama güncelleştirildikten sonra uygulamayı yeniden derleme gerekir. Açık bir `Command Prompt` gidin `D:\git\storage-dotnet-perf-scale-app`. Uygulamayı çalıştırarak yeniden `dotnet build` aşağıdaki örnekte görüldüğü gibi:

```
dotnet build
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama yeniden göre uygulama ile güncelleştirilmiş kod çalıştırma zamanı geldi. Zaten açık değilse, açık bir `Command Prompt` gidin `D:\git\storage-dotnet-perf-scale-app`.

Tür `dotnet run` uygulamayı çalıştırın.

```
dotnet run
```

Belirtilen depolama hesabında bulunan kapsayıcıları uygulaması okur **storageconnectionstring**. Kullanarak bir defada BLOB'lar 10 dolaşır [ListBlobsSegmented](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmented?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmented_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) kapsayıcıları ve bunları yerel makine kullanarak yüklemeleri yönteminde [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_DownloadToFileAsync_System_String_System_IO_FileMode_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) yöntemi.
Aşağıdaki tabloda [BlobRequestOptions](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions?view=azure-dotnet) tanımlanmış her bir blob olarak yüklenir.

|Özellik|Değer|Açıklama|
|---|---|---|
|[DisableContentMD5Validation](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.disablecontentmd5validation?view=azure-dotnet)| doğru| Bu özellik MD5 karma değeri karşıya içerik denetimi devre dışı bırakır. MD5 doğrulama devre dışı bırakılması daha hızlı bir aktarım üretir. Geçerlilik ya da aktarılan dosyaların bütünlüğünü doğrulamak değil ancak. |
|[StorBlobContentMD5](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.storeblobcontentmd5?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_StoreBlobContentMD5)| yanlış| Bu özellik, bir MD5 karma hesaplanır ve depolanan olup olmadığını belirler.   |

`DownloadFilesAsync` Görev, aşağıdaki örnekte gösterilir:

```csharp
private static async Task DownloadFilesAsync()
{
    CloudBlobClient blobClient = GetCloudBlobClient();

    // Define the BlobRequestionOptions on the download, including disabling MD5 hash validation for this example, this improves the download speed.
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

### <a name="validate-the-connections"></a>Bağlantıları doğrula

Dosyaları karşıdan yüklenirken eşzamanlı bağlantı sayısını da depolama hesabınıza doğrulayabilirsiniz. Açık bir `Command Prompt` ve türü `netstat -a | find /c "blob:https"`. Bu komutu kullanarak şu anda açıldığından bağlantı sayısını gösterir `netstat`. Aşağıdaki örnek öğretici kendiniz çalıştırırken gördüğünüz için benzer bir çıktıya gösterir. Örnekte görebildiğiniz gibi üzerinde 280 bağlantıları rastgele dosyalar depolama hesabından indirirken açın.

```
C:\>netstat -a | find /c "blob:https"
289

C:\>
```

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde seri üç, büyük miktarlarda rastgele verilerin nasıl gibi bir depolama hesabından karşıdan yükleme konusunda öğrenilen:

> [!div class="checklist"]
> * Uygulamayı çalıştırma
> * Bağlantı sayısını doğrula

Üretilen iş ve gecikmeyi ölçümleri Portalı'nda doğrulamak için seri dört kısmına ilerleyin.

> [!div class="nextstepaction"]
> [Üretilen iş ve gecikmeyi ölçümleri portalında doğrulayın](storage-blob-scalable-app-verify-metrics.md)

[previous-tutorial]: storage-blob-scalable-app-upload-files.md