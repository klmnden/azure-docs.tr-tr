---
title: "Azure medya Analizi ile hareketlerin algılamak | Microsoft Docs"
description: "Azure Media hareket algılayıcısı medya işlemcisi (MP), aksi takdirde uzun ve olaysız video içinde ilgi bölümleri verimli bir şekilde tanımlamak sağlar."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: d144f813-1a55-442f-a895-5c4cb6d0aeae
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/31/2017
ms.author: milanga;juliako;
ms.openlocfilehash: 115ad9dfd88062f23d5d17eed8897ce5d2ca8484
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="detect-motions-with-azure-media-analytics"></a>Azure medya Analizi ile hareketlerin Algıla
## <a name="overview"></a>Genel Bakış
**Azure medya hareket algılayıcısı** medya işlemcisi (MP), aksi takdirde uzun ve olaysız video içinde ilgi bölümleri verimli bir şekilde tanımlamak sağlar. Hareket algılama statik kamera görüntülerinin video hareket oluştuğu bölümlerini belirlemek için kullanılabilir. Zaman damgaları ve olayın gerçekleştiği sınırlayıcı bölge ile meta verileri içeren bir JSON dosyası oluşturur.

Doğru güvenlik video akışları hedeflenen, bu teknolojiyi ilgili olayları ve hatalı pozitif sonuç gölgeleri ve aydınlatma değişiklikleri gibi hareket kategorilere yapabiliyor. Bu, güvenlik uyarıları sonsuz ilgisiz olaylarıyla aşırı uzun gözetleme videoların ilgi dakika ayıklayın kullanabilmeye devam ederken adresinize olmadan kamera Akışları'oluşturmanıza olanak sağlar.

**Azure medya hareket algılayıcısı** MP şu anda önizlemede.

Bu konu hakkında ayrıntılar verir **Azure medya hareket algılayıcısı** ve .NET için Media Services SDK'sı ile kullanmak nasıl gösterir

## <a name="motion-detector-input-files"></a>Hareket algılayıcısı giriş dosyaları
Video dosyaları. Şu anda aşağıdaki biçimlerden desteklenir: MP4, MOV ve WMV.

## <a name="task-configuration-preset"></a>Görev yapılandırması (hazır)
Bir görev oluştururken **Azure medya hareket algılayıcısı**, bir yapılandırma hazır belirtmeniz gerekir. 

### <a name="parameters"></a>Parametreler
Şu parametreleri kullanabilirsiniz:

| Ad | Seçenekler | Açıklama | Varsayılan |
| --- | --- | --- | --- |
| sensitivityLevel |Dize: 'düşük', 'Orta', 'yüksek' |Hangi hareketlerin duyarlılık düzeyinde bildirilen ayarlar. Hatalı pozitif sonuç miktarını ayarlamak için bunu ayarlayın. |'Orta' |
| frameSamplingValue |Pozitif tamsayı |Algoritmayı sıklığında çalışan ayarlar. Her çerçeve eşittir 1, 2 her 2 çerçeve ve benzeri anlamına gelir. |1 |
| detectLightChange |Boolean: 'true', 'false' |Hafif değişiklikleri sonuçlarında bildirilen olup olmadığını belirler |'False' |
| mergeTimeThreshold |Xs zamanı: Ss: dd:<br/>Örnek: 00:00:03 |Burada 2 olayları birleştirilmiş ve olması 1 bildirilen hareket olaylar arasındaki zaman penceresi belirtir. |00:00:00 |
| detectionZones |Algılama bölgeleri dizisi:<br/>-3 veya daha fazla noktalar dizisi algılama bölgedir<br/>-Noktasıdır x ve y koordinat 0'dan 1. |Kullanılacak Çokgen algılama bölgelerinin listesi açıklar.<br/>Sonuçları ilk olan 'ID' ile bir kimliği olarak bölge ile bildirilir: 0 |Tüm çerçeve kapsayan tek bir bölge. |

### <a name="json-example"></a>JSON örneği
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


## <a name="motion-detector-output-files"></a>Hareket algılayıcısı çıktı dosyaları
Hareket algılama işi hareket uyarılar ve video içinde kendi kategorilerine tanımlayan çıkış varlığına bir JSON dosyası döndürür. Dosyanın saatini ve süresini videoda algılanan hareket hakkında bilgi içerir.

Bir sabit arka planda video (örn. bir izleme video) hareket nesneleri olduğunuzda hareket algılayıcısı API göstergeleri sağlar. Hareket algılayıcısı aydınlatma ve gölge değişiklikleri gibi yanlış alarmlar azaltmak için eğitildi. Geçerli sınırlamalar algoritmalarının gece görme videoları, yarı saydam nesneleri ve küçük nesneleri içerir.

### <a id="output_elements"></a>Çıkış JSON dosyasının öğeleri
> [!NOTE]
> En son sürümdeki çıkış JSON biçimi değişti ve bazı müşteriler için önemli bir değişiklik gösterebilir.
> 
> 

Aşağıdaki tabloda çıkış JSON dosyasının öğelerini açıklar.

| Öğesi | Açıklama |
| --- | --- |
| Sürüm |Video API sürümüne başvuruyor. Geçerli sürüm 2'dir. |
| Zaman Çizelgesi |Videonun saniyede "çizgilerine". |
| Uzaklık |Veritabanındaki tarih damgası "çizgilerine" zaman uzaklığı. Video API'leri 1.0 sürümünde, bu her zaman 0 olacaktır. Gelecekte destekliyoruz senaryoları, bu değeri değiştirebilirsiniz. |
| Kare hızı |Videonun Saniyedeki çerçeve sayısı. |
| Genişlik, Yükseklik |Genişlik ve yükseklik piksel cinsinden videonun ifade eder. |
| Başlatma |Başlangıç zaman damgasına "çizgilerine". |
| Süre |Olay, "çizgilerine" uzunluğu. |
| aralığı |Her giriş olayda "çizgilerine" aralığı. |
| Olaylar |Her olay parça bu süre içinde algılanan hareket içerir. |
| Tür |Geçerli sürümde, bu her zaman genel hareket ' 2' dir. Bu etiket verir Video kategorilere ayırmak için API esnekliği gelecekteki sürümleri hareket. |
| RegionID |Yukarıda açıklandığı şekilde, bu her zaman bu sürümde 0 olacaktır. Bu etiket Video API gelecekteki sürümlerinde çeşitli bölgelerdeki hareket bulmak için esneklik sunar. |
| Bölgeler |Hareket hakkında burada verdiğiniz videonuzu alanına başvuruyor. <br/><br/>-"id" temsil Bölge alanı – bu sürümde yalnızca bir olduğundan, kimliği 0. <br/>-"tür" önem verdiğiniz hareket bölge şekli temsil eder. Şu anda, "dikdörtgen" ve "Çokgen" desteklenir.<br/> "Dikdörtgen" belirtilmişse, bölge boyutu X, vardır. Y, genişlik ve yükseklik. X ve Y koordinatları 0.0 ile 1.0 normalleştirilmiş ölçeğini bölgede üst sol XY koordinatları temsil eder. Genişlik ve yükseklik 0.0 ile 1.0 normalleştirilmiş ölçeğini bölgede boyutunu temsil eder. Geçerli sürümde X, Y, genişlik ve yükseklik her zaman giderilen 0, 0 ve 1, 1. <br/>"Çokgen" belirtilmişse, bölge boyutları noktalar vardır. <br/> |
| Parçaları |Meta verileri ayarlama parçaları olarak adlandırılan farklı kesimler halinde öbekli. Her parça başlangıç, süre, aralığı sayısı ve olay içerir. Hiçbir olay ile bir parça yok hareket bu başlangıç saatini ve süresini sırasında algılandı anlamına gelir. |
| Köşeli ayraçlar] |Her köşeli ayraç olay bir aralığa temsil eder. Boş köşeli ayraçlar hiçbir hareket anlamına bu aralık için algılandı. |
| Konumları |Bu yeni girişi olayları altında hareket gerçekleştiği konum listeler. Bu algılama bölgeleri daha fazla özeldir. |

JSON çıkış örnek verilmiştir

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

    …
## <a name="limitations"></a>Sınırlamalar
* Desteklenen giriş video biçimleri MP4, MOV ve WMV içerir.
* Hareket algılama sabit arka plan videolar için optimize edilmiştir. Algoritma aydınlatma değişiklikleri ve gölgeleri gibi yanlış alarmlar azaltma odaklanır.
* Bazı hareket nedeniyle teknik zorluklar algılanamayabilir; Örneğin gece görme videoları, yarı saydam nesneler ve küçük nesneleri.

## <a name="net-sample-code"></a>.NET örnek kod

Aşağıdaki program gösterir nasıl yapılır:

1. Bir varlık oluşturun ve varlığa bir medya dosyasını yükleyin.
2. Aşağıdaki json hazır içeren bir yapılandırma dosyasına dayalı bir video hareket algılama görev ile bir iş oluşturun. 
   
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
3. Çıkış JSON dosyalarını indirin. 

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

    namespace VideoMotionDetection
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
                task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);

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
[Azure Media Services hareket algılayıcısı blogu](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services Analytics a genel bakış](media-services-analytics-overview.md)

[Azure medya analizi gösterileri](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

