---
title: "Azure Media Indexer 2 Önizleme ile medya dosyalarını dizin oluşturma | Microsoft Docs"
description: "Azure Media Indexer medya dosyalarınızı içeriğini aranabilir yapmanıza ve kapalı açıklamalı alt yazı ve anahtar sözcükler için tam metin dökümü oluşturmak üzere sağlar. Bu konu, Media Indexer 2 Önizleme kullanmayı gösterir."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 85d25525-a498-44eb-ae3a-2ca5ceb8e53d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: adsolank;juliako;
ms.openlocfilehash: 0afdd1c04e50215a55fb92c70b1210d1f80d8e3f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Azure Media Indexer 2 Önizleme ile medya dosyalarını dizin oluşturma
## <a name="overview"></a>Genel Bakış
**Azure Media Indexer 2 Önizleme** medya işlemcisi (MP) yanı sıra medya dosyaları ve içerik aranabilir yapmanıza kapalı açıklamalı alt yazı parçaları oluşturmak olanak sağlar. Önceki sürümü karşılaştırıldığında [Azure Media Indexer](media-services-index-content.md), **Azure Media Indexer 2 Önizleme** daha hızlı dizin oluşturma işlemi gerçekleştirir ve daha geniş dil desteği sunar. Desteklenen diller, İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Mandarin, Basitleştirilmiş), Portekizce, Arapça ve Japonca içerir.

**Azure Media Indexer 2 Önizleme** MP şu anda önizlemede.

Bu konu ile dizin oluşturma işleri oluşturmak nasıl gösterir **Azure Media Indexer 2 Önizleme**.

> [!NOTE]
> Aşağıdaki maddeler geçerlidir:
> 
> Dizin Oluşturucu 2 Azure Çin ve Azure kamu desteklenmiyor.
> 
> İçerik dizin oluştururken açıkça konuşma (olmadan, arka plan müzik, parazit, efektleri veya mikrofon hiss) sahip medya dosyalarını kullandığınızdan emin olun. Uygun içeriğin bazı örnekleri şunlardır: kaydedilen toplantılar, dersleri veya sunuları. Aşağıdaki içerik dizin oluşturma işlemi için uygun olmayabilir: filmler, TV programları karma ses ve ses efekti ile herhangi bir şey kötü içeriği arka plan gürültü (hiss) ile kaydedilmiş.
> 
> 

Bu konu hakkında ayrıntılar verir **Azure Media Indexer 2 Önizleme** ve .NET için Media Services SDK'sı ile kullanmak nasıl gösterir

## <a name="input-and-output-files"></a>Giriş ve çıkış dosyaları
### <a name="input-files"></a>Giriş dosyaları
Ses veya video dosyaları

### <a name="output-files"></a>Çıkış dosyaları
Bir dizin oluşturma işi kapalı açıklamalı alt yazı dosyaları aşağıdaki biçimlerde oluşturabilirsiniz:  

* **SAMI**
* **TTML**
* **WebVTT**

Bu biçimler dosyalarında ses ve video dosyaları işitme engelli kişiler için erişilebilir hale getirmek için kullanılan açıklamalı alt yazı (CC) kapatıldı.

## <a name="task-configuration-preset"></a>Görev yapılandırması (hazır)
Bir dizin oluşturma, görev ile **Azure Media Indexer 2 Önizleme**, bir yapılandırma hazır belirtmeniz gerekir.

Aşağıdaki JSON kullanılabilir parametreleri ayarlar.

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

## <a name="supported-languages"></a>Desteklenen diller
Azure Media Indexer 2 Önizleme konuşma metin (dil adı görev yapılandırmasında, kullanım 4 karakter kodu köşeli aşağıda gösterildiği gibi belirtirken) için şu dilleri destekler:

* İngilizce [en-us]
* İspanyolca [EsEs]
* Çince (Basitleştirilmiş Mandarin,) [ZhCn]
* Fransızca [FrFr]
* Almanca [DeDe]
* İtalyanca [ItIt]
* Portekizce [PtBr]
* Arapça (Mısır) [ArEg]
* Japonca [JaJp]
* Rusça [RuRu]
* İngiliz İngilizce [EnGb]
* Meksika İspanyolca [EsMx] 

## <a name="supported-file-types"></a>Desteklenen dosya türleri

Desteklenen dosya türleri hakkında daha fazla bilgi için bkz: [desteklenen codec bileşenleri/biçimleri](media-services-media-encoder-standard-formats.md#input-containerfile-formats) bölümü.

## <a name="net-sample-code"></a>.NET örnek kod

Aşağıdaki program gösterir nasıl yapılır:

1. Bir varlık oluşturun ve varlığa bir medya dosyasını yükleyin.
2. Aşağıdaki json hazır içeren bir yapılandırma dosyasına dayalı bir dizin oluşturma görevini içeren bir iş oluşturun.
   
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
3. Çıktı dosyalarını indirin. 
   
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

    namespace IndexContent
    {
        class Program
        {
            // Read values from the App.config file.
            private static readonly string _AADTenantDomain =
                ConfigurationManager.AppSettings["AADTenantDomain"];
            private static readonly string _RESTAPIEndpoint =
                ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

            // Field for service context.
            private static CloudMediaContext _context = null;

            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
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

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analytics a genel bakış](media-services-analytics-overview.md)

[Azure medya analizi gösterileri](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

