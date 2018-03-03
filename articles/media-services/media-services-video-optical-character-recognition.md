---
title: Azure medya analizi OCR metinle digitize | Microsoft Docs
description: "Azure medya analizi OCR (optik karakter tanıma) video dosyalarında metin içeriği düzenlenebilir, aranabilir dijital metne dönüştürmek etkinleştirir.  Bu, medya video sinyali anlamlı meta veri ayıklama otomatikleştirmenizi sağlar."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 307c196e-3a50-4f4b-b982-51585448ffc6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/09/2017
ms.author: juliako
ms.openlocfilehash: 03b9de7374880cdb2741821edae246bffaf3f921
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Video dosyalarında metin içeriği dijital metne dönüştürmek için Azure medya analizi kullanın
## <a name="overview"></a>Genel Bakış
Video dosyalarından metin içeriği ayıklamak ve düzenlenebilir, aranabilir dijital metin üretmek ihtiyacınız varsa, Azure medya analizi OCR (optik karakter tanıma) kullanmanız gerekir. Bu Azure medya işlemcisi video dosyalarında metin içeriği algılar ve kendi kullanımınız için metin dosyaları oluşturur. OCR medyanızın video sinyali anlamlı meta veri ayıklama otomatikleştirmenizi sağlar.

Bir arama motoru ile birlikte kullanıldığında, kolayca medyanızı metin dizin ve içeriğinizi bulunabilirliğini geliştirir. Bu bir video kaydı veya bir slayt gösterisi sunumu ekran yakalama gibi yüksek oranda metinsel videoda son derece yararlıdır. Azure OCR medya işlemcisi dijital metin için optimize edilmiştir.

**Azure medya OCR** medya işlemcisi şu anda önizlemede.

Bu makalede, ilgili ayrıntıları verir **Azure medya OCR** ve Media Services SDK'sı ile .NET için nasıl kullanılacağını gösterir. Daha fazla bilgi ve örnekler için bkz: [bu blog](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/).

## <a name="ocr-input-files"></a>OCR giriş dosyaları
Video dosyaları. Şu anda aşağıdaki biçimlerden desteklenir: MP4, MOV ve WMV.

## <a name="task-configuration"></a>Görev yapılandırması
Görev yapılandırması (hazır). Bir görev oluştururken **Azure medya OCR**, JSON veya XML kullanarak önceden belirlenmiş bir yapılandırma belirtmeniz gerekir. 

>[!NOTE]
>OCR altyapısı en fazla 32000 piksel olarak en az 40 piksel görüntü bölgesiyle hem yükseklik/genişlik içinde geçerli bir giriş olarak yalnızca alır.
>

### <a name="attribute-descriptions"></a>Öznitelik tanımlarını
| Öznitelik adı | Açıklama |
| --- | --- |
|AdvancedOutput| JSON çıktısını AdvancedOutput true olarak ayarlarsanız, her tek sözcüklük (ek olarak tümcecikleri ve bölgeler) için konumsal veri içermez. Bu ayrıntıları görmek istemiyorsanız bayrağı false olarak ayarlanır. Varsayılan değer false. Daha fazla bilgi için bkz: [bu blog](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/).|
| Dil |Aranacak metin dili (isteğe bağlı) açıklar. Şunlardan biri: Otomatik Algıla (varsayılan), Arapça, ChineseSimplified, ChineseTraditional, Çekçe Danca, Felemenkçe, İngilizce, Fince, Fransızca, Almanca, Yunanca, Macarca, İtalyanca, Japonca, Korece, Norveççe, Lehçe, Portekizce, Rumence, Rusça, SerbianCyrillic, SerbianLatin, Slovakça, İspanyolca, İsveççe, Türkçe. |
| TextOrientation |(isteğe bağlı) aranacak metnin yönünü açıklar.  Tüm harfler üstündeki işaret, sola doğru "Sol" anlamına gelir.  Varsayılan metin (örneğin, kitaptaki bulunan) "Yedekleme" yönlendirilmiş çağrılabilir.  Şunlardan biri: Otomatik Algıla (varsayılan), sağ, aşağı, sol. |
| TimeInterval |(isteğe bağlı) örnekleme hızını açıklar.  1/2 saniyede varsayılandır.<br/>JSON biçimi – SS: dd:. SSS (varsayılan 00:00:00.500)<br/>XML biçiminde – W3C XSD süresi ilkel (varsayılan PT0.5) |
| DetectRegions |(isteğe bağlı) Metin algılamak üzere video çerçevesinde bölgeler belirtme DetectRegion nesnelerinin bir dizisi.<br/>Bir DetectRegion nesnesi aşağıdaki dört tamsayı değerlerini yapılır:<br/>Sol – sol kenar boşluğundan piksel<br/>Üst – üst kenar boşluğundan piksel<br/>Genişlik – bölge piksel cinsinden genişliği<br/>Yükseklik – bölge piksel cinsinden yüksekliği |

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

#### <a name="xml-preset-example"></a>XML hazır örneği

```xml
    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
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
Çıktısı OCR medya işlemcisi bir JSON dosyasıdır.

### <a name="elements-of-the-output-json-file"></a>Çıkış JSON dosyasının öğeleri
Video OCR çıkış videonuzu içinde bulunan karakterleri zaman kesimli veri sağlar.  Dil veya yönlendirme gibi öznitelikleri hone tam olarak çözümlenmesinde ilgilenen sözcükleri açmak için kullanabilirsiniz. 

Çıktı aşağıdaki öznitelikleri içerir:

| Öğe | Açıklama |
| --- | --- |
| Zaman Çizelgesi |videonun saniyede "çizgilerine" |
| Uzaklık |zaman damgaları için uzaklık. Video API'leri 1.0 sürümünde, bu her zaman 0 olacaktır. |
| Kare hızı |Videonun Saniyedeki çerçeve sayısı |
| Genişlik |video piksel cinsinden genişliği |
| Yükseklik |video piksel cinsinden yüksekliği |
| Parçaları |Meta veriler içine öbekli video zamana dayalı parçalarını dizisi |
| start |Başlangıç saati "çizgilerine" parçadaki |
| Süre |"çizgilerine" parçadaki uzunluğu |
| interval |verilen parça içinde her olayın aralığı |
| etkinlikler |bölgeleri içeren bir dizi |
| bölge |nesnesini temsil eden kelimeler ve ifadeler algılandı |
| Dil |bir bölge içinde algılanan metin dili |
| Yönlendirme |bir bölge içinde algılanan metnin yönünü |
| satırları |bir bölge içinde algılanan metin satırı dizisi |
| Metin |gerçek metin |

### <a name="json-output-example"></a>JSON çıkış örneği
Aşağıdaki çıktı örneği genel video bilgi ve birkaç video parçalarını içerir. Video her parçasında, dil ve onun metin hizalamasını OCR MP tarafından algılanan her bölge içerir. Bölge ayrıca her word satır hattın metin, satırın konumunu ve bu satırdaki her bir word bilgileri (word içerik, konum ve güvenilirlik) ile bu bölgede yer alır. Bir örnek verilmiştir ve bazı açıklamalar satır içi yerleştirin.

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

## <a name="net-sample-code"></a>.NET örnek kod

Aşağıdaki program gösterir nasıl yapılır:

1. Bir varlık oluşturun ve varlığa bir medya dosyasını yükleyin.
2. İşi bir OCR yapılandırma/hazır dosyası oluşturun.
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
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analytics a genel bakış](media-services-analytics-overview.md)

