---
title: Azure medya analizi OCR metinle dijitalleştirerek | Microsoft Docs
description: Azure medya analizi OCR (optik karakter tanıma), metin içerikli videoları düzenlenebilir, aranabilir dijital metne dönüştürün olanak tanır.  Bu, anlamlı meta verileri ayıklama, medya video sinyalinden otomatik hale getirmenizi sağlar.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 307c196e-3a50-4f4b-b982-51585448ffc6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 91fad34073d7505c596bedfb6c93946ee7393dd7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60825617"
---
# <a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Azure medya analizi, metin içerikli videoları dijital metne dönüştürmek için kullanın  
## <a name="overview"></a>Genel Bakış
Video dosyanızdan metin içeriği ayıklama ve düzenlenebilir, aranabilir bir dijital metin oluşturmak ihtiyacınız varsa, Azure medya analizi OCR (optik karakter tanıma) kullanmanız gerekir. Bu Azure medya işlemci video dosyalarınızı metin içeriğini algılar ve kullanmanız için metin dosyaları oluşturur. OCR, anlamlı meta verileri ayıklama, medya video sinyalinden otomatik hale getirmenizi sağlar.

Bir arama motoru ile birlikte kullanıldığında, kolayca medyanızı metin dizin ve içeriğinizin bulunabilirliği geliştirin. Bu yüksek oranda metinsel videoda, bir video kaydı veya ekran yakalama slayt gösterisi sunu gibi son derece kullanışlıdır. Azure OCR medya işleyicisini dijital metin için optimize edilmiştir.

**Azure medya OCR** medya işlemci şu anda Önizleme aşamasındadır.

Bu makalede, ilgili ayrıntıları verir. **Azure medya OCR** ve .NET için Media Services SDK ile kullanma işlemi gösterilmektedir. Daha fazla bilgi ve örnekler için bkz. [bu blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

## <a name="ocr-input-files"></a>OCR giriş dosyaları
Video dosyaları. Şu anda aşağıdaki biçimleri desteklenir: MP4 MOV ve WMV.

## <a name="task-configuration"></a>Görev yapılandırması
Görev yapılandırması (hazır). Bir görev oluştururken **Azure medya OCR**, JSON veya XML kullanarak hazır bir yapılandırma belirtmeniz gerekir. 

>[!NOTE]
>OCR altyapısına maksimum 32000 piksele bir görüntü bölgesi en düşük 40 piksel ile hem yükseklik/genişlik içinde geçerli bir giriş olarak yalnızca kullanır.
>

### <a name="attribute-descriptions"></a>Öznitelik açıklamaları
| Öznitelik adı | Açıklama |
| --- | --- |
|AdvancedOutput| JSON çıkışını AdvancedOutput true olarak ayarlarsanız, her tek bir sözcük (ek olarak ifadeleri ve bölgeler gibi) için konumsal veri içerir. Bu ayrıntıları görmesini istemiyorsanız bayrağı false olarak ayarlayın. Varsayılan değer false'tur. Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/) bakın.|
| Dil |Aranacak metin dili (isteğe bağlı) açıklar. Aşağıdakilerden biri: Otomatik Algıla (varsayılan), Arapça, ChineseSimplified, ChineseTraditional, Çekçe Danca, Felemenkçe, İngilizce, Fince, Fransızca, Almanca, Yunanca, Macarca, İtalyanca, Japonca, Korece, Norveççe, Lehçe, Portekizce, Rumence, Rusça, SerbianCyrillic, SerbianLatin , Slovakya, İspanyolca, İsveççe, Türkçe. |
| TextOrientation |(isteğe bağlı) aranacak metnin yönünü açıklar.  "Sol" üst tüm harfleri gösterilen sola doğru anlamına gelir.  Varsayılan metni (örneğin, bir kitap bulunabilir) "Yukarı" yönlendirilmiş çağrılabilir.  Aşağıdakilerden biri: Otomatik Algıla (varsayılan), en fazla, doğru aşağı, sola. |
| TimeInterval |(isteğe bağlı) örnekleme hızını açıklar.  1/2 saniyede varsayılandır.<br/>JSON biçimi – SS. SSS (varsayılan 00:00:00.500)<br/>XML biçimi – W3C XSD süresi temel (varsayılan PT0.5) |
| DetectRegions |(isteğe bağlı) Metin algılamak üzere video çerçevesinde bölgeleri belirterek DetectRegion nesnelerin dizisi.<br/>Aşağıdaki dört tamsayı değerlerini DetectRegion nesne yapılır:<br/>Sol – sol kenar boşluğunu piksel<br/>Üst – üst kenar boşluğu piksel<br/>Genişlik: bölge piksel cinsinden genişliği<br/>Yükseklik – bölge piksel cinsinden yüksekliği |

#### <a name="json-preset-example"></a>Örnek JSON hazır

```json
    {
        "Version":1.0, 
        "Options": 
        {
            "AdvancedOutput":"true",
            "Language":"English", 
            "TimeInterval":"00:00:01.5",
            "TextOrientation":"Up",
            "DetectRegions": [
                    {
                       "Left": 10,
                       "Top": 10,
                       "Width": 100,
                       "Height": 50
                    }
             ]
        }
    }
```

#### <a name="xml-preset-example"></a>Önceden oluşturulmuş XML örneği

```xml
    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""https://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""https://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""https://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <AdvancedOutput>true</AdvancedOutput>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>10</Left>
                   <Top>10</Top>
                   <Width>100</Width>
                   <Height>50</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>
```

## <a name="ocr-output-files"></a>OCR çıktı dosyaları
Çıkış OCR medya işlemcisi bir JSON dosyasıdır.

### <a name="elements-of-the-output-json-file"></a>Çıkış JSON dosyasının öğeleri
Video OCR çıkış videonuza bulunan karakterleri zaman bölümlenmiş verileri sağlar.  Dil veya yönü gibi öznitelikleri hone çözümlenmesinde ilgilenen sözcükler için kullanabilirsiniz. 

Çıktı aşağıdaki öznitelikleri içerir:

| Öğe | Açıklama |
| --- | --- |
| Timescale |Videoyu saniye başına "ticks" |
| Offset |zaman damgaları için uzaklık. Video API'leri 1.0 sürümünde, bu her zaman 0 olacaktır. |
| Framerate |Saniyedeki kare video sayısı |
| Genişlik |videonun piksel cinsinden genişliği |
| Yükseklik |piksel cinsinden görüntü yüksekliği |
| Fragments |içine meta veriler öbekli video öbekleri zamana bağlı bir dizi |
| start |Başlangıç saati "ticks" parçasında |
| Süresi |"ticks" parçasında uzunluğu |
| interval |verilen parça içindeki her bir olay zaman aralığı |
| etkinlikler |bölgeleri içeren bir dizi |
| bölge |sözcük ve tümcecikleri temsil eden nesne algılandı |
| language |bir bölge içinde algılanan metnin dilini |
| Yönlendirme |bir bölge içinde algılanan metnin yönünü |
| satırları |bir bölge içinde algılanan metin satırlarını dizisi |
| metin |Gerçek metniniz |

### <a name="json-output-example"></a>JSON çıkış örneği
Aşağıdaki örnek çıktı, video genel bilgiler ve çeşitli video parçalarını içerir. Her video parçasında, dil ve onun metin hizalamasını ile OCR MP tarafından algılanan her bölge içerir. Bölge, her sözcük satırı satırın metin, satırın konumunu ve bu satırdaki her bir sözcük bilgi (word içerik, konumunu ve güvenle) ile bu bölgede de içerir. Bir örnek verilmiştir ve ben bazı açıklamalar satır içi yerleştirin.

```json
    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }
```

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki program gösterir nasıl yapılır:

1. Bir varlık oluşturun ve varlığa bir medya dosyası yükleyin.
2. Bir iş, bir OCR yapılandırma/hazır dosyası oluşturun.
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

namespace OCR
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

            // Run the OCR job.
            var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                        @"C:\supportFiles\OCR\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
        }

        static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My OCR Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My OCR Job");

            // Get a reference to Azure Media OCR.
            string MediaProcessorName = "Azure Media OCR";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My OCR Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);

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

