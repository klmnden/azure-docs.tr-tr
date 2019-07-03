---
title: Azure medya Analizi ile hareketlerin algılamak | Microsoft Docs
description: Azure medya hareket algılayıcısı medya işlemci (MP) faiz yoksa uzun ve olaysız bir video içinde bölümlerini verimli bir şekilde belirlemenizi sağlar.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/19/2019
ms.author: milanga;juliako;
ms.openlocfilehash: e0b083cba575f4d1c0eb19afb76fca29431ae75e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61463540"
---
# <a name="detect-motions-with-azure-media-analytics"></a>Azure medya Analizi ile hareketlerin algılayın
## <a name="overview"></a>Genel Bakış
**Azure ortam hareket algılayıcısı** medya işlemci (MP) faiz yoksa uzun ve olaysız bir video içinde bölümlerini verimli bir şekilde belirlemenizi sağlar. Hareket algılama üzerinde statik kamerası kayıtları, video bölümlerini hareket nerede oluştuğunu belirlemek için kullanılabilir. Bu zaman damgaları ve olayın gerçekleştiği sınırlayıcı bölge ile bir meta veri içeren bir JSON dosyası oluşturur.

Doğru güvenlik video akışları hedeflenen, ilgili olaylar ve hatalı pozitif sonuçları shadows ve ışık değişiklikleri gibi hareket kategorilere ayırmak bu teknoloji kuramıyor. Bu güvenlik uyarıları kamera akışlarından sonsuz ilgisiz olaylarla ilgi dakika uzunluğunda gözetim videolardan ayıklayamayabilir olmanın yanı sıra adresinize gerek kalmadan oluşturmanıza olanak sağlar.

**Azure ortam hareket algılayıcısı** MP şu anda Önizleme aşamasındadır.

Bu makalede, ilgili ayrıntıları verir. **Azure ortam hareket algılayıcısı** ve .NET için Media Services SDK ile kullanma işlemi gösterilmektedir

## <a name="motion-detector-input-files"></a>Hareket algılayıcısı giriş dosyaları
Video dosyaları. Şu anda aşağıdaki biçimleri desteklenir: MP4 MOV ve WMV.

## <a name="task-configuration-preset"></a>Görev yapılandırması (hazır)
Bir görev oluştururken **Azure ortam hareket algılayıcısı**, yapılandırma hazır belirtmeniz gerekir. 

### <a name="parameters"></a>Parametreler
Aşağıdaki parametreleri kullanabilirsiniz:

| Ad | Seçenekler | Açıklama | Varsayılan |
| --- | --- | --- | --- |
| sensitivityLevel |Dize: 'düşük', 'Orta', 'yüksek' |Hangi hareketleri bildirilen duyarlılık düzeyini ayarlar. Hatalı pozitif uyarıların sayısını ayarlamak için bunu ayarlayın. |'Orta' |
| frameSamplingValue |pozitif bir tamsayı |Algoritmayı sıklığında çalışan ayarlar. her kare eşittir 1, 2, her ikinci kare ve benzeri anlamına gelir. |1 |
| detectLightChange |Boole: 'true', 'false' |Işık değişiklikleri, sonuçları raporlanır olup olmadığını ayarlar |'False' |
| mergeTimeThreshold |Xs zamanı: Hh:mm:ss<br/>Örnek: 00:00:03 |2 olay nerede hareket olaylar arasındaki zaman penceresi ve birleştirilmiş 1 bildirilen belirtir. |00:00:00 |
| detectionZones |Algılama bölgeleri dizisi:<br/>-Algılama bölge 3 veya daha fazla puan dizisidir<br/>-Noktası, bir x ve y koordinatı 0'dan 1. |Kullanılacak Çokgen algılama bölgelerinin listesi açıklar.<br/>Sonuçları ile ilk 'id' olan bir kimliği olarak bölgeler ile bildirilir: 0 |Tek bölge tüm çerçeveyi kapsar. |

### <a name="json-example"></a>JSON örneği

```json
    {
      "version": "1.0",
      "options": {
        "sensitivityLevel": "medium",
        "frameSamplingValue": 1,
        "detectLightChange": "False",
        "mergeTimeThreshold":
        "00:00:02",
        "detectionZones": [
          [
            {"x": 0, "y": 0},
            {"x": 0.5, "y": 0},
            {"x": 0, "y": 1}
           ],
          [
            {"x": 0.3, "y": 0.3},
            {"x": 0.55, "y": 0.3},
            {"x": 0.8, "y": 0.3},
            {"x": 0.8, "y": 0.55},
            {"x": 0.8, "y": 0.8},
            {"x": 0.55, "y": 0.8},
            {"x": 0.3, "y": 0.8},
            {"x": 0.3, "y": 0.55}
          ]
        ]
      }
    }
```

## <a name="motion-detector-output-files"></a>Hareket algılayıcısı çıktı dosyaları
Hareket algılama iş çıktı varlığına hareket uyarıların ve bunların kategorilerde videoyu açıklayan bir JSON dosyası döndürür. Dosya saatini ve süresini videoda algılanan hareket hakkında bilgi içerir.

Nesneler (örneğin, bir gözetim video) Hareket halindeki bir video sabit arka planda olduğunda göstergeleri hareket algılayıcısı API sağlar. Hareket algılayıcısı aydınlatma ve gölge değişiklikleri gibi yanlış alarmların azaltılması için çalıştırılır. Geçerli sınırlamalar algoritmaları gece işleme videolar, yarı şeffaf nesnelerin ve küçük nesneleri içerir.

### <a id="output_elements"></a>Çıkış JSON dosyasının öğeleri
> [!NOTE]
> En son sürümde, çıktıyı JSON biçimine değiştirildi ve bazı müşteriler için önemli bir değişiklik gösterebilir.
> 
> 

Aşağıdaki tabloda, çıkış JSON dosyasının öğeleri açıklar.

| Öğe | Açıklama |
| --- | --- |
| Version |Bu Video API'si sürümüne başvurur. Geçerli sürüm 2'dir. |
| Timescale |Videoyu saniye başına "ticks". |
| Offset |Tarih damgası "saat döngüsü." içindeki saati uzaklığı Video API'leri 1.0 sürümünde, bu her zaman 0 olacaktır. Gelecekte senaryoları destekliyoruz, bu değeri değiştirebilirsiniz. |
| Framerate |Videodaki saniye başına kare hızı. |
| Width, Height |Piksellerde görüntü yüksekliğini ve genişliğini belirtir. |
| Start |"Tick" cinsinden başlangıç zaman damgası. |
| Duration |"Ticks" cinsinden uzunluğu. |
| Interval |Her giriş olayı, "ticks" aralığı. |
| Events |Her olay parça bu süre içinde algılanan hareket içerir. |
| Type |Geçerli sürümde, bu her zaman genel hareket ' 2' dir. Bu etiket verir kategorilere ayırmak için Video API'lerini esnekliği gelecek sürümleri Ara. |
| RegionID |Yukarıda açıklandığı gibi bu her zaman bu sürümde 0 olur. Bu etiket, Video API'si gelecekteki sürümlerde çeşitli bölgelerdeki hareket bulmak için esnekliği sunar. |
| Regions |Videonuzu hareket hakkında burada ilgilendiğiniz alanı ifade eder. <br/><br/>-"id" temsil Bölge alanı – bu sürümde yalnızca bir tane olduğunu, 0 kimliği. <br/>-"tür", önem verdiğiniz hareket bölge şeklini temsil eder. Şu anda "rectangle" ve "Çokgen" desteklenir.<br/> "Rectangle" belirttiyseniz, bölge, X boyuta sahiptir. Y, genişlik ve yükseklik. X ve Y koordinatları 0,0-1,0 normalleştirilmiş ölçeğini bölgede üst sol XY koordinatları temsil eder. Genişlik ve yükseklik 0.0 ile 1.0 normalleştirilmiş ölçeğini bölgede boyutunu temsil eder. Geçerli sürümde X, Y, genişlik ve yükseklik her zaman sabittir 0, 0 ve 1, 1. <br/>"Çokgen" belirttiyseniz, bölge boyutları noktaları vardır. <br/> |
| Fragments |Meta verileri ayarlama parçaları olarak adlandırılan farklı parçalara öbekli. Her parçada başlangıç, süre, aralık sayısı ve olaylar vardır. Olay içermeyen bir parça, başlangıç zamanını ve süresini sırasında hiçbir hareket algılandı anlamına gelir. |
| Köşeli ayraçlar] |Her köşeli ayraç olayda bir aralığı temsil eder. Boş köşeli ayraçlar için hiçbir hareket anlamına bu aralığın algılandı. |
| locations |Bu yeni giriş olayları altında hareketin gerçekleştiği konum listeler. Bu algılama bölgeleri daha fazla özgüdür. |

Aşağıdaki JSON örnek çıktı gösterilmektedir:

```json
    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
```

## <a name="limitations"></a>Sınırlamalar
* Giriş desteklenen video biçimleri MP4 MOV ve WMV içerir.
* Hareket algılama, sabit bir arka plan videolar için optimize edilmiştir. Algoritma aydınlatma değişiklikleri ve gölge gibi yanlış alarm azaltmak üzerinde odaklanır.
* Teknik güçlükler nedeniyle bazı hareket algılanamayabilir; Örneğin, gece işleme videoları, yarı şeffaf nesnelerin ve küçük nesneleri.

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki program gösterir nasıl yapılır:

1. Bir varlık oluşturun ve varlığa bir medya dosyası yükleyin.
2. Aşağıdaki json hazır içeren bir yapılandırma dosyasını temel alan bir video hareket algılama görev ile bir iş oluşturun: 
   
    ```json
            {
            "Version": "1.0",
            "Options": {
                "SensitivityLevel": "medium",
                "FrameSamplingValue": 1,
                "DetectLightChange": "False",
                "MergeTimeThreshold":
                "00:00:02",
                "DetectionZones": [
                [
                    {"x": 0, "y": 0},
                    {"x": 0.5, "y": 0},
                    {"x": 0, "y": 1}
                ],
                [
                    {"x": 0.3, "y": 0.3},
                    {"x": 0.55, "y": 0.3},
                    {"x": 0.8, "y": 0.3},
                    {"x": 0.8, "y": 0.55},
                    {"x": 0.8, "y": 0.8},
                    {"x": 0.55, "y": 0.8},
                    {"x": 0.3, "y": 0.8},
                    {"x": 0.3, "y": 0.55}
                ]
                ]
            }
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

namespace VideoMotionDetection
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

            // Run the VideoMotionDetection job.
            var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                        @"C:\supportFiles\VideoMotionDetection\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
        }

        static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My Video Motion Detection Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Video Motion Detection Job");

            // Get a reference to Azure Media Motion Detector.
            string MediaProcessorName = "Azure Media Motion Detector";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Video Motion Detection Output Asset", AssetCreationOptions.None);

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
[Azure Media Services hareket algılayıcısı blogu](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services Analizi'ne genel bakış](media-services-analytics-overview.md)

[Azure medya analizi tanıtımları](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

