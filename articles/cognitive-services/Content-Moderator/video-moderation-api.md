---
title: Azure içerik aracı - video denetleme | Microsoft Docs
description: Olası yetişkin ve saldırganlardan içerik için taramak üzere video yönetimini kullanın.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/02/2018
ms.author: sajagtap
ms.openlocfilehash: ef58f5990d4a0a19ab2b8c61b42ab2a0754dc6fa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355097"
---
# <a name="video-moderation"></a>Video denetimi

Bugün, çevrimiçi görüntüleyiciler popüler ve bölgesel sosyal medya web siteleri ve artan video görünümleri milyarlarca oluşturur. Olası yetişkin ve saldırganlardan içeriği tahmin etmek için makine öğrenme tabanlı hizmetler uygulayarak yönetimini çabalarınız maliyetini düşürün.

## <a name="sign-up-for-the-content-moderator-media-processor-public-preview"></a>İçerik denetleyici medya işlemcisi (genel Önizleme) için kaydolun

### <a name="create-a-free-azure-account"></a>Ücretsiz bir Azure hesabı oluşturun

[Buradan başlayın](https://azure.microsoft.com/free/) zaten yoksa, ücretsiz bir Azure hesabı oluşturmak için.

### <a name="create-an-azure-media-services-account"></a>Azure Media Services hesabı oluşturma

İçerik denetleyicinin video yetenek genel Önizleme kullanılabilir **medya işlemcisi** ücretsiz Azure Media Services (AMS) içinde.

[Bir Azure Media Services hesabı oluşturma](https://docs.microsoft.com/azure/media-services/media-services-portal-create-account) Azure aboneliğinizde.

### <a name="get-azure-active-directory-credentials"></a>Azure Active Directory kimlik bilgilerini alma

   1. Okuma [Azure Media Services portal makale](https://docs.microsoft.com/azure/media-services/media-services-portal-get-started-with-aad) Azure portalı, Azure AD kimlik doğrulama kimlik bilgilerini almak için nasıl kullanılacağını öğrenin.
   1. Okuma [Azure Media Services .NET makale](https://docs.microsoft.com/azure/media-services/media-services-dotnet-get-started-with-aad) .NET SDK'sı ile Azure Active Directory kimlik bilgilerinizi kullanmayı öğrenmek için.

   > [!NOTE]
   > Bu hızlı başlangıç örnek kodda kullanan **hizmet asıl kimlik doğrulaması** hem makalelerinde açıklanan yöntemi.

AMS kimlik bilgilerinizi aldıktan sonra içerik denetleyici medya işlemcisi denemek için iki yolu vardır.

## <a name="use-azure-media-services-explorer"></a>Azure Media Services Gezgini kullanın

Etkileşimli kullanmak [Azure Media Services (AMS) explorer](https://azure.microsoft.com/blog/managing-media-workflows-with-the-new-azure-media-services-explorer-tool/) AMS hesabınızı göz atmak için videoları karşıya yüklemek ve tarama içerik denetleyici medya işlemcisi. [Karşıdan yükleyip](https://github.com/Azure/Azure-Media-Services-Explorer/releases) github'dan ve [kaynak kodunu Gözat](http://github.com/Azure/Azure-Media-Services-Explorer) AMS SDK'sını kullanarak içine çalışmak için.

![Azure Media Services Gezgini içerik Denetleyici ile](images/ams-explorer-content-moderator.PNG)

## <a name="net-quickstart-with-visual-studio-and-c"></a>Visual Studio ve C# ile .NET hızlı başlangıç

1. Yeni bir ekleme **konsol uygulaması (.NET Framework)** çözümünüzü projeye.

   Örnek kodda proje adı **VideoModeration**.

1. Bu proje çözüme yönelik tek başlangıç projesi olarak seçin.

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

Kullanılabilir aşağıdaki NuGet paketi yüklemesi [NuGet](https://www.nuget.org/).

- windowsazure.mediaservices
- windowsazure.mediaservices.Extensions

### <a name="update-the-programs-using-statements"></a>Güncelleştirme program using deyimleri

Değiştirme program using deyimleri kullanıcının.

    using System;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.IO;
    using System.Threading;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using System.Collections.Generic;


### <a name="initialize-application-specific-settings"></a>Uygulamaya özgü ayarları başlatma

Aşağıdaki statik alanları ekleme **Program** Program.cs sınıfında.

    // declare constants and globals
    private static CloudMediaContext _context = null;
    private static CloudStorageAccount _StorageAccount = null;

    // Azure Media Services (AMS) associated Storage Account, Key, and the Container that has 
    // a list of Blobs to be processed.
    static string STORAGE_NAME = "YOUR AMS ASSOCIATED BLOB STORAGE NAME";
    static string STORAGE_KEY = "YOUR AMS ASSOCIATED BLOB STORAGE KEY";
    static string STORAGE_CONTAINER_NAME = "YOUR BLOB CONTAINER FOR VIDEO FILES";

    private static StorageCredentials _StorageCredentials = null;

    // Azure Media Services authentication. See the quickstart for how to get these.
    private const string AZURE_AD_TENANT_NAME = "microsoft.onmicrosoft.com";
    private const string CLIENT_ID = "YOUR CLIENT ID"
    private const string CLIENT_SECRET = "YOUR CLIENT SECRET";

    // REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
    private const string REST_API_ENDPOINT = "YOUR API ENDPOINT";

    // Content Moderator Media Processor Nam
    private const string MEDIA_PROCESSOR = "Azure Media Content Moderator";

    // Input and Output files in the current directory of the executable
    private const string INPUT_FILE = "VIDEO FILE NAME";
    private const string OUTPUT_FOLDER = "";

### <a name="create-a-preset-file-json"></a>Önceden belirlenmiş bir dosya (json) oluşturun

Geçerli dizinde sürüm numarasına sahip bir JSON dosyası oluşturun.

    private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";
    //Example file content:
    //        {
    //             "version": "2.0"
    //        }
    private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";

### <a name="add-the-following-code-to-the-main-method"></a>Ana yöntemine aşağıdaki kodu ekleyin

Blob depolama alanına videolarınızı olduğu durumlarda ana yöntem ilk Azure medya içeriği ve bir Azure depolama bağlamı oluşturur.
Kalan kod bir yerel klasör, blob veya bir Azure depolama kapsayıcısı içinde birden çok BLOB video tarar.
Tüm seçenekleri diğer satırlar kod yorum oluşturmayı deneyebilirsiniz.

    // Create Azure Media Context
    CreateMediaContext();

    // Create Storage Context
    CreateStorageContext();

    // Use a file as the input.
    // IAsset asset = CreateAssetfromFile();
    
    // -- OR ---
    
    // Or a blob as the input
    // IAsset asset = CreateAssetfromBlob((CloudBlockBlob)GetBlobsList().First());

    // Then submit the asset to Content Moderator
    // RunContentModeratorJob(asset);

    //-- OR ----

    // Just run the content moderator on all blobs in a list (from a Blob Container)
    RunContentModeratorJobOnBlobs();

### <a name="add-the-code-to-create-an-azure-media-context"></a>Bir Azure Media bağlamı oluşturmak için kodu ekleyin

    /// <summary>
    /// Creates a media context from azure credentials
    /// </summary>
    static void CreateMediaContext()
    {
        // Get Azure AD credentials
        var tokenCredentials = new AzureAdTokenCredentials(AZURE_AD_TENANT_NAME,
            new AzureAdClientSymmetricKey(CLIENT_ID, CLIENT_SECRET),
            AzureEnvironments.AzureCloudEnvironment);

        // Initialize an Azure AD token
        var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

        // Create a media context
        _context = new CloudMediaContext(new Uri(REST_API_ENDPOINT), tokenProvider);
    }

### <a name="add-the-code-to-create-an-azure-storage-context"></a>Bir Azure depolama bağlamı oluşturmak için kodu ekleyin
Blob depolama alanınızın erişmek için depolama kimlik bilgilerinizi oluşturulan depolama bağlamı kullanın.

    /// <summary>
    /// Creates a storage context from the AMS associated storage name and key
    /// </summary>
    static void CreateStorageContext()
    {
        // Get a reference to the storage account associated with a Media Services account. 
        if (_StorageCredentials == null)
        {
            _StorageCredentials = new StorageCredentials(STORAGE_NAME, STORAGE_KEY);
        }
        _StorageAccount = new CloudStorageAccount(_StorageCredentials, false);
    }

### <a name="add-the-code-to-create-azure-media-assets-from-local-file-and-blob"></a>Yerel dosya ve blob Azure medya varlıklar oluşturmak için kodu ekleyin
İşlerini içerik denetleyici medya işlemcisi çalıştığı **varlıklar** Azure Media Services platformunun içinde.
Bu yöntemler, yerel bir dosya veya ilişkili bir blob varlıklar oluşturun.

    /// <summary>
    /// Creates an Azure Media Services Asset from the video file
    /// </summary>
    /// <returns>Asset</returns>
    static IAsset CreateAssetfromFile()
    {
        return _context.Assets.CreateFromFile(INPUT_FILE, AssetCreationOptions.None); ;
    }

    /// <summary>
    /// Creates an Azure Media Services asset from your blog storage
    /// </summary>
    /// <param name="Blob"></param>
    /// <returns>Asset</returns>
    static IAsset CreateAssetfromBlob(CloudBlockBlob Blob)
    {
        // Create asset from the FIRST blob in the list and return it
        return _context.Assets.CreateFromBlob(Blob, _StorageCredentials, AssetCreationOptions.None);
    }

### <a name="add-the-code-to-scan-a-collection-of-videos-as-blobs-within-a-container"></a>Bir kapsayıcıdaki (BLOB) olarak videolar koleksiyonu taramak için kodu ekleyin

    /// <summary>
    /// Runs the Content Moderator Job on all Blobs in a given container name
    /// </summary>
    static void RunContentModeratorJobOnBlobs()
    {
        // Get the reference to the list of Blobs. See the following method.
        var blobList = GetBlobsList();

        // Iterate over the Blob list items or work on specific ones as needed
        foreach (var sourceBlob in blobList)
        {
            // Create an Asset
            IAsset asset = _context.Assets.CreateFromBlob((CloudBlockBlob)sourceBlob,
                               _StorageCredentials, AssetCreationOptions.None);
            asset.Update();

            // Submit to Content Moderator
            RunContentModeratorJob(asset);
        }
    }

    /// <summary>
    /// Get all blobs in your container
    /// </summary>
    /// <returns></returns>
    static IEnumerable<IListBlobItem> GetBlobsList()
    {
        // Get a reference to the Container within the Storage Account
        // that has the files (blobs) for moderation
        CloudBlobClient CloudBlobClient = _StorageAccount.CreateCloudBlobClient();
        CloudBlobContainer MediaBlobContainer = CloudBlobClient.GetContainerReference(STORAGE_CONTAINER_NAME);

        // Get the reference to the list of Blobs 
        var blobList = MediaBlobContainer.ListBlobs();
        return blobList;
    }

### <a name="add-the-method-to-run-the-content-moderator-job"></a>İçerik denetleyici işi çalıştırmak için bir yöntem ekleyin

    /// <summary>
    /// Run the Content Moderator job on the designated Asset from local file or blob storage
    /// </summary>
    /// <param name="asset"></param>
    static void RunContentModeratorJob(IAsset asset)
    {
        // Grab the presets
        string configuration = File.ReadAllText(CONTENT_MODERATOR_PRESET_FILE);

        // grab instance of Azure Media Content Moderator MP
        IMediaProcessor mp = _context.MediaProcessors.GetLatestMediaProcessorByName(MEDIA_PROCESSOR);

        // create Job with Content Moderator task
        IJob job = _context.Jobs.Create(String.Format("Content Moderator {0}",
                asset.AssetFiles.First() + "_" + Guid.NewGuid()));

        ITask contentModeratorTask = job.Tasks.AddNew("Adult and racy classifier task",
                mp, configuration,
                TaskOptions.None);
        contentModeratorTask.InputAssets.Add(asset);
        contentModeratorTask.OutputAssets.AddNew("Adult and racy classifier output",
            AssetCreationOptions.None);

        job.Submit();


        // Create progress printing and querying tasks
        Task progressPrintTask = new Task(() =>
        {
            IJob jobQuery = null;
            do
            {
                var progressContext = _context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                    .First();
                    Console.WriteLine(string.Format("{0}\t{1}",
                    DateTime.Now,
                    jobQuery.State));
                    Thread.Sleep(10000);
             }
             while (jobQuery.State != JobState.Finished &&
             jobQuery.State != JobState.Error &&
             jobQuery.State != JobState.Canceled);
        });
        progressPrintTask.Start();

        Task progressJobTask = job.GetExecutionProgressTask(
        CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling 
        // method for job progress should log errors.  Here we check 
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            ErrorDetail error = job.Tasks.First().ErrorDetails.First();
            Console.WriteLine(string.Format("Error: {0}. {1}",
            error.Code,
            error.Message));
        }

        DownloadAsset(job.OutputMediaAssets.First(), OUTPUT_FOLDER);
    }

### <a name="add-a-couple-of-helper-functions"></a>Yardımcı işlevleri birkaç ekleme

Bu yöntemler Azure Media Services varlığından içerik denetleyici çıktı dosyası (JSON) indirin ve böylece programın konsolunda çalışan durumu oturum yönetimini işinin durumunu izlemenize yardımcı olması.

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    // event handler for Job State
    static void StateChanged(object sender, JobStateChangedEventArgs e)
    {
        Console.WriteLine("Job state changed event:");
        Console.WriteLine("  Previous state: " + e.PreviousState);
        Console.WriteLine("  Current state: " + e.CurrentState);
        switch (e.CurrentState)
        {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("Job finished.");
                break;
            case JobState.Canceling:
            case JobState.Queued:
            case JobState.Scheduled:
            case JobState.Processing:
                Console.WriteLine("Please wait...\n");
                break;
            case JobState.Canceled:
                Console.WriteLine("Job is canceled.\n");
                break;
            case JobState.Error:
                Console.WriteLine("Job failed.\n");
                break;
            default:
                break;
        }
    }

### <a name="run-the-program-and-review-the-output"></a>Programını çalıştırın ve çıktıyı gözden geçirin

İçerik yönetimini iş tamamlandıktan sonra JSON yanıt analiz edin. Bu öğelerden oluşur:

- Video bilgilerinin özeti
- **Görüntüleri** olarak "**parçaları**"
- **Anahtar çerçeveleri** olarak "**olayları**" ile bir **reviewRecommended "(true veya false (=)"** bayrağı temel alarak **yetişkin** ve **Racy** puanları
- **Başlat**, **süresi**, **totalDuration**, ve **zaman damgası** "çizgilerine içinde". Bölün **ölçeği** saniye cinsinden sayısı alınamadı.
 
> [!NOTE]

> - `adultScore` olası varlığı ve tahmin puan cinsel açık veya bazı durumlarda yetişkinlere yönelik olarak kabul içeriği temsil eder.
> - `racyScore` olası varlığı ve tahmin puan cinsel müstehcen veya bazı durumlarda olgun sayılabilecek içeriği temsil eder.
> - `adultScore` ve `racyScore` 0 ve 1 arasında. Yüksek puanı kategori uygulanabilen yüksek modeli tahmin etmektir. Bu önizleme el ile kodlanmış sonuçlar yerine bir istatistik modeli kullanır. Her kategoride gereksinimlerinizi için nasıl hizalandığını belirlemek için kendi içerikle sınama öneririz.
> - `reviewRecommended` true veya false iç puan üzerinde eşikleri bağlı değil. Müşteriler bu değeri kullanın ya da kendi içerik ilkelerine dayalı özel eşikler karar değerlendirmelisiniz.
>

    {
    "version": 2,
    "timescale": 90000,
    "offset": 0,
    "framerate": 50,
    "width": 1280,
    "height": 720,
    "totalDuration": 18696321,
    "fragments": [
    {
      "start": 0,
      "duration": 18000
    },
    {
      "start": 18000,
      "duration": 3600,
      "interval": 3600,
      "events": [
        [
          {
            "reviewRecommended": false,
            "adultScore": 0.00001,
            "racyScore": 0.03077,
            "index": 5,
            "timestamp": 18000,
            "shotIndex": 0
          }
        ]
      ]
    },
    {
      "start": 18386372,
      "duration": 119149,
      "interval": 119149,
      "events": [
        [
          {
            "reviewRecommended": true,
            "adultScore": 0.00000,
            "racyScore": 0.91902,
            "index": 5085,
            "timestamp": 18386372,
            "shotIndex": 62
          }
        ]
      ]
    }
    ]
    }

## <a name="next-steps"></a>Sonraki adımlar

Nasıl oluşturulacağını öğrenin [video incelemeleri](video-reviews-quickstart-dotnet.md) yönetimini çıktısından.

Ekleme [dökümü yönetimini](video-transcript-moderation-review-tutorial-dotnet.md) video incelemelere.

Yapı konusunda ayrıntılı öğretici kullanıma bir [tamamlamak videoyu ve dökümü yönetimini çözüm](video-transcript-moderation-review-tutorial-dotnet.md).

[Visual Studio çözümü indirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer içerik denetleyici hızlı başlangıç ipuçları için .NET için.
