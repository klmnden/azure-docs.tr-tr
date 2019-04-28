---
title: Yüz ve duygu tanıma ile Azure medya analizi | Microsoft Docs
description: Bu konu, yüz ve duyguları Azure medya Analizi ile nasıl gösterir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 5ca4692c-23f1-451d-9d82-cbc8bf0fd707
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: milanga;juliako;
ms.openlocfilehash: 46e60583da79006c133c8d9fac63e27f28bd699f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61217257"
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a>Yüz ve duygu tanıma ile Azure medya analizi algılayın
## <a name="overview"></a>Genel Bakış
**Azure medya yüz algılayıcısı** medya işlemci (MP) sayısı, hareketleri izlemek ve hatta İzleyici katılım ve yüz ifadelerini aracılığıyla tepki ölçer olanak sağlar. Bu hizmet, iki özellik içerir: 

* **Yüz algılama**
  
    Yüz algılama bulur ve video İnsan yüzlerini izler. Birden fazla yüzeye algılanabilir ve bunlar JSON dosyasında döndürülen zaman ve konum meta verileriyle hareket ettirmek sonradan izlenmesi. İzleme sırasında obstructed veya kısaca çerçeve bırakın bile kişi ekranda taşınıyor geçici bir çözüm olsa aynı yüz için tutarlı bir kimlik sağlamak çalışır.
  
  > [!NOTE]
  > Bu hizmet, yüz tanıma gerçekleştirmez. Çerçeve ayrıldığında ya da için obstructed olur bireysel bunlar döndüğünüzde yeni bir kimliği çok uzun sunulur.
  > 
  > 
* **Duygu algılama**
  
    Duygu algılama, birden çok duygusal özniteliklerinde mutluluk, üzüntü, Korku, kızgınlık ve daha fazlası dahil olmak üzere algılanan yüzleri analiz döndüren yüz algılama medya işlemcisi isteğe bağlı bir bileşendir. 

**Azure medya yüz algılayıcısı** MP şu anda Önizleme aşamasındadır.

Bu makalede, ilgili ayrıntıları verir. **Azure medya yüz algılayıcısı** ve .NET için Media Services SDK ile kullanma işlemi gösterilmektedir.

## <a name="face-detector-input-files"></a>Yüz algılayıcısı giriş dosyaları
Video dosyaları. Şu anda aşağıdaki biçimleri desteklenir: MP4 MOV ve WMV.

## <a name="face-detector-output-files"></a>Yüz algılayıcısı çıktı dosyaları
Yüz algılama ve İzleme API, yüksek hassaslıktaki yüz konumu algılama ve en fazla 64 İnsan yüzlerini bir videoda algılayabilir izleme sağlar. En iyi sonuçları tamamen çıplak yüzleri sağlayabilir, yan yüz ve küçük yüzleri (küçüktür veya eşittir 24 x 24 piksel) doğru olmayabilir.

Algılanan ve izlenen yüzleri koordinatları (sol, üst, genişlik ve yükseklik) döndürülen bu kişiye izlemenin belirten bir yüz kimliği sayı yanı sıra piksel görüntüdeki yüzleri konumunu belirten. Yüz Kimliği numaraları atanan birden çok kimlikler bazı kişiler kaynaklanan koşullar altında tamamen çıplak yüz kayıp veya çerçevede çakışan sıfırlama eğilimlidir.

## <a id="output_elements"></a>Çıkış JSON dosyasının öğeleri

[!INCLUDE [media-services-analytics-output-json](../../../includes/media-services-analytics-output-json.md)]

Yüz algılayıcısı (çok büyük aldıkları durumunda nerede olayların ayrılır) (burada meta veriler, zaman tabanlı öbekler halinde bölünmüştür ve yalnızca ihtiyacınız olan indirebilirsiniz) parçalanma ve segmentasyon teknikleri kullanır. Bazı basit hesaplamalar, verileri dönüştürmenize yardımcı olabilir. Örneğin, bir olay 2997 (saat döngüsü/sn) bir ölçeğini 6300 (saat döngüsü) başladıysanız ve ardından 29.97 (çerçeveleri/sn), kare hızı:

* Başlangıç/Ölçek = 2,1 saniye
* X frameRate saniye 63 çerçeveler =

## <a name="face-detection-input-and-output-example"></a>Yüz algılama giriş ve örnek çıktı
### <a name="input-video"></a>Giriş video
[Giriş Video](https://ampdemo.azureedge.net/azuremediaplayer.html?url=httpss%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Görev yapılandırması (hazır)
Bir görev oluştururken **Azure medya yüz algılayıcısı**, yapılandırma hazır belirtmeniz gerekir. Yalnızca yüz algılama için aşağıdaki yapılandırma hazır olur.

```json
    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }
```

#### <a name="attribute-descriptions"></a>Öznitelik açıklamaları
| Öznitelik adı | Açıklama |
| --- | --- |
| Mod |Fast - hızlı, ancak daha az doğru (varsayılan) hızlı işleniyor.|

### <a name="json-output"></a>JSON çıkış
Aşağıdaki örnek JSON çıktı kesildi.

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
[Giriş Video](https://ampdemo.azureedge.net/azuremediaplayer.html?url=httpss%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Görev yapılandırması (hazır)
Bir görev oluştururken **Azure medya yüz algılayıcısı**, yapılandırma hazır belirtmeniz gerekir. JSON tabanlı duygu Algılama işlemi oluşturmak için aşağıdaki yapılandırma hazır belirtir.

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


#### <a name="attribute-descriptions"></a>Öznitelik açıklamaları
| Öznitelik adı | Açıklama |
| --- | --- |
| Mod |Yüzleri: Yalnızca yüz algılama.<br/>PerFaceEmotion: Duygu tanıma bağımsız olarak her yüz algılama için döndürür.<br/>AggregateEmotion: Çerçevede tüm yüzeyleri için ortalama duygu tanıma değerleri döndürür. |
| AggregateEmotionWindowMs |AggregateEmotion modunu seçtiyseniz bu seçeneği kullanın. Milisaniye cinsinden her toplama sonucu oluşturmak için kullanılan video uzunluğunu belirtir. |
| AggregateEmotionIntervalMs |AggregateEmotion modunu seçtiyseniz bu seçeneği kullanın. Toplama sonuçları üretmek için çalıştırılma sıklığını belirtir. |

#### <a name="aggregate-defaults"></a>Toplama Varsayılanları
Aşağıdaki değerleri toplama penceresi ve aralığı ayarları için önerilir. AggregateEmotionWindowMs AggregateEmotionIntervalMs uzun olmalıdır.

|| Varsayılan (s) | Max(s) | Dakika |
|--- | --- | --- | --- |
| AggregateEmotionWindowMs |0,5 |2 |0.25|
| AggregateEmotionIntervalMs |0,5 |1 |0.25|

### <a name="json-output"></a>JSON çıkış
JSON için toplu duygu (kesilmiş) çıktı:

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
* Giriş desteklenen video biçimleri MP4 MOV ve WMV içerir.
* Algılanabilir yüz boyut aralığı 24 x 24 için 2048 x 2048 pikseldir. Bu aralık dışında yüzleri algılanmaz.
* Her video için döndürülen yüz sayısı 64'tür.
* Teknik güçlükler nedeniyle bazı yüzleri algılanamayabilir; Örneğin, çok büyük yüz açıları (poz head) ve büyük kapatma. Tamamen çıplak ve neredeyse tamamen yüzleri en iyi sonucu var.

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki program gösterir nasıl yapılır:

1. Bir varlık oluşturun ve varlığa bir medya dosyası yükleyin.
2. Aşağıdaki json hazır içeren bir yapılandırma dosyasını temel alan bir yüz algılama görev ile bir iş oluşturun: 

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
            task.OutputAssets.AddNew("My Face Detection Output Asset", AssetCreationOptions.None);

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

[Azure medya analizi tanıtımları](https://amslabs.azurewebsites.net/demos/Analytics.html)

