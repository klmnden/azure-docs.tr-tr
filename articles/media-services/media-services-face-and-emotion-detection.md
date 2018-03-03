---
title: "Yüz ve duygu Azure medya Analizi ile algılamak | Microsoft Docs"
description: "Bu konu, yüzler ve Azure medya Analizi ile duygular algılamak gösterilmiştir."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5ca4692c-23f1-451d-9d82-cbc8bf0fd707
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2017
ms.author: milanga;juliako;
ms.openlocfilehash: 7a16745fc21d03f81ca6140ace54f84468749364
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a>Yüz ve duygu Azure medya Analizi ile Algıla
## <a name="overview"></a>Genel Bakış
**Azure medya yüz algılayıcısı** medya işlemcisi (MP) sayısı, hareketleri izlemek ve İzleyici katılım ve tepki yüz ifadeleri aracılığıyla bile ölçer olanak sağlar. Bu hizmet, iki özellik içerir: 

* **Yüz algılama**
  
    Yüz algılama bulur ve video içinde İnsan yüzeyleri izler. Birden çok yüzeyleri algılanabilir ve bunların geçici bir JSON dosyası döndürülen zaman ve yer meta verilerle taşırken sonradan izlenmesi. İzleme sırasında obstructed veya kısaca çerçeve bırakın olsa bile kişinin ekranında dolaşma sırada tutarlı kimliği aynı yüz vermek çalışır.
  
  > [!NOTE]
  > Bu hizmet, yüz tanıma gerçekleştirmez. Çerçeve ayrıldığında ya da için obstructed hale bir kişi döndürmeleri zaman uzun yeni bir kimlik verilir.
  > 
  > 
* **Duygu algılama**
  
    Duygu algılama analiz mutluluk, sadness, Korku, öfke ve daha fazlası da dahil olmak üzere algılandı, yüzler birden çok kendini özniteliklerinde döndürür yüz algılama medya işlemcisi isteğe bağlı bir bileşenidir. 

**Azure medya yüz algılayıcısı** MP şu anda önizlemede.

Bu makalede, ilgili ayrıntıları verir **Azure medya yüz algılayıcısı** ve Media Services SDK'sı ile .NET için nasıl kullanılacağını gösterir.

## <a name="face-detector-input-files"></a>Yüz algılayıcısı giriş dosyaları
Video dosyaları. Şu anda aşağıdaki biçimlerden desteklenir: MP4, MOV ve WMV.

## <a name="face-detector-output-files"></a>Yüz algılayıcısı çıktı dosyaları
Yüz algılama ve İzleme API, Yüksek duyarlılık yüz konumu algılama ve en fazla 64 İnsan yüzeyleri bir video algılayabilir izleme sağlar. En iyi sonuçlar tamamen çıplak yüzeyleri sağlayabilir, yan yüz sırasında ve küçük yazıtipleri (küçük veya eşittir 24 x 24 piksel) olarak doğru olmayabilir.

Algılanan ve izlenen yüzeyleri (sol, üst, genişlik ve yükseklik) koordinatları ile döndürülen yazıtipleri, tek tek izlenmesi belirten bir nominal kimlik numarası yanı sıra piksel görüntüdeki konumunu belirten. Yüz kimlik numaraları birden çok kimliği atanan bazı kişiler kaynaklanan tamamen çıplak yüz kaybolduğunda veya çerçevede çakışan koşullarda sıfırlamak fazladır.

## <a id="output_elements"></a>Çıkış JSON dosyasının öğeleri

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

Yüz algılayıcısı (çok büyük alma durumunda olduğu olayları ayrılır) (burada zamana dayalı yığınlar halinde meta veriler bölünmüştür ve yalnızca ihtiyacınız yükleyebilirsiniz) parçalanma ve kesimlere ayırma teknikleri kullanır. Bazı basit hesaplamalar, veri dönüştürme yardımcı olabilir. Örneğin, bir olay 2997 (çizgilerine/sn) bir ölçeğini 6300 (çizgilerine) başlatılan ve kare 29.97 (Çerçeve/sn), ardından hızına:

* Başlat/ölçeği = 2.1 saniye
* X frameRate saniye 63 çerçeveler =

## <a name="face-detection-input-and-output-example"></a>Algılama giriş yüz ve örnek çıkış
### <a name="input-video"></a>Giriş video
[Video girişi](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Görev yapılandırması (hazır)
Bir görev oluştururken **Azure medya yüz algılayıcısı**, bir yapılandırma hazır belirtmeniz gerekir. Yalnızca yüz algılama için aşağıdaki yapılandırma hazır olur.

```json
    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }
```

#### <a name="attribute-descriptions"></a>Öznitelik tanımlarını
| Öznitelik adı | Açıklama |
| --- | --- |
| Mod |Hızlı - hızı, ancak daha az doğru (varsayılan) hızlı işleniyor.|

### <a name="json-output"></a>JSON çıktısını
Aşağıdaki örnek JSON çıktısını kesildi.

```json
    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],
```


## <a name="emotion-detection-input-and-output-example"></a>Giriş ve çıkış duygu algılama örneği
### <a name="input-video"></a>Giriş video
[Video girişi](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Görev yapılandırması (hazır)
Bir görev oluştururken **Azure medya yüz algılayıcısı**, bir yapılandırma hazır belirtmeniz gerekir. JSON tabanlı duygu algılama oluşturmak için aşağıdaki yapılandırma hazır belirtir.

```json
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }
```


#### <a name="attribute-descriptions"></a>Öznitelik tanımlarını
| Öznitelik adı | Açıklama |
| --- | --- |
| Mod |Yazıtipleri: Yalnızca algılama karşılaşıyor.<br/>PerFaceEmotion: her yüz algılama için ayrı ayrı duygu döndür.<br/>AggregateEmotion: Tüm yüzeyleri çerçevesinde dönüş ortalama duygu değerleri. |
| AggregateEmotionWindowMs |AggregateEmotion modunu seçtiyseniz kullanın. Milisaniye cinsinden toplam her sonucu oluşturmak için kullanılan video uzunluğunu belirtir. |
| AggregateEmotionIntervalMs |AggregateEmotion modunu seçtiyseniz kullanın. Toplama sonuçları üretmek için çalıştırılma sıklığını belirtir. |

#### <a name="aggregate-defaults"></a>Birleşik Varsayılanları
Aşağıdaki değerler toplama penceresi ve aralığı ayarlar için önerilir. AggregateEmotionWindowMs AggregateEmotionIntervalMs uzun olmalıdır.

|| Varsayılanları (s) | Max(s) | Min(s) |
|--- | --- | --- | --- |
| AggregateEmotionWindowMs |0.5 |2 |0.25|
| AggregateEmotionIntervalMs |0.5 |1 |0.25|

### <a name="json-output"></a>JSON çıktısını
JSON (kesilmiş) toplama duygu tanıma için çıktı:

```json
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
```

## <a name="limitations"></a>Sınırlamalar
* Desteklenen giriş video biçimleri MP4, MOV ve WMV içerir.
* Algılanabilir yüz boyutu aralığı 24 x 24 için 2048 x 2048 pikseldir. Bu aralık dışında yüzeyleri algılanmaz.
* Her video için döndürülen yüzeyleri en fazla 64 ' dir.
* Bazı yüzeyleri nedeniyle teknik zorluklar algılanamayabilir; Örneğin, çok büyük yüz açıları (poz head) ve büyük kapanması için. Tamamen çıplak ve tamamen yakın yüzler en iyi sonuçlar sahip.

## <a name="net-sample-code"></a>.NET örnek kod

Aşağıdaki program gösterir nasıl yapılır:

1. Bir varlık oluşturun ve varlığa bir medya dosyasını yükleyin.
2. Aşağıdaki json hazır içeren bir yapılandırma dosyasına dayalı yüz algılama görevle ilgili bir iş oluşturun: 

    ```json
            {
                "version": "1.0"
            }
    ```
3. Çıkış JSON dosyalarını indirin. 

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

namespace FaceDetection
{
    class Program
    {
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

            // Run the FaceDetection job.
            var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                        @"C:\supportFiles\FaceDetection\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
        }

        static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My Face Detection Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Detection Job");

            // Get a reference to Azure Media Face Detector.
            string MediaProcessorName = "Azure Media Face Detector";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Detection Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);

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
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analytics a genel bakış](media-services-analytics-overview.md)

[Azure medya analizi gösterileri](http://amslabs.azurewebsites.net/demos/Analytics.html)

