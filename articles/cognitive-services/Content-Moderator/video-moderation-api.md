---
title: Video içeriğinde içeriklere analiz C# -Content Moderator
titlesuffix: Azure Cognitive Services
description: Video içeriği için içerik Moderator SDK'sını kullanarak .NET için çeşitli içeriklere analiz etme
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 7e987c1249360b14fddf8af57c61fdd1a46ee6c5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60605343"
---
# <a name="analyze-video-content-for-objectionable-material-in-c"></a>Video içeriğinde içeriklere analiz edinC#

Bu makalede bilgiler sağlanmaktadır ve yardımcı olması için kod örnekleri, kullanmaya başlama [.NET için içerik Moderator SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) video içeriği yetişkinlere yönelik veya müstehcen içerik için tarayın.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü

## <a name="set-up-azure-resources"></a>Azure kaynakları ayarlama

Content Moderator'ın video denetimi özelliği, ücretsiz bir genel önizleme olarak kullanılabilir **medya İşlemci** Azure Media Services (AMS). Azure Media Services, depolama ve akışla aktarma video içeriği için özel bir Azure hizmeti ' dir. 

### <a name="create-an-azure-media-services-account"></a>Azure Media Services hesabı oluşturma

Bölümündeki yönergeleri [bir Azure Media Services hesabı oluşturma](https://docs.microsoft.com/azure/media-services/media-services-portal-create-account) AMS'ye abone ve ilişkili Azure depolama hesabı oluşturun. Bu depolama hesabında yeni bir Blob Depolama kapsayıcısı oluşturun.

### <a name="create-an-azure-active-directory-application"></a>Bir Azure Active Directory uygulaması oluşturma

Azure portal ve seçin, yeni AMS aboneliğinize gidin **API erişimi** yan menüsünde. Seçin **hizmet sorumlusu ile Azure Media Services'e bağlanma**. Değeri Not **REST API uç noktası** alan; ihtiyaç duyacağınız bunu daha sonra.

İçinde **Azure AD uygulaması** bölümünden **Yeni Oluştur** ve yeni adı Azure AD uygulama kaydı (örneğin, "VideoModADApp"). Tıklayın **Kaydet** ve uygulama yapılandırılmış birkaç dakika bekleyin. Ardından, yeni uygulama kaydı altında görmelisiniz **Azure AD uygulaması** sayfasının bölümünde.

Uygulama kaydınızı seçip tıklayın **uygulamasını Yönet** altındaki düğmesi. Değeri Not **uygulama kimliği** alan; ihtiyaç duyacağınız bunu daha sonra. Seçin **ayarları** > **anahtarları**ve yeni bir anahtar (örneğin, "VideoModKey") için bir açıklama girin. Tıklayın **Kaydet**ve ardından yeni anahtar değeri dikkat edin. Bu dizeyi kopyalayın ve güvenli bir yere kaydedin.

Yukarıdaki işlem daha kapsamlı bir kılavuz için bkz: [Azure AD kimlik doğrulamayı kullanmaya başlama](https://docs.microsoft.com/azure/media-services/media-services-portal-get-started-with-aad).

Bunu yaptıktan sonra iki farklı şekilde video denetimi Medya işleyicisi kullanabilirsiniz.

## <a name="use-azure-media-services-explorer"></a>Azure Media Services Gezgini kullanma

Azure Media Services Gezgin AMS için kullanıcı dostu bir ön uç ' dir. AMS hesabınıza gidin, karşıya video yükleme ve Content Moderator Medya işleyicisi ile içerik taraması için kullanın. Buradan indirip [GitHub](https://github.com/Azure/Azure-Media-Services-Explorer/releases), veya [Azure Media Services Gezgin blog gönderisi](https://azure.microsoft.com/blog/managing-media-workflows-with-the-new-azure-media-services-explorer-tool/) daha fazla bilgi için.

![Content Moderator ile Azure Media Services Gezgini](images/ams-explorer-content-moderator.PNG)

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

1. Visual Studio'da yeni bir oluşturma **konsol uygulaması (.NET Framework)** adlandırın ve proje **VideoModeration**. 
1. Çözümünüzde başka projeler de varsa, tek başlangıç projesi olarak bunu seçin.
1. Gereken NuGet paketlerini alın. Çözüm Gezgini'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet**'i seçin; ardından aşağıdaki projeleri bulun ve yükleyin:
    - windowsazure.mediaservices
    - windowsazure.mediaservices.extensions

## <a name="add-video-moderation-code"></a>Video denetimi kod ekleyin

Ardından, temel bir içerik moderasyonu senaryosu uygulamak için kodu bu kılavuzdan kopyalayıp projenize yapıştıracaksınız.

### <a name="update-the-programs-using-statements"></a>Programı deyimler kullanarak güncelleştirme

Aşağıdaki `using` deyimlerini _Program.cs_ dosyanızın en üstüne ekleyin.

```csharp
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
```

### <a name="set-up-resource-references"></a>Kaynak başvurularını Ayarla

Aşağıdaki statik alanları _Program.cs_ dosyasındaki **Program** sınıfına ekleyin. Bu alanlar, AMS aboneliğinize bağlanmak için gerekli bilgileri tutun. Bunları, yukarıdaki adımlarda aldığınız değerlerle doldurun. Unutmayın `CLIENT_ID` olduğu **uygulama kimliği** Azure AD uygulamanızı değerini ve `CLIENT_SECRET` "Bu uygulama için oluşturduğunuz VideoModKey" değeridir.

```csharp
// declare constants and globals
private static CloudMediaContext _context = null;
private static CloudStorageAccount _StorageAccount = null;

// Azure Media Services (AMS) associated Storage Account, Key, and the Container that has 
// a list of Blobs to be processed.
static string STORAGE_NAME = "YOUR AMS ASSOCIATED BLOB STORAGE NAME";
static string STORAGE_KEY = "YOUR AMS ASSOCIATED BLOB STORAGE KEY";
static string STORAGE_CONTAINER_NAME = "YOUR BLOB CONTAINER FOR VIDEO FILES";

private static StorageCredentials _StorageCredentials = null;

// Azure Media Services authentication. 
private const string AZURE_AD_TENANT_NAME = "microsoft.onmicrosoft.com";
private const string CLIENT_ID = "YOUR CLIENT ID";
private const string CLIENT_SECRET = "YOUR CLIENT SECRET";

// REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
private const string REST_API_ENDPOINT = "YOUR API ENDPOINT";

// Content Moderator Media Processor Nam
private const string MEDIA_PROCESSOR = "Azure Media Content Moderator";

// Input and Output files in the current directory of the executable
private const string INPUT_FILE = "VIDEO FILE NAME";
private const string OUTPUT_FOLDER = "";

// JSON settings file
private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";

```

Yerel bir video dosyası (en basit durumda) kullanmak istiyorsanız, projeye ekleyin ve yol olarak girin `INPUT_FILE` (yürütme dizine göre olan göreli yolları) değeri.

Oluşturmanız gerekecektir _preset.json_ dosyasını geçerli dizinde ve sürüm numarasını belirtmek için kullanın. Örneğin:

```JSON
{
    "version": "2.0"
}
```

### <a name="load-the-input-videos"></a>Giriş video yükleme

**Ana** yöntemi **Program** sınıfı oluşturur bir Azure medya içeriği ve bir Azure depolama bağlamı (blob depolama alanında videolarınızı olması durumunda). Geri kalan kod, bir yerel klasör, blob ya da bir Azure depolama kapsayıcısı içinde birden çok BLOB'lar video tarar. Tüm seçenekleri diğer kod satırları açıklama satırı yaparak deneyebilirsiniz.

```csharp
// Create Azure Media Context
CreateMediaContext();

// Create Storage Context
CreateStorageContext();

// Use a file as the input.
IAsset asset = CreateAssetfromFile();

// -- OR ---

// Or a blob as the input
// IAsset asset = CreateAssetfromBlob((CloudBlockBlob)GetBlobsList().First());

// Then submit the asset to Content Moderator
RunContentModeratorJob(asset);

//-- OR ----

// Just run the content moderator on all blobs in a list (from a Blob Container)
// RunContentModeratorJobOnBlobs();
```

### <a name="create-an-azure-media-context"></a>Azure medya içeriği oluşturma

**Program** sınıfına aşağıdaki yöntemi ekleyin. AMS ile iletişime izin verecek şekilde AMS kimlik bilgilerinizi kullanır.

```csharp
// Creates a media context from azure credentials
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
```

### <a name="add-the-code-to-create-an-azure-storage-context"></a>Bir Azure depolama bağlamı oluşturmak için kod ekleyin

**Program** sınıfına aşağıdaki yöntemi ekleyin. Depolama kimlik bilgilerinizi oluşturulan depolama bağlamı, blob depolamaya erişmek için kullanın.

```csharp
// Creates a storage context from the AMS associated storage name and key
static void CreateStorageContext()
{
    // Get a reference to the storage account associated with a Media Services account. 
    if (_StorageCredentials == null)
    {
        _StorageCredentials = new StorageCredentials(STORAGE_NAME, STORAGE_KEY);
    }
    _StorageAccount = new CloudStorageAccount(_StorageCredentials, false);
}
```

### <a name="add-the-code-to-create-azure-media-assets-from-local-file-and-blob"></a>Yerel dosya ve blob Azure medya varlıkları oluşturmak için kod ekleyin

Content Moderator medya işleyicisini işler üzerinde çalıştığı **varlıklar** Azure Media Services platformunda içinde.
Bu yöntemler, yerel bir dosya veya ilişkili bir blob varlıklar oluşturun.

```csharp
// Creates an Azure Media Services Asset from the video file
static IAsset CreateAssetfromFile()
{
    return _context.Assets.CreateFromFile(INPUT_FILE, AssetCreationOptions.None); ;
}

// Creates an Azure Media Services asset from your blog storage
static IAsset CreateAssetfromBlob(CloudBlockBlob Blob)
{
    // Create asset from the FIRST blob in the list and return it
    return _context.Assets.CreateFromBlob(Blob, _StorageCredentials, AssetCreationOptions.None);
}
```

### <a name="add-the-code-to-scan-a-collection-of-videos-as-blobs-within-a-container"></a>Bir kapsayıcı içindeki videoları (BLOB) olarak koleksiyonunu taramak için kod ekleyin

```csharp
// Runs the Content Moderator Job on all Blobs in a given container name
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

// Get all blobs in your container
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
```

### <a name="add-the-method-to-run-the-content-moderator-job"></a>Content Moderator işi çalıştırmak için bir yöntem ekleyin

```csharp
// Run the Content Moderator job on the designated Asset from local file or blob storage
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
```

### <a name="add-helper-functions"></a>Yardımcı işlevleri ekleme

Bu yöntemler, Azure Media Services varlığından Content Moderator çıktı dosyası (JSON) indirin ve programın çalışma durumunu konsola oturum açabilmesi adına denetimi işinin durumunu izlemenize yardımcı olması.

```csharp
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
```

### <a name="run-the-program-and-review-the-output"></a>Programı çalıştırma ve çıktıyı gözden geçirme

İçerik denetleme iş tamamlandıktan sonra JSON yanıtı analiz edin. Bu, bu öğelerden oluşur:

- Video bilgilerinin özeti
- **Görüntüleri** olarak "**parçaları**"
- **Anahtar çerçeveler** olarak "**olayları**" ile bir **reviewRecommended "(= true veya false)"** bayrağı temel alarak **yetişkinlere yönelik** ve **Racy** puanları
- **Başlangıç**, **süresi**, **totalDuration**, ve **zaman damgası** "dalgalanmasındaki" olan. Bölen **ölçeği** saniyeler içinde numarasını almak için.
 
> [!NOTE]
> - `adultScore` cinsel açık veya bazı durumlarda yetişkinlere yönelik olarak kabul içeriği olası durum ve tahmin puanı temsil eder.
> - `racyScore` cinsel müstehcen veya bazı durumlarda yetişkin olarak kabul içeriği olası durum ve tahmin puanı temsil eder.
> - `adultScore` ve `racyScore` 0 ile 1 arasındadır. Yüksek puanı, kategori uygun olabilir yüksek modeli tahmin etmektir. Bu önizleme, el ile kodlanmış sonuçları yerine istatistiksel bir model kullanır. Her kategori için gereksinimlerinizi nasıl hizalandığını belirlemek için kendi içeriğe sahip test etmenizi öneririz.
> - `reviewRecommended` true veya false iç puanına göre eşikleri bağlı değil. Müşteriler, bu değeri kullanın veya kendi içerik ilkelere dayalı özel eşikler karar değerlendirmelisiniz.

```json
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
```

## <a name="next-steps"></a>Sonraki adımlar

Nasıl oluşturacağınızı öğrenin [video incelemeleri](video-reviews-quickstart-dotnet.md) , denetimi çıktısından.

Ekleme [döküm denetimi](video-transcript-moderation-review-tutorial-dotnet.md) video, incelemeleri için.

Yapı hakkında ayrıntılı öğretici kullanıma bir [tamamlamak video ve döküm denetimi çözümü](video-transcript-moderation-review-tutorial-dotnet.md).

[Visual Studio çözümü indirme](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer Content Moderator hızlı başlangıçlar için .NET için.
