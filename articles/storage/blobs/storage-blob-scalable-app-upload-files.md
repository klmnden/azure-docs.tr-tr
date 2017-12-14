---
title: "Büyük miktarda rastgele verilerin Azure Storage'a paralel karşıya | Microsoft Docs"
description: "Büyük miktarda paralel bir Azure depolama hesabı için rastgele verilerin karşıya yüklemek için Azure SDK'sını kullanmayı öğrenin"
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
ms.openlocfilehash: c587a5fb23ad72b4a20475a83ad6ac4764717c42
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="upload-large-amounts-of-random-data-in-parallel-to-azure-storage"></a>Büyük miktarda Azure depolama paralel rastgele verilerin karşıya yükle

Bu öğretici iki serinin bir parçasıdır. Bu öğretici bir Azure depolama hesabı büyük miktarda rastgele veri yükleyen bir uygulama dağıttığınız gösterir.

Bölümünde dizisinin iki bilgi nasıl yapılır:

> [!div class="checklist"]
> * Bağlantı dizesini yapılandırma
> * Uygulama oluşturma
> * Uygulamayı çalıştırma
> * Bağlantı sayısını doğrula

Azure blob depolama, verilerinizi depolamak için ölçeklenebilir bir hizmet sunar. Uygulamanızı nasıl blob depolama works önerilen bir anlamak mümkün olduğunca kullanıcı olarak sağlamaktır. Azure BLOB'ları için sınırları bilgisi önemlidir, öğrenmek için bu sınırları hakkında daha fazla ziyaret edin: [depolama ölçeklenebilirlik hedefleri blob](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-blob-storage-scale-targets).

[Bölüm adlandırma](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#subheading47) başka bir önemli BLOB'ları kullanarak yüksek performanslı bir uygulama tasarlarken faktördür. Azure depolama bir aralık tabanlı bölümleme düzeni ölçek ve yük dengelemek için kullanır. Bu yapılandırma dosyaları benzer adlandırma kuralları veya öneki ile aynı bölüme gidin anlamına gelir. Bu mantığı dosyaları karşıya yüklenen kapsayıcının adını içerir. Bu öğreticide, GUID'ler adları için rastgele iyi olarak oluşturulmuş içerik olarak dosyaları kullanın. Bunlar daha sonra rastgele adlarıyla beş farklı kapsayıcılar yüklenir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için önceki depolama öğretici tamamlamış olmanız gerekir: [bir sanal makine ve ölçeklenebilir bir uygulama için depolama hesabı oluşturma][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Sanal makineniz içine uzaktan

Sanal makineyle bir Uzak Masaüstü oturumu oluşturmak için yerel makinenizde aşağıdaki komutu kullanın. IP adresi, sanal makinenize Publicıpaddress ile değiştirin. İstendiğinde, sanal makine oluştururken kullanılacak kimlik bilgilerini girin.

```
mstsc /v:<publicIpAddress>
```

## <a name="configure-the-connection-string"></a>Bağlantı dizesini yapılandırma

Azure Portal'da depolama hesabınıza gidin. Seçin **erişim anahtarları** altında **ayarları** depolama hesabınızdaki. Kopya **bağlantı dizesi** birincil veya ikincil anahtarı. Önceki öğreticide oluşturduğunuz sanal makine oturum açın. Açık bir **komut istemi** bir yönetici ve çalışma olarak `setx` komutunu `/m` anahtarı, bu komut bir makine ayar ortam değişkenine kaydeder. Ortam değişkeni, yeniden kadar kullanılamaz **komut istemi**. Değiştir  **\<storageConnectionString\>**  aşağıdaki örnekteki:

```
setx storageconnectionstring "<storageConnectionString>" /m
```

Tamamlandığında, başka bir açık **komut istemi**, gitmek `D:\git\storage-dotnet-perf-scale-app` ve türü `dotnet build` uygulamayı yapılandırmak için.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Gidin `D:\git\storage-dotnet-perf-scale-app`.

Tür `dotnet run` uygulamayı çalıştırın. İlk çalıştırdığınızda `dotnet` geri yükleme hızını artırmak ve çevrimdışı erişimi etkinleştirmek için yerel paket önbelleğiniz doldurur. Bu komutun tamamlanması için bir dakika sürer ve yalnızca bir kez gerçekleşir.

```
dotnet run
```

Uygulama beş rastgele adlandırılmış kapsayıcıları oluşturur ve hazırlama dizindeki dosyaların depolama hesabına yükleniyor başlar. Uygulamanın en az iş parçacığı 100'e ayarlar ve [DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(v=vs.110).aspx) çok sayıda eşzamanlı bağlantı ne zaman izin verildiğinden emin olmak için 100'e uygulamayı çalıştırmayı.

İş parçacığı oluşturma ve bağlantı sınırı ayarları ayarı yanı sıra [BlobRequestOptions](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions?view=azure-dotnet) için [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) yöntemi paralellik kullanın ve MD5 karma doğrulama devre dışı bırakmak için yapılandırılır. 100 mb bloklarında karşıya yüklenen dosyaların, bu yapılandırma daha iyi performans sağlar, ancak kötü gerçekleştirme kullanıyorsanız, ağ hatası tüm 100 mb blok ise olarak denenir maliyetli olabilir.

|Özellik|Değer|Açıklama|
|---|---|---|
|[ParallelOperationThreadCount](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.paralleloperationthreadcount?view=azure-dotnet)| 8| Ayar blob karşıya yüklenirken bloklarda keser. En yüksek performans için bu değer çekirdek sayısı 8 kat olmalıdır. |
|[DisableContentMD5Validation](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.disablecontentmd5validation?view=azure-dotnet)| true| Bu özellik MD5 karma değeri karşıya içerik denetimi devre dışı bırakır. MD5 doğrulama devre dışı bırakılması daha hızlı bir aktarım üretir. Geçerlilik ya da aktarılan dosyaların bütünlüğünü doğrulamak değil ancak.   |
|[StorBlobContentMD5](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.storeblobcontentmd5?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_StoreBlobContentMD5)| yanlış| Bu özellik, bir MD5 karma hesaplanır ve dosyasıyla birlikte saklanır, belirler.   |
| [RetryPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.retrypolicy?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_RetryPolicy)| 2-ikinci geri Çekilme 10 en fazla yeniden deneme ile |Yeniden deneme ilkesi isteklerinin belirler. Bağlantı hataları yeniden deneme işlemi, bu örnekte bir [ExponentialRetry](/dotnet/api/microsoft.windowsazure.storage.retrypolicies.exponentialretry?view=azure-dotnet) İlkesi 2 saniyelik geri Çekilme ve maksimum yeniden deneme sayısı 10 ile yapılandırılır. Bu ayar, uygulamanızın basarsa yakın aldığında önemlidir [depolama ölçeklenebilirlik hedefleri blob](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-blob-storage-scale-targets).  |

`UploadFilesAsync` Görev, aşağıdaki örnekte gösterilir:

```csharp
private static async Task UploadFilesAsync()
{
    // Create random 5 characters containers to upload files to.
    CloudBlobContainer[] containers = await GetRandomContainersAsync();
    var currentdir = System.IO.Directory.GetCurrentDirectory();

    // path to the directory to upload
    string uploadPath = currentdir + "\\upload";
    Stopwatch time = Stopwatch.StartNew();
    try
    {
        Console.WriteLine("Iterating in directiory: {0}", uploadPath);
        int count = 0;
        int max_outstanding = 100;
        int completed_count = 0;

        // Define the BlobRequestionOptions on the upload.
        // This includes defining an exponential retry policy to ensure that failed connections are retried with a backoff policy. As multiple large files are being uploaded
        // large block sizes this can cause an issue if an exponential retry policy is not defined.  Additionally parallel operations are enabled with a thread count of 8
        // This could be should be multiple of the number of cores that the machine has. Lastly MD5 hash validation is disabled for this example, this improves the upload speed.
        BlobRequestOptions options = new BlobRequestOptions
        {
            ParallelOperationThreadCount = 8,
            DisableContentMD5Validation = true,
            StoreBlobContentMD5 = false
        };
        // Create a new instance of the SemaphoreSlim class to define the number of threads to use in the application.
        SemaphoreSlim sem = new SemaphoreSlim(max_outstanding, max_outstanding);

        List<Task> tasks = new List<Task>();
        Console.WriteLine("Found {0} file(s)", Directory.GetFiles(uploadPath).Count());

        // Iterate through the files
        foreach (string path in Directory.GetFiles(uploadPath))
        {
            // Create random file names and set the block size that is used for the upload.
            var container = containers[count % 5];
            string fileName = Path.GetFileName(path);
            Console.WriteLine("Uploading {0} to container {1}.", path, container.Name);
            CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

            // Set block size to 100MB.
            blockBlob.StreamWriteSizeInBytes = 100 * 1024 * 1024;
            await sem.WaitAsync();

            // Create tasks for each file that is uploaded. This is added to a collection that executes them all asyncronously.  
            tasks.Add(blockBlob.UploadFromFileAsync(path, null, options, null).ContinueWith((t) =>
            {
                sem.Release();
                Interlocked.Increment(ref completed_count);
            }));
            count++;
        }

        // Creates an asynchronous task that completes when all the uploads complete.
        await Task.WhenAll(tasks);

        time.Stop();

        Console.WriteLine("Upload has been completed in {0} seconds. Press any key to continue", time.Elapsed.TotalSeconds.ToString());

        Console.ReadLine();
    }
    catch (DirectoryNotFoundException ex)
    {
        Console.WriteLine("Error parsing files in the directory: {0}", ex.Message);
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```

Windows sistemi üzerinde çalışan bir kesilmiş uygulama çıkış örneğidir.

```
Created container https://mystorageaccount.blob.core.windows.net/9efa7ecb-2b24-49ff-8e5b-1d25e5481076
Created container https://mystorageaccount.blob.core.windows.net/bbe5f0c8-be9e-4fc3-bcbd-2092433dbf6b
Created container https://mystorageaccount.blob.core.windows.net/9ac2f71c-6b44-40e7-b7be-8519d3ba4e8f
Created container https://mystorageaccount.blob.core.windows.net/47646f1a-c498-40cd-9dae-840f46072180
Created container https://mystorageaccount.blob.core.windows.net/38b2cdab-45fa-4cf9-94e7-d533837365aa
Iterating in directiory: D:\git\storage-dotnet-perf-scale-app\upload
Found 50 file(s)
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\1d596d16-f6de-4c4c-8058-50ebd8141e4d.txt to container 9efa7ecb-2b24-49ff-8e5b-1d25e5481076.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\242ff392-78be-41fb-b9d4-aee8152a6279.txt to container bbe5f0c8-be9e-4fc3-bcbd-2092433dbf6b.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\38d4d7e2-acb4-4efc-ba39-f9611d0d55ef.txt to container 9ac2f71c-6b44-40e7-b7be-8519d3ba4e8f.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\45930d63-b0d0-425f-a766-cda27ff00d32.txt to container 47646f1a-c498-40cd-9dae-840f46072180.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\5129b385-5781-43be-8bac-e2fbb7d2bd82.txt to container 38b2cdab-45fa-4cf9-94e7-d533837365aa.
...
Upload has been completed in 142.0429536 seconds. Press any key to continue
```

### <a name="validate-the-connections"></a>Bağlantıları doğrula

Dosyaları karşıya olsa da, depolama hesabınıza eşzamanlı bağlantı sayısını doğrulayabilirsiniz. Açık bir **komut istemi** ve türü `netstat -a | find /c "blob:https"`. Bu komutu kullanarak şu anda açıldığından bağlantı sayısını gösterir `netstat`. Aşağıdaki örnek öğretici kendiniz çalıştırırken gördüğünüz için benzer bir çıktıya gösterir. Örnekte görebildiğiniz gibi 800 bağlantıları depolama hesabı için rastgele dosyaları karşıya yüklenirken açın. Bu değer, karşıya yükleme çalıştıran boyunca değişir. Paralel blok yığınlar halinde karşıya yükleyerek içeriği aktarmak için gereken süreyi önemli ölçüde azalır.

```
C:\>netstat -a | find /c "blob:https"
800

C:\>
```

## <a name="next-steps"></a>Sonraki adımlar

Bölümünde seri iki, bir depolama hesabına nasıl gibi paralel büyük miktarlarda rastgele verilerin karşıya öğrenilen:

> [!div class="checklist"]
> * Bağlantı dizesini yapılandırma
> * Uygulama oluşturma
> * Uygulamayı çalıştırma
> * Bağlantı sayısını doğrula

Büyük miktarlarda verinin bir depolama hesabından karşıdan yüklemek için seri üç kısmına ilerleyin.

> [!div class="nextstepaction"]
> [Büyük miktarda depolama hesabı paralel büyük dosyaları karşıya yükleme](storage-blob-scalable-app-download-files.md)

[previous-tutorial]: storage-blob-scalable-app-create-vm.md