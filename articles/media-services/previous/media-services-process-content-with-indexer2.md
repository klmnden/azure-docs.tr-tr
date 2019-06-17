---
title: Azure Media Indexer 2 Önizleme ile medya dosyalarının dizinini oluşturarak | Microsoft Docs
description: Azure Media Indexer, medya dosyalarınızın içeriklerini aranabilir yapmanıza ve Kapalı Açıklamalı Altyazı veya anahtar sözcükler için bir tam metin dökümü oluşturmak için sağlar. Bu konuda, Media Indexer 2 Önizleme kullanma işlemini gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/19/2019
ms.author: adsolank;juliako;
ms.openlocfilehash: 304ecda320e1fdd9573bc961fde74efe03400aa3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64712961"
---
# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Azure Media Indexer 2 Önizleme ile medya dosyalarının dizinini oluşturarak
## <a name="overview"></a>Genel Bakış
**Azure Media Indexer 2 Önizleme** medya işlemci (MP) kapalı açıklamalı alt yazı parçaları oluşturmak yanı sıra medya dosyalarını ve içerik aranabilir yapmanıza olanak tanır. Önceki sürüme kıyasla [Azure Media Indexer'ın](media-services-index-content.md), **Azure Media Indexer 2 Önizleme** dizin daha hızlı gerçekleştirir ve daha geniş bir dil desteği sunar. Desteklenen diller şunlardır: İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Mandarin, Basitleştirilmiş), Portekizce, Arapça, Rusça ve Japonca.

**Azure Media Indexer 2 Önizleme** MP şu anda Önizleme aşamasındadır.

Bu makalede, dizin oluşturma işleri ile oluşturma işlemi gösterilmektedir **Azure Media Indexer 2 Önizleme**.

> [!NOTE]
> Aşağıdaki maddeler geçerlidir:
> 
> Dizin Oluşturucu 2 Azure Çin'de hem de Azure Kamu'da desteklenmiyor.
> 
> İçerik dizinleme, çok konuşma (olmadan, arka plan müzik, parazit, etkileri veya mikrofon hiss) sahip medya dosyalarını kullanan emin olun. Uygun içerik bazı örnekleri şunlardır: kaydedilen toplantılar, dersler ve sunumlar. Aşağıdaki içerik dizinleme için uygun olmayabilir: filmler, TV programları, sonraki her şey karma ses ve ses efektleri ile kötü içeriği (hiss) arka plan gürültüsü ile kaydedilmiş.
> 
> 

Bu makalede, ilgili ayrıntıları verir. **Azure Media Indexer 2 Önizleme** ve .NET için Media Services SDK ile kullanma işlemi gösterilmektedir

## <a name="input-and-output-files"></a>Giriş ve çıkış dosyaları
### <a name="input-files"></a>Giriş dosyaları
Ses veya video dosyaları

### <a name="output-files"></a>Çıktı dosyaları
Bir dizin oluşturma işi kapalı açıklamalı alt yazı dosyaları aşağıdaki biçimlerde oluşturabilirsiniz:  

* **SAMI**
* **TTML**
* **WebVTT**

Bu biçimler dosyalarında ses ve video dosyaları işitme engelli kişiler için erişilebilir hale getirmek için kullanılan açıklamalı alt yazı (CC) kapatıldı.

## <a name="task-configuration-preset"></a>Görev yapılandırması (hazır)
Bir dizin oluşturma, görev ile **Azure Media Indexer 2 Önizleme**, yapılandırma hazır belirtmeniz gerekir.

Kullanılabilir parametrelerin aşağıdaki JSON ayarlar.

```json
    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }
```

## <a name="supported-languages"></a>Desteklenen diller
Azure Media Indexer 2 Önizleme konuşma metin (dil adı köşeli ayraçlar içindeki kullanım 4 karakterli kodu görev yapılandırması aşağıda gösterildiği gibi belirtirken) için aşağıdaki dilleri desteklemektedir:

* İngilizce [en-us]
* İspanyolca [EsEs]
* Çince (Basitleştirilmiş Mandarin,) [ZhCn]
* Fransızca [FrFr]
* Almanca [DeDe]
* İtalyanca [ItIt]
* Portekizce [PtBr]
* Arabic (Egyptian) [ArEg]
* Japonca [JaJp]
* Rusça [RuRu]
* İngiliz İngilizce [EnGb]
* İspanyolca (Meksika) [EsMx] 

## <a name="supported-file-types"></a>Desteklenen dosya türleri

Desteklenen dosya türleri hakkında daha fazla bilgi için bkz. [desteklenen codec'ler/biçimleri](media-services-media-encoder-standard-formats.md#input-containerfile-formats) bölümü.

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki program gösterir nasıl yapılır:

1. Bir varlık oluşturun ve varlığa bir medya dosyası yükleyin.
2. Aşağıdaki json hazır içeren bir yapılandırma dosyasını temel alan bir dizin oluşturma görevini içeren bir proje oluşturun:

    ```json
            {
            "version":"1.0",
            "Features":
                [
                {
                "Options": {
                        "Formats":["WebVtt","ttml"],
                        "Language":"enUs",
                        "Type":"RecoOptions"
                },
                "Type":"SpReco"
                }]
            }
    ```
    
3. Çıkış dosyalarını indirin. 
   
#### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

#### <a name="example"></a>Örnek

```csharp
using System;
using System.Configuration;
using System.IO;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Threading;
using System.Threading.Tasks;

namespace IndexContent
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

            // Run indexing job.
            var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                        @"C:\supportFiles\Indexer\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
        }

        static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My Indexing Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Indexing Job");

            // Get a reference to Azure Media Indexer 2 Preview.
            string MediaProcessorName = "Azure Media Indexer 2 Preview";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Indexing Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset to be indexed.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

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
```

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analizi'ne genel bakış](media-services-analytics-overview.md)

[Azure medya analizi tanıtımları](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

