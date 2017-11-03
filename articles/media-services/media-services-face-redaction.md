---
title: "Azure medya Analizi ile yüzeyleri Redaksiyon | Microsoft Docs"
description: "Bu konuda, Azure medya Analizi ile yüzeyleri Redaksiyon gösterilmiştir."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 5b6d8b8c-5f4d-4fef-b3d6-dc22c6b5a0f5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/27/2017
ms.author: juliako;
ms.openlocfilehash: b3584c5aa5405e7f5acdd9bc0a6573b4acbab855
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="redact-faces-with-azure-media-analytics"></a>Azure medya Analizi ile yüzeyleri Redaksiyon
## <a name="overview"></a>Genel Bakış
**Azure Media Redactor** olan bir [Azure medya analizi](media-services-analytics-overview.md) medya işlemcisi (MP) ölçeklenebilir yüz Redaksiyon bulutta sunar. Yüz Redaksiyon seçili kişiler yüzeyleri ölçeklendirilmelidir için videonuzu değiştirmenizi sağlar. Genel güvenlik ve haber medya senaryolarda yüz Redaksiyon hizmeti kullanmak isteyebilirsiniz. Birden çok yüzeyleri içeren çekimi birkaç dakika el ile Redaksiyon saat sürebilir, ancak bu hizmet ile birkaç basit adımla yüz Redaksiyon işlem gerektirir. Daha fazla bilgi için bkz: [bu](https://azure.microsoft.com/blog/azure-media-redactor/) blogu.

Bu konu hakkında ayrıntılar verir **Azure medya Redactor** ve Media Services SDK'sı ile .NET için nasıl kullanılacağını gösterir.

## <a name="face-redaction-modes"></a>Yüz Redaksiyon modları
Böylece aynı tek diğer açıları bulanık yüz Redaksiyon her çerçevesinde video yüz algılama ve her iki yüz nesne, zaman içindeki iletir ve geriye doğru izleme çalışır. Otomatik Redaksiyon çok karmaşık bir işlemdir ve mu değil her zaman ürettiği % 100'ünü bu nedenle medya analizi için istenen çıkış sağlar, çeşitli şekillerde son çıkışı değiştirilecek.

Tamamen otomatik modu ek olarak, seçim/Kaldır-selection kimlikleri listesini aracılığıyla bulunan yazıtipleri, sağlayan iki geçişi iş akışı yok. Ayrıca, çerçeve ayarlamalar MP rasgele yapmak için bir meta veri dosyası JSON biçiminde kullanır. Bu iş akışı bölündüğünü **Çözümle** ve **Redact** modları. Her iki görevi bir işe çalıştıran tek bir geçiş iki modda birleştirebilirsiniz. Bu mod adlı **birleştirilmiş**.

### <a name="combined-mode"></a>Birleşik modu
Bu Redaksiyonu yapılmış bir mp4 otomatik olarak giriş herhangi bir el ile oluşturur.

| Aşama | Dosya adı | Notlar |
| --- | --- | --- |
| Giriş varlık |foo.bar |Video WMV, MOV veya MP4 biçimindeki |
| Giriş yapılandırma |İş yapılandırması hazır |{'version':'1.0 ', 'Seçenekler': {'modu': 'birleştirilmiş'}} |
| Çıkış varlık |foo_redacted.mp4 |Uygulanan Bulanıklaştırma ile video |

#### <a name="input-example"></a>Giriş örnek:
[Bu videoyu izleyin](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a>Çıkış örneği:
[Bu videoyu izleyin](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a>Mod Çözümle
**Analiz** geçişi iki geçişi iş akışının video girdi alır ve yüz konumların bir JSON dosyası oluşturur ve her birinin jpg görüntüleri yüz algılandı.

| Aşama | Dosya adı | Notlar |
| --- | --- | --- |
| Giriş varlık |foo.bar |Video WMV, MPV veya MP4 biçimindeki |
| Giriş yapılandırma |İş yapılandırması hazır |{'version':'1.0 ', 'Seçenekler': {'modu': 'çözümleyin'}} |
| Çıkış varlık |foo_annotations.JSON |Ek açıklama verileri JSON biçiminde yüz konumu. Bu sınırlayıcı kutular Bulanıklaştırma değiştirmek için kullanıcı tarafından düzenlenebilir. Aşağıdaki örneğe bakın. |
| Çıkış varlık |foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg] |Yüz burada yüz labelId sayıyı belirtir, her kırpılmış jpg algılandı |

#### <a name="output-example"></a>Çıkış örneği:

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

### <a name="redact-mode"></a>Mod Redaksiyon
İkinci geçişi iş akışının çok sayıda tek bir varlığa birleştirilmelidir girişleri alır.

Bu ölçeklendirilmelidir kimlikleri, özgün video ve ek açıklamalar JSON listesini içerir. Bu mod, video giriş Bulanıklaştırma uygulamak için ek açıklamalar kullanır.

Çözümle geçişi çıktısını özgün video içermez. Video Redact modu görev için giriş varlık içine yüklenebilir ve birincil dosya olarak seçili gerekiyor.

| Aşama | Dosya adı | Notlar |
| --- | --- | --- |
| Giriş varlık |foo.bar |Video WMV, MPV veya MP4 biçimindeki. Aynı 1. adım olduğu gibi video. |
| Giriş varlık |foo_annotations.JSON |meta veri dosyasından birinci aşaması, isteğe bağlı değişiklik yapılması açısından ek açıklamaları. |
| Giriş varlık |foo_IDList.txt (isteğe bağlı) |İsteğe bağlı yeni bir satırla ayrılmış yüz Redaksiyon kimliklerinin listesi. Boş bırakılırsa, bu tüm yüzeyleri bulanıklaştırır. |
| Giriş yapılandırma |İş yapılandırması hazır |{'version':'1.0 ', 'Seçenekler': {'modu': 'Redaksiyon'}} |
| Çıkış varlık |foo_redacted.mp4 |Ek açıklamalar uygulanan Bulanıklaştırma ile video dayalı |

#### <a name="example-output"></a>Örnek çıktı
Seçili bir Kimliğine sahip bir IDList çıkışı budur.

[Bu videoyu izleyin](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

Örnek foo_IDList.txt
 
     1
     2
     3

## <a name="blur-types"></a>Türleri ölçeklendirilmelidir

İçinde **birleştirilmiş** veya **Redact** modu, seçim yapabileceğiniz JSON giriş yapılandırma 5 farklı Bulanıklaştırma modu vardır: **düşük**, **Med**, **Yüksek**, **kutusunu**, ve **siyah**. Varsayılan olarak **Med** kullanılır.

Bulanıklaştırma türlerinin örnekleri bulabilirsiniz.

### <a name="example-json"></a>Örnek JSON:

    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}

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

İzleme özelliği en fazla 64 İnsan yüzeyleri video çerçevede algılayabilir ve yüksek düzeyde hassasiyet yüz konumu algılama Redaksiyon MP sağlar. Yan yüz sırasında ve küçük yazıtipleri (küçük veya eşittir 24 x 24 piksel) zor, en iyi sonuçlar tamamen çıplak yüzeyleri belirtin.

[!INCLUDE [media-services-analytics-output-json](../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a>.NET örnek kod

Aşağıdaki program gösterir nasıl yapılır:

1. Bir varlık oluşturun ve varlığa bir medya dosyasını yükleyin.
2. Aşağıdaki json hazır içeren bir yapılandırma dosyasına dayalı yüz Redaksiyon görevle ilgili bir iş oluşturun. 
   
        {'version':'1.0', 'options': {'mode':'combined'}}
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

    namespace FaceRedaction
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

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analytics a genel bakış](media-services-analytics-overview.md)

[Azure medya analizi gösterileri](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

