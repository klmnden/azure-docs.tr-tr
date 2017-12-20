---
title: "Dosyaları .NET kullanarak bir Media Services hesabına veri yükleme | Microsoft Docs"
description: "Oluşturma ve karşıya varlıklar Media Services'e medya içeriği alma hakkında bilgi."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: c9c86380-9395-4db8-acea-507c52066f73
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2017
ms.author: juliako
ms.openlocfilehash: ec8c1da633374ba684f6a0a895c542ee76ef73b8
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="upload-files-into-a-media-services-account-using-net"></a>.NET kullanarak bir Media Services hesabına dosya yükleme
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-upload-files.md)
> * [REST](media-services-rest-upload-files.md)
> * [Portal](media-services-portal-upload-files.md)
> 
> 

Media Services’de, dijital dosyalar bir varlığa yüklenir (veya alınır). **Varlık** varlık içerebilir video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar hakkındaki meta verileri.)  Dosyalar yüklendiğinde, içeriğiniz sonraki işleme ve akışla aktarma faaliyetleri için güvenli bir şekilde bulutta depolanmış olur.

Varlık içindeki dosyalara **Varlık Dosyaları** adı verilir. **AssetFile** örneği ve gerçek medya dosyası olan iki farklı nesneler. Medya dosyasının gerçek medya içeriği içerirken AssetFile örneği medya dosyası hakkındaki meta verileri içerir.

> [!NOTE]
> Aşağıdaki maddeler geçerlidir:
> 
> * Media Services IAssetFile.Name özelliğinin değeri, URL akış içeriğini (örneğin, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) oluştururken kullanır. Bu nedenle, yüzde kodlama izin verilmiyor. Değeri **adı** özelliği aşağıdakilerden herhangi birini içeremez [yüzde kodlama-ayrılmış karakterleri](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Ayrıca, yalnızca bir olabilir '.' dosya adı uzantısı için.
> * Adının uzunluğu 260 karakterden uzun olmamalıdır.
> * Media Services ile işleme için desteklenen dosya boyutlarına yönelik üst sınır uygulanır. Dosya boyutu sınırlaması hakkında ayrıntılı bilgi için lütfen [bu konu başlığını](media-services-quotas-and-limitations.md) inceleyin.
> * Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu](media-services-dotnet-manage-entities.md#limit-access-policies) konu başlığına bakın.
> 

Varlıklar oluşturduğunuzda, aşağıdaki şifreleme seçeneklerini belirtebilirsiniz. 

* **Hiçbiri**: Şifreleme kullanılmaz. Varsayılan değer budur. Bu seçenek kullanıldığında, içeriğinizin aktarım veya deposunda kalan korunmadığını unutmayın.
  Aşamalı indirme kullanarak bir MP4 iletmeyi planlıyorsanız bu seçeneği kullanın. 
* **CommonEncryption** -zaten şifrelenmiş ve ortak şifreleme veya PlayReady DRM (örneğin, ile korunan kesintisiz akış PlayReady DRM) ile korunan içerik yüklüyorsanız bu seçeneği kullanın.
* **EnvelopeEncrypted** – AES ile şifrelenmiş HLS yüklüyorsanız bu seçeneği kullanın. Dosyaların Transform Manager tarafından kodlanmış ve şifrelenmiş olması gerektiğini unutmayın.
* **StorageEncrypted** - Temizle içeriğinizi yerel olarak AES 256 bit şifreleme kullanarak şifreler ve Azure depolanır şifrelenen depolama alanına yükler. Depolama Şifrelemesi ile korunan varlıklar, kodlamadan önce otomatik olarak şifrelenerek şifrelenmiş bir dosya sistemine yerleştirilir ve yeni bir çıktı varlığı şeklinde geri yüklenmeden önce isteğe bağlı olarak yeniden şifrelenir. Depolama şifrelemesinin birincil kullanım durumu, güçlü şifreleme REST diskte, yüksek kaliteli giriş medya dosyalarınızın güvenliğini sağlamak istediğiniz durumdur.
  
    Media Services değil üzerinden dijital hak Yöneticisi (DRM) gibi hat varlıklarınızı için disk üzerinde depolama şifreleme sağlar.
  
    Şifrelenmiş depolama varlığınız olması durumunda, varlık teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için bkz: [varlık teslim ilkesini yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md).

Varlığınıza ile şifrelenmiş belirtirseniz, bir **CommonEncrypted** seçeneği veya bir **EnvelopeEncypted** seçeneği gerekir, varlıkla ilişkilendirme bir **ContentKey**. Daha fazla bilgi için bkz: [bir ContentKey oluşturma](media-services-dotnet-create-contentkey.md). 

Varlığınıza ile şifrelenmiş belirtirseniz bir **StorageEncrypted** seçeneğini Media Services SDK'sı .NET oluşturacak bir **StorateEncrypted** **ContentKey** varlığınıza.

Bu konu, Media Services .NET SDK uzantıları yanı sıra, Media Services .NET SDK'sı bir Media Services varlığa dosyaları yüklemek için nasıl kullanılacağını gösterir.

## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Media Services .NET SDK'sı ile tek bir dosyayı karşıya yükleme
Aşağıdaki örnek kod, tek bir dosyayı karşıya yüklemek için .NET SDK'yı kullanır. AccessPolicy ve Bulucu oluşturulur ve karşıya yükleme işlevi tarafından yok. 


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


## <a name="upload-multiple-files-with-media-services-net-sdk"></a>Media Services .NET SDK'sı ile birden çok dosya karşıya yükleme
Aşağıdaki kod, bir varlık oluşturun ve birden çok dosya karşıya yükleme gösterilmektedir.

Kod şunları yapar:

* Önceki adımda tanımlanan CreateEmptyAsset yöntemi kullanarak boş bir varlık oluşturur.
* Oluşturur bir **AccessPolicy** izinler ve erişim süresi varlık için tanımlar örneği.
* Oluşturur bir **Bulucu** varlık için erişim sağlayan örneği.
* Oluşturur bir **BlobTransferClient** örneği. Bu tür Azure BLOB'ları üzerinde çalıştığı bir istemci temsil eder. Bu örnekte, istemci yükleme ilerleme durumunu izlemek için kullanırız. 
* Belirtilen dizindeki dosyaları aracılığıyla numaralandırır ve oluşturan bir **AssetFile** her dosya için örneği.
* Media Services kullanarak dosyaları karşıya yükleme **UploadAsync** yöntemi. 

> [!NOTE]
> Çağrıları engellemediğini ve paralel olarak karşıya yüklenen dosyaların emin olmak için UploadAsync yöntemini kullanın.
> 
> 

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

                // It is recommended to validate AccestFiles before upload. 
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



Çok sayıda varlıklar karşıya yüklenirken aşağıdakileri göz önünde bulundurun.

* Yeni bir **CloudMediaContext** iş parçacığı başına nesne. **CloudMediaContext** sınıfı iş parçacığı içinde korumalı değil.
* NumberOfConcurrentTransfers 2 varsayılan değerinden daha yüksek bir değere 5 gibi artırın. Bu özelliği ayarlamak etkiler tüm örneklerini **CloudMediaContext**. 
* ParallelTransferThreadCount 10 varsayılan değerini koruyun.

## <a id="ingest_in_bulk"></a>Varlıklar Media Services .NET SDK kullanarak toplu alma
Büyük varlık dosyaları karşıya yükleme varlık oluşturma sırasında bir performans sorunu olabilir. Varlıklar toplu ya da "Toplu alma" alanını, karşıya yükleme işlemi varlık oluşturma kesilmesi içerir. Toplu bir yaklaşım alanını kullanmak için varlık ve ilişkili dosyaları tanımlayan bir bildirim (IngestManifest) oluşturun. Sonra bildirim blob kapsayıcısına ilişkili dosyaları karşıya yükleme için tercih ettiğiniz karşıya yükleme yöntemini kullanın. Microsoft Azure Media Services bildirimini ile ilişkili blob kapsayıcısı izler. Dosya blob kapsayıcısına yüklendikten sonra Microsoft Azure Media Services (IngestManifestAsset) bildiriminde varlık yapılandırmasını temel alarak varlık oluşturma işlemini tamamlar.

Yeni bir IngestManifest oluşturmak için CloudMediaContext IngestManifests koleksiyonda tarafından sunulan oluşturma yöntemini çağırın. Bu yöntem, sağladığınız bildirim adıyla yeni IngestManifest oluşturur.

    IIngestManifest manifest = context.IngestManifests.Create(name);

IngestManifest toplu ile ilişkilendirilecek varlıklar oluşturun. Toplu alma için varlık üzerinde istenen şifreleme seçeneklerini yapılandırın.

    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);

Bir IngestManifestAsset toplu alma için bir toplu IngestManifest bir varlık ilişkilendirir. Ayrıca, her varlık yapacak AssetFiles ilişkilendirir. Bir IngestManifestAsset oluşturmak için sunucu içeriğine oluşturma yöntemini kullanın.

Aşağıdaki örnek, daha önce toplu oluşturulan iki varlıklar ilişkilendirmek ekleme iki yeni IngestManifestAssets bildirim alma gösterir. Her IngestManifestAsset toplu alma sırasında yüklenecek dosyaları her varlık için bir dizi de ilişkilendirir.  

    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });

Blob depolama kapsayıcısını URI tarafından sağlanan varlık dosyaları yükleme özellikli tüm yüksek hızlı istemci uygulaması kullanabileceğiniz **IIngestManifest.BlobStorageUriForUpload** IngestManifest özelliği. Bir önem düzeyindeki yüksek hızlı karşıya yükleme hizmeti [Aspera istendiğinde Azure uygulaması için](https://datamarket.azure.com/application/2cdbc511-cb12-4715-9871-c7e7fbbb82a6). Aşağıdaki kod örneğinde gösterildiği gibi varlıklar dosyaları karşıya yükleme için kod da yazabilirsiniz.

    static void UploadBlobFile(string destBlobURI, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(destBlobURI);

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

Bu konuda kullanılan örnek için varlık dosyaları karşıya yükleme için kodu aşağıdaki kod örneğinde gösterilir.

    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);


İle ilişkili tüm varlıklar için toplu alma ilerlemesini belirlemek bir **IngestManifest** istatistikleri özelliği yoklayarak **IngestManifest**. İlerleme durumu bilgileri güncelleştirmek için yeni bir kullanmalısınız **CloudMediaContext** her zaman istatistikleri özelliği yoklamak.

Aşağıdaki örnek, bir IngestManifest tarafından yoklama gösterir, **kimliği**.

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



## <a name="upload-files-using-net-sdk-extensions"></a>.NET SDK uzantıları kullanarak dosyaları karşıya yükleme
Aşağıdaki örnek, .NET SDK uzantıları kullanarak tek bir dosyayı karşıya gösterilmektedir. Bu durumda **CreateFromFile** yöntemi kullanılır, ancak zaman uyumsuz sürümü de kullanılabilir (**CreateFromFileAsync**). **CreateFromFile** yöntemini dosya adı, şifreleme seçeneği ve bir geri çağırma dosya karşıya yükleme ilerlemesini bildirmek üzere belirtmenize olanak sağlar.

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

Aşağıdaki örnek UploadFile işlevini çağırır ve depolama şifrelemesi varlık oluşturma seçeneği olarak belirtir.  

    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);

## <a name="next-steps"></a>Sonraki adımlar

Karşıya yüklenen varlıklarınızı artık kodlayabilirsiniz. Daha fazla bilgi için bkz. [Varlıkları kodlama](media-services-portal-encode.md).

Yapılandırılmış kapsayıcıya gelen dosyaya göre bir kodlama işi tetiklemek için Azure İşlevleri’ni de kullanabilirsiniz. Daha fazla bilgi için [bu örneğe](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ) bakın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Sonraki adım
Bir varlık için Media Services karşıya yüklediğiniz, Git [medya işlemcisi alma] [ How to Get a Media Processor] konu.

[How to Get a Media Processor]: media-services-get-media-processor.md

