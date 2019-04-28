---
title: Media Services ile Azure işlevleri geliştirme
description: Bu konuda, Azure portalını kullanarak Media Services ile Azure işlevleri geliştirmeye başlamak nasıl gösterilmektedir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 51bdcb01-1846-4e1f-bd90-70020ab471b0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 618acae10b874eb5ebd5b6da7fe081368528dbd8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61217528"
---
# <a name="develop-azure-functions-with-media-services"></a>Media Services ile Azure işlevleri geliştirme

Bu makalede, Azure Media Services kullanan işlevler oluşturmaya başlamak gösterilmektedir. Bu makalede bahsedilen Azure işlevi adlı bir depolama hesabı kapsayıcı izler **giriş** yeni MP4 dosyaları için. Bir dosya depolama kapsayıcısına bırakıldıktan sonra blob tetikleyicisi işlevi yürütür. Azure işlevleri gözden geçirmek için bkz: [genel bakış](../../azure-functions/functions-overview.md) ve diğer konularda **Azure işlevleri** bölümü.

Keşfedin ve Azure Media Services kullanan mevcut Azure işlevleri'ni dağıtmak istiyorsanız, kullanıma [medya Hizmetleri Azure işlevleri](https://github.com/Azure-Samples/media-services-dotnet-functions-integration). Bu depo almak için ilgili iş akışlarını doğrudan blob depolama alanından, blob depolama alanına kodlama ve içerik yazma geri içeriğini göstermek için Media Services'ı kullanan örnekler içerir. Ayrıca, Web kancaları ve Azure kuyrukları ile iş bildirimlerini izlemek örnekleri içerir. Örneklerde temel işlevlerinizi da geliştirebilirsiniz [medya Hizmetleri Azure işlevleri](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) depo. İşlevleri dağıtmak için basın **azure'a Dağıt** düğmesi.

## <a name="prerequisites"></a>Önkoşullar

- İlk işlevinizin oluşturmadan önce etkin bir Azure hesabınız olması gerekir. Bir Azure hesabınız yoksa [ücretsiz hesaplar kullanılabilir](https://azure.microsoft.com/free/).
- Azure işlevleri, Azure Media Services (AMS) hesabınızdaki eylemleri gerçekleştirmek veya Media Services tarafından gönderilen olayları dinlemek oluşturmak için kullanacaksanız, açıklandığı bir AMS hesabının oluşturmalısınız [burada](media-services-portal-create-account.md).
    
## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

1. [Azure portalına](https://portal.azure.com) gidip Azure hesabınızla oturum açın.
2. Açıklandığı bir işlev uygulaması oluşturma [burada](../../azure-functions/functions-create-function-app-portal.md).

>[!NOTE]
> Belirttiğiniz bir depolama hesabı **StorageConnection** ortam değişkeni, uygulama ile aynı bölgede olmalıdır (sonraki adıma bakın).

## <a name="configure-function-app-settings"></a>İşlev uygulaması ayarlarını yapılandırma

Media Services işlevleri geliştirirken, işlevlerinizin kullanılacak ortam değişkenleri eklemek için kullanışlıdır. Uygulama ayarlarını yapılandırmak için uygulama ayarlarını yapılandır bağlantısına tıklayın. Daha fazla bilgi için [Azure işlev uygulaması ayarlarını yapılandırmak nasıl](../../azure-functions/functions-how-to-use-azure-function-app-settings.md). 

Bu makalede, tanımlı işlev, aşağıdaki ortam değişkenlerini içinde uygulama ayarlarınızı sahip olduğunuz varsayılır:

**AMSAADTenantDomain**: Azure AD Kiracı uç noktası. AMS API'ye bağlanma hakkında daha fazla bilgi için bkz. [bu](media-services-use-aad-auth-to-access-ams-api.md) makalesi.

**AMSRESTAPIEndpoint**:  REST API uç noktası gösteren URI. 

**AMSClientId**: Azure AD uygulama istemci kimliği.

**AMSClientSecret**: Azure AD uygulama istemci gizli anahtarı.

**StorageConnection**: Media Services hesabıyla ilişkili hesabın depolama bağlantısı. Bu değer kullanılıyor **function.json** dosya ve **run.csx** dosyası (aşağıda açıklanmıştır).

## <a name="create-a-function"></a>İşlev oluşturma

İşlev Uygulamanız dağıtıldıktan sonra arasında bulabilirsiniz **uygulama hizmetleri** Azure işlevleri.

1. İşlev uygulamanızı seçin ve tıklayın **yeni işlev**.
2. Seçin **C#** dil ve **veri işleme** senaryo.
3. Seçin **BlobTrigger** şablonu. Bu işlev, bir blob içine yüklenen her tetiklenir **giriş** kapsayıcı. **Giriş** adı belirtilen **yolu**, sonraki adımda.

    ![dosya görüntüle](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. Seçtiğinizde **BlobTrigger**, diğer bazı denetimler sayfada görüntülenir.

    ![dosya görüntüle](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. **Oluştur**’a tıklayın. 

## <a name="files"></a>Dosyalar

Azure işlevinizi ve bu bölümde açıklanan diğer dosyaları kod dosyaları ile ilişkilidir. Bir işlev oluşturmak için Azure portalı kullandığınızda **function.json** ve **run.csx** sizin için oluşturulur. Ekleme veya karşıya yüklemek gereken bir **project.json** dosya. Bu bölümün geri kalanında her dosya için kısa bir açıklama sunar ve bunların tanımlarının gösterir.

![dosya görüntüle](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a>Function.JSON

Function.json dosyası, işlev bağlamaları ve diğer yapılandırma ayarlarını tanımlar. Çalışma zamanı izlenecek olaylar belirlemek için bu dosya ve verileri aktarmak ve veri işlevi yürütülmesini döndürmek nasıl kullanır. Daha fazla bilgi için [Azure işlevleri HTTP ve Web kancası bağlamaları](../../azure-functions/functions-reference.md#function-code).

>[!NOTE]
>Ayarlama **devre dışı** özelliğini **true** yürütülmekte olan işlevin önlemek için. 

Mevcut function.json dosyasının içeriğini aşağıdaki kodla değiştirin:

```json
{
  "bindings": [
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "input/{filename}.mp4",
      "connection": "ConnectionString"
    }
  ],
  "disabled": false
}
```

### <a name="projectjson"></a>project.json

Project.json dosyası bağımlılıkları içeriyor. İşte bir örnek **project.json** nuget'ten gerekli .NET Azure Media Services paketlerini içeren dosya. En son sürümleri onaylamanız, böylece sürüm numaraları en son güncelleştirmeleri paketler, değiştirmek unutmayın. 

Project.json'yi aşağıdaki tanımını ekleyin. 

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "windowsazure.mediaservices": "4.0.0.4",
        "windowsazure.mediaservices.extensions": "4.0.0.4",
        "Microsoft.IdentityModel.Clients.ActiveDirectory": "3.13.1",
        "Microsoft.IdentityModel.Protocol.Extensions": "1.0.2.206221351"
      }
    }
   }
}

```
    
### <a name="runcsx"></a>Run.csx

Bu C# işleviniz için kod.  İşlev adlı bir depolama hesabı kapsayıcı izleyiciler tanımlanan **giriş** (ne yolunda belirtilen değil) yeni bir MP4 dosyaları için. Bir dosya depolama kapsayıcısına bırakıldıktan sonra blob tetikleyicisi işlevi yürütür.
    
Bu bölümde tanımlanan örnek gösterir 

1. nasıl bir varlık (bir blobu bir AMS varlığa artıştan tarafından), bir Azure Media Services hesabına içe alma ve 
2. bir kodlama işi göndermek için Media Encoder Standard's nasıl kullanır? "Uyarlamalı akış" hazır.

İşin ilerleme durumunu izlemek ve ardından, kodlanmış bir varlık yayımlamak gerçek hayatta senaryosunda, büyük olasılıkla istersiniz. Daha fazla bilgi için [kullanımı Azure Media Services iş bildirimlerini izlemek için Web kancaları](media-services-dotnet-check-job-progress-with-webhooks.md). Daha fazla örnek için bkz. [medya Hizmetleri Azure işlevleri](https://github.com/Azure-Samples/media-services-dotnet-functions-integration).  

Mevcut run.csx dosyasının içeriğini aşağıdaki kodla değiştirin: İşiniz bittiğinde, işlevinizi tanımlama tıklayın **Kaydet ve Çalıştır**.

```csharp
#r "Microsoft.WindowsAzure.Storage"
#r "Newtonsoft.Json"
#r "System.Web"

using System;
using System.Net;
using System.Net.Http;
using Newtonsoft.Json;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.IO;
using System.Web;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.Azure.WebJobs;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
  
// Read values from the App.config file.

static readonly string _AADTenantDomain = Environment.GetEnvironmentVariable("AMSAADTenantDomain");
static readonly string _RESTAPIEndpoint = Environment.GetEnvironmentVariable("AMSRESTAPIEndpoint");
 
static readonly string _mediaservicesClientId = Environment.GetEnvironmentVariable("AMSClientId");
static readonly string _mediaservicesClientSecret = Environment.GetEnvironmentVariable("AMSClientSecret");

static readonly string _connectionString = Environment.GetEnvironmentVariable("ConnectionString");  

private static CloudMediaContext _context = null;
private static CloudStorageAccount _destinationStorageAccount = null;

public static void Run(CloudBlockBlob myBlob, string fileName, TraceWriter log)
{
    // NOTE that the variables {fileName} here come from the path setting in function.json
    // and are passed into the  Run method signature above. We can use this to make decisions on what type of file
    // was dropped into the input container for the function. 

    // No need to do any Retry strategy in this function, By default, the SDK calls a function up to 5 times for a 
    // given blob. If the fifth try fails, the SDK adds a message to a queue named webjobs-blobtrigger-poison.

    log.Info($"C# Blob trigger function processed: {fileName}.mp4");
    log.Info($"Media Services REST endpoint : {_RESTAPIEndpoint}");

    try
    {
        AzureAdTokenCredentials tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain,
                            new AzureAdClientSymmetricKey(_mediaservicesClientId, _mediaservicesClientSecret),
                            AzureEnvironments.AzureCloudEnvironment);
 
        AzureAdTokenProvider tokenProvider = new AzureAdTokenProvider(tokenCredentials);
 
        _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

        IAsset newAsset = CreateAssetFromBlob(myBlob, fileName, log).GetAwaiter().GetResult();

        // Step 2: Create an Encoding Job

        // Declare a new encoding job with the Standard encoder
        IJob job = _context.Jobs.Create("Azure Function - MES Job");

        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Create a task with the encoding details, using a custom preset
        ITask task = job.Tasks.AddNew("Encode with Adaptive Streaming",
            processor,
            "Adaptive Streaming",
            TaskOptions.None); 

        // Specify the input asset to be encoded.
        task.InputAssets.Add(newAsset);

        // Add an output asset to contain the results of the job. 
        // This output is specified as AssetCreationOptions.None, which 
        // means the output asset is not encrypted. 
        task.OutputAssets.AddNew(fileName, AssetCreationOptions.None);

        job.Submit();
        log.Info("Job Submitted");

    }
    catch (Exception ex)
    {
        log.Error("ERROR: failed.");
        log.Info($"StackTrace : {ex.StackTrace}");
        throw ex;
    }
}

private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
{
    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

    if (processor == null)
    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

    return processor;
}

public static async Task<IAsset> CreateAssetFromBlob(CloudBlockBlob blob, string assetName, TraceWriter log){
    IAsset newAsset = null;

    try{
        Task<IAsset> copyAssetTask = CreateAssetFromBlobAsync(blob, assetName, log);
        newAsset = await copyAssetTask;
        log.Info($"Asset Copied : {newAsset.Id}");
    }
    catch(Exception ex){
        log.Info("Copy Failed");
        log.Info($"ERROR : {ex.Message}");
        throw ex;
    }

    return newAsset;
}

/// <summary>
/// Creates a new asset and copies blobs from the specifed storage account.
/// </summary>
/// <param name="blob">The specified blob.</param>
/// <returns>The new asset.</returns>
public static async Task<IAsset> CreateAssetFromBlobAsync(CloudBlockBlob blob, string assetName, TraceWriter log)
{
     //Get a reference to the storage account that is associated with the Media Services account. 
    _destinationStorageAccount = CloudStorageAccount.Parse(_connectionString);

    // Create a new asset. 
    var asset = _context.Assets.Create(blob.Name, AssetCreationOptions.None);
    log.Info($"Created new asset {asset.Name}");

    IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
    TimeSpan.FromHours(4), AccessPermissions.Write);
    ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);
    CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

    // Get the destination asset container reference
    string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];
    CloudBlobContainer assetContainer = destBlobStorage.GetContainerReference(destinationContainerName);

    try{
    assetContainer.CreateIfNotExists();
    }
    catch (Exception ex)
    {
    log.Error ("ERROR:" + ex.Message);
    }

    log.Info("Created asset.");

    // Get hold of the destination blob
    CloudBlockBlob destinationBlob = assetContainer.GetBlockBlobReference(blob.Name);

    // Copy Blob
    try
    {
    using (var stream = await blob.OpenReadAsync()) 
    {            
        await destinationBlob.UploadFromStreamAsync(stream);          
    }

    log.Info("Copy Complete.");

    var assetFile = asset.AssetFiles.Create(blob.Name);
    assetFile.ContentFileSize = blob.Properties.Length;
    assetFile.IsPrimary = true;
    assetFile.Update();
    asset.Update();
    }
    catch (Exception ex)
    {
    log.Error(ex.Message);
    log.Info (ex.StackTrace);
    log.Info ("Copy Failed.");
    throw;
    }

    destinationLocator.Delete();
    writePolicy.Delete();

    return asset;
}
```

## <a name="test-your-function"></a>İşlevinizi sınama

İşlevinizi test etmek için bir MP4 dosyasına karşıya yüklemek gereken **giriş** kapsayıcı bağlantı dizesinde belirtilen depolama hesabı.  

1. Belirtilen depolama hesabı seçin **StorageConnection** ortam değişkeni.
2. Tıklayın **Blobları**.
3. Tıklayın **+ kapsayıcı**. Kapsayıcı adı **giriş**.
4. Tuşuna **karşıya** ve karşıya yüklemek istediğiniz bir .mp4 dosyasına göz atın.

>[!NOTE]
> Blob tetikleyicisi bir tüketim planında kullanırken, olabilir bir 10 dakikaya kadar bir işlev uygulaması boşta geçti sonra yeni BLOB'ları işleme. İşlev uygulaması çalışmaya başladıktan sonra BLOB'ları hemen işlenir. Daha fazla bilgi için [Blob Depolama Tetikleyicileri ve bağlamaları](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob).

## <a name="next-steps"></a>Sonraki adımlar

Bu noktada, Media Services uygulama geliştirmeye başlamak hazırsınız. 
 
Daha fazla ayrıntı ve Azure işlevleri ve Logic Apps özel içerik oluşturma iş akışlarını oluşturmak için Azure Media Services ile kullanma tam samples/çözüm için bkz. [github'da Media Services .NET işlevleri tümleştirme örneği](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)

Ayrıca bkz [kullanımı Azure Media Services .NET ile iş bildirimlerini izlemek için Web kancaları](media-services-dotnet-check-job-progress-with-webhooks.md). 

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

