---
title: Azure medya Analizi ile yüzleri özgürlüğü | Microsoft Docs
description: Bu konuda, Azure medya Analizi ile yüzleri özgürlüğü gösterilmiştir.
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
ms.date: 03/18/2019
ms.author: juliako;
ms.openlocfilehash: 1fe003ae13bc5f195932f4f140e17c4dc2791959
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61247397"
---
# <a name="redact-faces-with-azure-media-analytics"></a>Azure medya Analizi ile yüzleri özgürlüğü 
## <a name="overview"></a>Genel Bakış
**Azure Media Redactor** olduğu bir [Azure medya analizi](media-services-analytics-overview.md) medya işlemci (MP) bulutta ölçeklenebilir yüz flulaştırma sunar. Yüz flulaştırma seçilen kişilerin yüzlerini bulanıklaştıran için videonuzu değiştirmenize olanak sağlar. Yüz flulaştırma hizmet kamu güvenliği ve haber medya senaryolarında kullanmak isteyebilirsiniz. El ile özgürlüğü saat birden fazla yüzeye içeren görüntülerini, birkaç dakika sürebilir, ancak bu hizmet ile yalnızca birkaç basit adımda yüz flulaştırma işlemi gerektirir. Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.

Bu makalede, ilgili ayrıntıları verir. **Azure Media Redactor** ve .NET için Media Services SDK ile kullanma işlemi gösterilmektedir.

## <a name="face-redaction-modes"></a>Yüz flulaştırma modları
Aynı tek tek de diğer açıları bulanık, böylece videonun her çerçevede yüzleri algılama ve izleme iletir ve geriye dönük zaman içinde her iki yüz nesnesi tarafından yüz flulaştırma çalışır. Otomatik Redaksiyon işlemi karmaşıktır ve mu değil her zaman üretim % 100'ünü, bu nedenle medya analizi için istenen çıkış sağlar, çeşitli şekillerde son çıktısı değiştirilecek.

Tamamen otomatik bir mod yanı sıra seçimi/sona erdirme olanağı-selection kimliklerinin bir listesi üzerinden bulunan dikdörtgenlerini sağlayan bir iki geçişli iş akışı yok. Ayrıca, çerçeve ayarlamaları MP rastgele hale getirmek için JSON biçiminde bir meta veri dosyası kullanır. Bu iş akışı bölündüğünü **Çözümle** ve **Redact** modu. İki modun hem bir işlemde çalıştırır ve tek bir geçişteki birleştirebilirsiniz. Bu modu olarak adlandırılır **birleştirilmiş**.

### <a name="combined-mode"></a>Birleşik modu
Bu sonuç kısaltılmıştır mp4 otomatik olarak giriş herhangi bir el ile üretir.

| Aşama | Dosya Adı | Notlar |
| --- | --- | --- |
| Giriş varlığı |foo.bar |WMV, MOV veya MP4 biçimindeki video |
| Giriş yapılandırma |İş yapılandırması hazır |{'version':'1.0 ', 'Seçenekler': {'mode': 'birleştirilmiş'}} |
| Çıktı varlığına |foo_redacted.mp4 |Uygulanan Bulanıklaştırma ile video |

#### <a name="input-example"></a>Örnek Giriş:
[Bu videoyu izleyin](https://ampdemo.azureedge.net/?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a>Örnek çıktı:
[Bu videoyu izleyin](https://ampdemo.azureedge.net/?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a>Modu analiz edin
**Analiz** iki geçişli bir iş akışı geçişi video bir girdi alır ve yüz konumları bir JSON dosyası oluşturur ve her birinin jpg görüntüleri yüz algılandı.

| Aşama | Dosya Adı | Notlar |
| --- | --- | --- |
| Giriş varlığı |foo.bar |WMV, MPV veya MP4 biçimindeki video |
| Giriş yapılandırma |İş yapılandırması hazır |{'version':'1.0 ', 'Seçenekler': {'mode': 'çözümleme'}} |
| Çıktı varlığına |foo_annotations.json |Ek açıklama verileri JSON biçiminde yüz konumları. Sınırlayıcı kutular Bulanıklaştırma değiştirmek için kullanıcı tarafından düzenlenebilir. Aşağıdaki örneğe bakın. |
| Çıktı varlığına |foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg] |Yüz tanıma, burada sayıyı labelId yazıtipinin değerini belirtir her kırpılmış jpg algılandı |

#### <a name="output-example"></a>Örnek çıktı:

```json
    {
      "version": 1,
      "timescale": 24000,
      "offset": 0,
      "framerate": 23.976,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 48048,
          "interval": 1001,
          "events": [
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [
              {
                "index": 13,
                "id": 1138,
                "x": 0.29537,
                "y": -0.18987,
                "width": 0.36239,
                "height": 0.80335
              },
              {
                "index": 13,
                "id": 2028,
                "x": 0.60427,
                "y": 0.16098,
                "width": 0.26958,
                "height": 0.57943
              }
            ],

    … truncated
```

### <a name="redact-mode"></a>Modu özgürlüğü
İkinci geçiş için iş akışının çok sayıda tek bir varlığa birleştirilmelidir girişleri alır.

Bu, ek açıklamalar JSON bulanıklaştıran kimliklerinin ve özgün video listesini içerir. Bu mod, giriş videosu bulanıklaştırarak uygulamak için ek açıklamalar kullanır.

Özgün videoyu çözümleme geçişi çıkışı içermez. Video Redact modu görev için giriş varlığı içine yüklenir ve birincil dosya seçili gerekir.

| Aşama | Dosya Adı | Notlar |
| --- | --- | --- |
| Giriş varlığı |foo.bar |Video WMV, MPV veya MP4 biçimindeki. Aynı video 1. adım olduğu gibi. |
| Giriş varlığı |foo_annotations.json |meta veri dosyasından aşaması, isteğe bağlı değişiklikler ile ek açıklamalar. |
| Giriş varlığı |foo_IDList.txt (isteğe bağlı) |İsteğe bağlı yeni bir satır virgülle ayrılmış yüz özgürlüğü kimliklerinin bir listesi. Boş bırakılırsa, bu tüm yüzleri bulanıklaştırır. |
| Giriş yapılandırma |İş yapılandırması hazır |{'version':'1.0 ', 'Seçenekler': {'mode': 'özgürlüğü'}} |
| Çıktı varlığına |foo_redacted.mp4 |Ek açıklamalar göre uygulanan Bulanıklaştırma ile video |

#### <a name="example-output"></a>Örnek çıktı
Bu, seçilen bir Kimliğe sahip bir IDList çıktısı bulunmaktadır.

[Bu videoyu izleyin](https://ampdemo.azureedge.net/?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

Örnek foo_IDList.txt
 
     1
     2
     3

## <a name="blur-types"></a>Bulanıklaştırma türleri

İçinde **birleştirilmiş** veya **Redact** modu, seçim yapabileceğiniz bir JSON giriş yapılandırma ile 5 farklı Bulanıklaştırma mod vardır: **Düşük**, **Med**, **yüksek**, **kutusu**, ve **siyah**. Varsayılan olarak **Med** kullanılır.

Bulanıklaştırma türlerinin örnekleri bulabilirsiniz.

### <a name="example-json"></a>Örnek JSON:

```json
    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}
```

#### <a name="low"></a>Düşük

![Düşük](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a>MED

![MED](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a>Yüksek

![Yüksek](./media/media-services-face-redaction/blur3.png)

#### <a name="box"></a>Box

![Box](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a>Siyah

![Siyah](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-the-output-json-file"></a>Çıkış JSON dosyasının öğeleri

Yüksek hassaslıktaki yüz konumu algılama Redaksiyon MP sağlar ve izleme en fazla 64 İnsan yüzlerini video çerçevede algılayabilir. En iyi sonuçları tamamen çıplak yüzleri sağlayın, yan yüz ve küçük yüzleri (küçüktür veya eşittir 24 x 24 piksel) zor olur.

[!INCLUDE [media-services-analytics-output-json](../../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki program gösterir nasıl yapılır:

1. Bir varlık oluşturun ve varlığa bir medya dosyası yükleyin.
2. Aşağıdaki json hazır içeren bir yapılandırma dosyasını temel alan bir yüz flulaştırma görev ile bir iş oluşturun: 

    ```json
            {
                'version':'1.0',
                'options': {
                    'mode':'combined'
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

namespace FaceRedaction
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

            // Run the FaceRedaction job.
            var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                        @"C:\supportFiles\FaceRedaction\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
        }

        static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Face Redaction Input Asset",
            AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Redaction Job");

            // Get a reference to Azure Media Redactor.
            string MediaProcessorName = "Azure Media Redactor";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Redaction Task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);

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

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analizi'ne genel bakış](media-services-analytics-overview.md)

[Azure medya analizi tanıtımları](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

