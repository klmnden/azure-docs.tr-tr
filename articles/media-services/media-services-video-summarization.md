---
title: "Video özetleme oluşturmak için Azure medya Video küçük resimleri kullanın | Microsoft Docs"
description: "Video özetleme otomatik olarak kaynak video ilginç parçacıkları'i seçerek uzun videoları özetlerini oluşturmanıza yardımcı olabilir. Bu, uzun bir video beklenmesi gerekenler hızlı bir bakış sağlamak istediğinizde yararlıdır."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: a245529f-3150-4afc-93ec-e40d8a6b761d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2017
ms.author: milanga;juliako;
ms.openlocfilehash: 92c730addb69bc4d12708ccd789edce0c2336c80
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="use-azure-media-video-thumbnails-to-create-a-video-summarization"></a>Video özetleme oluşturmak için Azure medya Video küçük resimleri kullanın
## <a name="overview"></a>Genel Bakış
**Azure medya Video küçük resimleri** medya işlemcisi (MP), uzun bir video özetini Önizleme istediğiniz müşteriler için kullanışlı bir video özetini oluşturmanıza olanak sağlar. Örneğin, müşteriler kısa bir "özeti video" görmek isteyebilirsiniz küçük üzerine bunlar vurgulu zaman. Parametreleri uyguladıkça tarafından **Azure medya Video küçük resimleri** yapılandırma hazır MP'nin güçlü görüntüsü algılama ve birleştirme teknolojisi algorithmically açıklayıcı subclip oluşturmak için kullanabilirsiniz.  

**Azure medya Video küçük** MP şu anda önizlemede.

Bu makalede, ilgili ayrıntıları verir **Azure medya Video küçük** ve Media Services SDK'sı ile .NET için nasıl kullanılacağını gösterir.

## <a name="limitations"></a>Sınırlamalar

Videonuzu farklı perde, oluşan değil, bazı durumlarda, çıktı yalnızca tek bir görüntüsü.

## <a name="video-summary-example"></a>Video özeti örneği
Azure medya Video küçük resimleri medya işlemcisi yapabileceklerine ilişkin bazı örnekler şunlardır:

### <a name="original-video"></a>Özgün video
[Özgün video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Faed33834-ec2d-4788-88b5-a4505b3d032c%2FMicrosoft%27s%20HoloLens%20Live%20Demonstration.ism%2Fmanifest)

### <a name="video-thumbnail-result"></a>Video küçük resim sonucu
[Video küçük resim sonucu](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Ff5c91052-4232-41d4-b531-062e07b6a9ae%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

## <a name="task-configuration-preset"></a>Görev yapılandırması (hazır)
Video küçük resim görev oluştururken **Azure medya Video küçük resimleri**, bir yapılandırma hazır belirtmeniz gerekir. Yukarıdaki küçük örnek aşağıdaki temel JSON yapılandırma ile oluşturuldu:

    {"version":"1.0"}

Şu anda, aşağıdaki parametreleri değiştirebilirsiniz:

| Param | Açıklama |
| --- | --- |
| outputAudio |Sonuç video, ses içerip içermeyeceğini belirtir. <br/>İzin verilen değerler: True veya False. Varsayılan true'dur. |
| fadeInFadeOut |Belirtir desteklemediğini yavaşça geçişleri ayrı hareket küçük resimleri arasında kullanılır.  <br/>İzin verilen değerler: True veya False.  Varsayılan true'dur. |
| maxMotionThumbnailDurationInSecs |Tüm sonuç video ne kadar süreyle olmayacağını belirten tamsayı.  Varsayılan özgün video süresine bağlıdır. |

Varsayılan süre aşağıdaki tabloda açıklanmaktadır, ne zaman **maxMotionThumbnailInSecs** kullanılmaz.

|  |  |  |
| --- | --- | --- | --- | --- |
| Video süresi |d < 3 min |3 min < d < 15 dakika |
| Küçük resim süresi |15 saniye (2-3 planda) |30 saniye (3-5 planda) |

Aşağıdaki JSON kullanılabilir parametreleri ayarlar.

    {
        "version": "1.0",
        "options": {
            "outputAudio": "true",
            "maxMotionThumbnailDurationInSecs": "10",
            "fadeInFadeOut": "true"
        }
    }

## <a name="net-sample-code"></a>.NET örnek kod

Aşağıdaki program gösterir nasıl yapılır:

1. Bir varlık oluşturun ve varlığa bir medya dosyasını yükleyin.
2. Aşağıdaki json hazır içeren bir yapılandırma dosyasına dayalı bir video küçük resim görevini içeren bir iş oluşturur: 
   
        {                
            "version": "1.0",
            "options": {
                "outputAudio": "true",
                "maxMotionThumbnailDurationInSecs": "30",
                "fadeInFadeOut": "false"
            }
        }
3. Çıkış dosyaları indirir. 

#### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

#### <a name="example"></a>Örnek

    using System;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Threading;
    using System.Threading.Tasks;

    namespace VideoSummarization
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _AADTenantDomain =
                ConfigurationManager.AppSettings["AMSAADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
                ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
            private static readonly string _AMSClientId =
                ConfigurationManager.AppSettings["AMSClientId"];
            private static readonly string _AMSClientSecret =
                ConfigurationManager.AppSettings["AMSClientSecret"];

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {
                AzureAdTokenCredentials tokenCredentials = 
                    new AzureAdTokenCredentials(_AADTenantDomain,
                        new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                        AzureEnvironments.AzureCloudEnvironment);

                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

                _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);


                // Run the thumbnail job.
                var asset = RunVideoThumbnailJob(@"C:\supportFiles\VideoThumbnail\BigBuckBunny.mp4",
                                            @"C:\supportFiles\VideoThumbnail\config.json");

                // Download the job output asset.
                DownloadAsset(asset, @"C:\supportFiles\VideoThumbnail\Output");
            }

            static IAsset RunVideoThumbnailJob(string inputMediaFilePath, string configurationFile)
            {
                // Create an asset and upload the input media file to storage.
                IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                    "My Video Thumbnail Input Asset",
                    AssetCreationOptions.None);

                // Declare a new job.
                IJob job = _context.Jobs.Create("My Video Thumbnail Job");

                // Get a reference to Azure Media Video Thumbnails.
                string MediaProcessorName = "Azure Media Video Thumbnails";

                var processor = GetLatestMediaProcessorByName(MediaProcessorName);

                // Read configuration from the specified file.
                string configuration = File.ReadAllText(configurationFile);

                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("My Video Thumbnail Task",
                    processor,
                    configuration,
                    TaskOptions.None);

                // Specify the input asset.
                task.InputAssets.Add(asset);

                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("My Video Thumbnail Output Asset", AssetCreationOptions.None);

                // Use the following event handler to check job progress.  
                job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

                // Launch the job.
                job.Submit();

                // Check job execution and wait for job to finish.
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);

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
                    return null;
                }

                return job.OutputMediaAssets[0];
            }

            static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
            {
                IAsset asset = _context.Assets.Create(assetName, options);

                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                assetFile.Upload(filePath);

                return asset;
            }

            static void DownloadAsset(IAsset asset, string outputDirectory)
            {
                foreach (IAssetFile file in asset.AssetFiles)
                {
                    file.Download(Path.Combine(outputDirectory, file.Name));
                }
            }

            static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors
                    .Where(p => p.Name == mediaProcessorName)
                    .ToList()
                    .OrderBy(p => new Version(p.Version))
                    .LastOrDefault();

                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor",
                                                               mediaProcessorName));

                return processor;
            }

            static private void StateChanged(object sender, JobStateChangedEventArgs e)
            {
                Console.WriteLine("Job state changed event:");
                Console.WriteLine("  Previous state: " + e.PreviousState);
                Console.WriteLine("  Current state: " + e.CurrentState);

                switch (e.CurrentState)
                {
                    case JobState.Finished:
                        Console.WriteLine();
                        Console.WriteLine("Job is finished.");
                        Console.WriteLine();
                        break;
                    case JobState.Canceling:
                    case JobState.Queued:
                    case JobState.Scheduled:
                    case JobState.Processing:
                        Console.WriteLine("Please wait...\n");
                        break;
                    case JobState.Canceled:
                    case JobState.Error:
                        // Cast sender as a job.
                        IJob job = (IJob)sender;
                        // Display or log error details as needed.
                        // LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }

        }
    }

### <a name="video-thumbnail-output"></a>Video küçük resim çıkışı
[Video küçük resim çıkışı](http://ampdemo.azureedge.net/azuremediaplayer.html?url=http%3A%2F%2Fnimbuscdn-nimbuspm.streaming.mediaservices.windows.net%2Fd06f24dc-bc81-488e-a8d0-348b7dc41b56%2FHololens%2520Demo_VideoThumbnails_MotionThumbnail.mp4)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analytics a genel bakış](media-services-analytics-overview.md)

[Azure medya analizi gösterileri](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

