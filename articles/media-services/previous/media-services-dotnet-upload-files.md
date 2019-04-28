---
title: .NET kullanarak bir Media Services hesabına dosya yükleme | Microsoft Docs
description: Medya içeriği oluşturma ve karşıya yükleme varlıklar Media Services'e almayı öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: c9c86380-9395-4db8-acea-507c52066f73
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 4098d55a0b7505b2178c95d612c078a427adc9a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61464224"
---
# <a name="upload-files-into-a-media-services-account-using-net"></a>.NET kullanarak bir Media Services hesabına dosya yükleme 
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Portal](media-services-portal-upload-files.md)
> 
> 

Media Services’de, dijital dosyalar bir varlığa yüklenir (veya alınır). **Varlık** varlığı video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar hakkındaki meta veriler.) içerebilir  Dosyalar yüklendiğinde, içeriğiniz sonraki işleme ve akışla aktarma faaliyetleri için güvenli bir şekilde bulutta depolanmış olur.

Varlık içindeki dosyalara **Varlık Dosyaları** adı verilir. **AssetFile** örneği ve gerçek medya dosyası olan iki farklı bir nesne. Medya dosyası gerçek medya içeriği içerirken AssetFile örneği medya dosyası hakkındaki meta verileri içerir.

> [!NOTE]
> Aşağıdaki maddeler geçerlidir:
> 
> * Media Services IAssetFile.Name özelliğinin değeri, URL'leri akış içeriği için (örneğin, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) oluştururken kullanır. Bu nedenle, yüzde kodlama izin verilmez. Değerini **adı** özelliği aşağıdakilerden herhangi birini içeremez [yüzde kodlama-ayrılmış karakterleri](https://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Ayrıca, yalnızca bir olabilir '.' dosya adı uzantısı için.
> * Adının uzunluğu 260 karakterden uzun olmamalıdır.
> * Media Services ile işleme için desteklenen dosya boyutlarına yönelik üst sınır uygulanır. Dosya boyutu sınırlaması hakkında ayrıntılı bilgi için [bu](media-services-quotas-and-limitations.md) makaleye bakın.
> * Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.
> 

Varlıklar oluşturduğunuzda, aşağıdaki şifreleme seçenekleri belirtebilirsiniz:

* **Hiçbiri**: Şifreleme kullanılmaz. Varsayılan değer budur. Bu seçeneği tercih edildiğinde, içeriğinizi Aktarımdaki veya depolama bölgesinde, bekleyen korunmaz.
  Bir MP4 iletmeyi planlıyorsanız aşamalı indirme kullanarak, bu seçeneği kullanın: 
* **CommonEncryption** -zaten şifrelenmiş ve ortak şifreleme veya PlayReady DRM (örneğin, ile korunan kesintisiz akış PlayReady DRM) ile korunan içerik yüklüyorsanız bu seçeneği kullanın.
* **EnvelopeEncrypted** : AES ile şifrelenmiş HLS yüklüyorsanız bu seçeneği kullanın. Dosyaların Transform Manager tarafından kodlanmış ve şifrelenmiş olması gerektiğini unutmayın.
* **StorageEncrypted** - clear içeriğinizi AES-256 bit şifreleme kullanarak yerel olarak şifreler ve bunu Azure bunu depolandığı bekleme sırasında şifrelenmiş depolama alanına yükler. Depolama Şifrelemesi ile korunan varlıklar, kodlamadan önce otomatik olarak şifrelenerek şifrelenmiş bir dosya sistemine yerleştirilir ve yeni bir çıktı varlığı şeklinde geri yüklenmeden önce isteğe bağlı olarak yeniden şifrelenir. Depolama Şifrelemesinin birincil kullanım nedeni, yüksek kaliteli girdi medya dosyalarınızın güvenliğini güçlü şifrelemeyle diskte bekleyen konumda sağlamak istediğiniz durumdur.
  
    Media Services değil üzerinden dijital hak Yöneticisi (DRM) gibi hat varlıklarınız için disk üzerinde depolama şifrelemesi sağlar.
  
    Şifrelenmiş depolama varlığınız ise varlık teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için [varlık teslim ilkesini yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md).

İle şifrelenmiş varlığınıza belirtirseniz bir **CommonEncrypted** seçeneği veya bir **EnvelopeEncrypted** seçeneğine ihtiyacınız varlığınız ile ilişkilendirilecek bir **ContentKey**. Daha fazla bilgi için [bir ContentKey oluşturma](media-services-dotnet-create-contentkey.md). 

Varlığınıza ile şifrelenmiş belirtirseniz bir **StorageEncrypted** seçeneğini Media Services SDK'sı .NET oluşturur bir **StorageEncrypted** **ContentKey** varlığınıza.

Bu makalede, Media Services varlığa dosyaları karşıya yüklemek için Media Services .NET SDK uzantıları yanı sıra, Media Services .NET SDK'sını kullanma gösterilmektedir.

## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Media Services .NET SDK ile tek bir dosyayı karşıya yükleyin
Aşağıdaki kod, tek bir dosyayı karşıya yüklemek için .NET kullanır. AccessPolicy Bulucu oluşturulur ve karşıya yükleme işlevi tarafından yok. 

```csharp
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }
```


## <a name="upload-multiple-files-with-media-services-net-sdk"></a>Media Services .NET SDK ile birden çok dosya yükleme
Aşağıdaki kod bir varlık oluşturun ve birden çok dosya yükleme işlemini gösterir.

Kod şunları yapar:

* Önceki adımda tanımlanan CreateEmptyAsset yöntemi kullanarak boş bir varlık oluşturur.
* Oluşturur bir **AccessPolicy** varlığına erişim süresi ve izinleri tanımlar örneği.
* Oluşturur bir **Bulucu** varlığına erişim sağlayan bir örneği.
* Oluşturur bir **BlobTransferClient** örneği. Bu tür, Azure BLOB'ları üzerinde çalışan bir istemci temsil eder. Bu örnekte, istemci yükleme ilerleme durumunu izler. 
* Belirtilen dizindeki dosyaları aracılığıyla numaralandırır ve oluşturan bir **AssetFile** örneği her dosya için.
* Media Services kullanarak dosyaları yükler **UploadAsync** yöntemi. 

> [!NOTE]
> Kullanım UploadAsync yöntemi çağrıları değil engellemediğinden emin olun ve dosyalar paralel olarak karşıya yüklenir.
> 
> 

```csharp
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AssetFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }

    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }
```


Çok sayıda varlık karşıya yüklerken aşağıdakileri göz önünde bulundurun:

* Yeni bir **CloudMediaContext** iş parçacığı başına nesne. **CloudMediaContext** sınıf iş parçacığı açısından güvenli değildir.
* NumberOfConcurrentTransfers 5 gibi daha yüksek bir değere 2 varsayılan değerini artırın. Bu özelliğin ayarlanması etkileyen tüm örneklerini **CloudMediaContext**. 
* ParallelTransferThreadCount 10 varsayılan değerinde tutmak.

## <a id="ingest_in_bulk"></a>Varlıklar Media Services .NET SDK kullanarak toplu başlayan kümeniz
Büyük varlık dosyaları karşıya yükleme, varlık oluşturma sırasında bir performans sorunu olabilir. Toplu veya "Toplu başlayan kümeniz" varlıklar başlayan kümeniz, karşıya yükleme işlemi varlık oluşturma ayırma içerir. Bir toplu yaklaşım başlayan kümeniz kullanmak için varlık ve ilişkili dosyalarını tanımlayan bir bildirim (IngestManifest) oluşturun. Ardından ilişkili dosyalar bildirim blob kapsayıcısını karşıya yüklemek için tercih ettiğiniz karşıya yükleme yöntemi kullanın. Microsoft Azure Media Services bildirimi ile ilişkili blob kapsayıcısı izler. Blob kapsayıcısına bir dosya karşıya yüklendikten sonra Microsoft Azure Media Services (IngestManifestAsset) bildirimindeki varlığın yapılandırmasına göre varlık oluşturmayı tamamlar.

Yeni bir IngestManifest oluşturmak için CloudMediaContext IngestManifests koleksiyonunda tarafından kullanıma sunulan Create yöntemini çağırın. Bu yöntem, yeni IngestManifest bildirim sağladığınız adla oluşturur.

```csharp
    IIngestManifest manifest = context.IngestManifests.Create(name);
```

Toplu IngestManifest ile ilişkili olan varlıkları oluşturun. Toplu almak için varlık üzerinde istenen şifreleme seçeneklerini yapılandırın.

```csharp
    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);
```

Bir varlığı bir IngestManifestAsset toplu almak için bir toplu IngestManifest ilişkilendirir. Ayrıca, her varlık yapan AssetFiles ilişkilendirir. Bir IngestManifestAsset oluşturmak için sunucu bağlamı üzerinde Create yöntemini kullanın.

Aşağıdaki örnek, toplu olarak önceden oluşturulmuş iki varlıkları ilişkilendirme ekleme iki yeni IngestManifestAssets bildirim alma gösterir. Her IngestManifestAsset toplu başlayan kümeniz sırasında yüklenen dosyalar her varlık için bir dizi de ilişkilendirir.  

```csharp
    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
```

Blob depolama kapsayıcısının URI tarafından sağlanan varlık dosyaları karşıya özellikli herhangi bir yüksek hızlı bir istemci uygulama kullanabileceğiniz **IIngestManifest.BlobStorageUriForUpload** IngestManifest özelliğidir. 

Aşağıdaki kod varlık dosyaları karşıya yüklemek için .NET SDK'sını kullanmayı gösterir.

```csharp
    static void UploadBlobFile(string containerName, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(containerName);

            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);

            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);

            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });

        copytask.Start();
    }
```

Bu makalede kullanılan örnek için varlık dosyaları karşıya yükleme için kodu aşağıdaki kod örneğinde gösterilmiştir:

```csharp
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
```

İle ilişkili tüm varlıklar için toplu almak ilerleme durumunu belirlemek bir **IngestManifest** istatistikleri özelliği yoklayarak **IngestManifest**. İlerleme durumu bilgileri güncelleştirmek için yeni bir kullanmalısınız **CloudMediaContext** her zaman istatistikleri özelliği yoklar.

Aşağıdaki örnek, bir IngestManifest tarafından yoklama gösterir, **kimliği**.

```csharp
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();

          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);

                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }

             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
```


## <a name="upload-files-using-net-sdk-extensions"></a>.NET SDK uzantıları kullanarak karşıya dosya yükleme
Aşağıdaki örnek, .NET SDK uzantıları kullanarak tek bir dosyayı karşıya yükleme işlemini gösterir. Bu durumda **CreateFromFile** yöntemi kullanılır, ancak zaman uyumsuz sürümü de sağlanır (**CreateFromFileAsync**). **CreateFromFile** yöntemi dosya karşıya yükleme ilerlemesini bildirmek üzere dosya adı, şifreleme seçeneği ve bir geri çağırma belirtmenize olanak sağlar.

```csharp
    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }
```

Aşağıdaki örnek, UploadFile işlevini çağırır ve depolama şifrelemesi varlık oluşturma seçeneği olarak belirtir.  

```csharp
    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);
```

## <a name="next-steps"></a>Sonraki adımlar

Karşıya yüklenen varlıklarınızı artık kodlayabilirsiniz. Daha fazla bilgi için bkz. [Varlıkları kodlama](media-services-portal-encode.md).

Yapılandırılmış kapsayıcıya gelen dosyaya göre bir kodlama işi tetiklemek için Azure İşlevleri’ni de kullanabilirsiniz. Daha fazla bilgi için [bu örneğe](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ) bakın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Sonraki adım
Yüklediğiniz bir varlık için Media Services, Git [Medya işleyicisi alma] [ How to Get a Media Processor] makalesi.

[How to Get a Media Processor]: media-services-get-media-processor.md

