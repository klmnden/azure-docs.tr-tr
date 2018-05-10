---
title: Bit hızı Merdiveni otomatik olarak oluşturmak için Azure Medya Kodlayıcısı standart kullanın | Microsoft Docs
description: Bu konu, Medya Kodlayıcısı standart (MES) giriş çözünürlük ve bit hızı dayalı bir bit hızı Merdiveni otomatik olarak oluşturmak için nasıl kullanılacağını gösterir. Giriş çözünürlük ve bit hızı hiçbir zaman aşacak. Örneğin, giriş olması durumunda 3 MB/sn, çıktı adresindeki 720p 720 p en iyi kalır ve 3 MB/sn düşük hızlarında başlar.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2017
ms.author: juliako
ms.openlocfilehash: 80f76f413ec2c267ba8fb93514480e168563f470
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
#  <a name="use-azure-media-encoder-standard-to-auto-generate-a-bitrate-ladder"></a>Bit hızı Merdiveni otomatik olarak oluşturmak için Azure Medya Kodlayıcısı standart'ı kullanın

## <a name="overview"></a>Genel Bakış

Bu makalede Medya Kodlayıcısı standart (MES) giriş çözünürlük ve bit hızı dayalı bir bit hızı Merdiveni (bit hızı çözümleme çiftleri) otomatik olarak oluşturmak için nasıl kullanılacağı gösterilmektedir. Otomatik olarak oluşturulan hazır hiç giriş çözünürlük ve bit hızı aşacak. Örneğin, giriş 3 MB/sn hızında 720 p ise, çıktı 720 p en iyi kalır ve 3 MB/sn düşük hızlarında başlar.

### <a name="encoding-for-streaming-only"></a>Yalnızca akış için kodlama

Ardından, amacı yalnızca akış için kaynak videonuzu kodlamak için ise, "Uyarlamalı kodlama bir görev oluştururken önceden akış" kullanmanız gerekir. Kullanırken **Uyarlamalı akış** hazır MES Kodlayıcı akıllıca bit hızı Merdiveni cap. Ancak, hizmeti kullanmak için kaç tane Katmanlar belirler beri kodlama, maliyetleri denetlemek ve hangi çözünürlükte mümkün olmayacaktır. Örnek ile kodlama sonucunda MES tarafından üretilen çıkış katmanların görebilirsiniz **Uyarlamalı akış** bu makalenin sonundaki hazır. Ses ve video burada varlık MP4 dosyaları içeren çıktı araya eklenmez.

### <a name="encoding-for-streaming-and-progressive-download"></a>Akış ve aşamalı indirme kodlama

Maksadınızı kaynak videonuzu akışla kodlanacak yanı sıra MP4 dosyalarını aşamalı indirmek üretmek için ise, "içerik Uyarlamalı Çoklu bit hızı kodlama bir görev oluştururken önceden MP4" kullanmanız gerekir. Kullanırken **içerik Uyarlamalı Çoklu bit hızlı MP4** hazır MES Kodlayıcı kodlama yukarıdaki aynı mantığını uygular, ancak şimdi çıkış varlığına MP4 dosyaları ses burada içerir ve video araya eklemeli. Aşamalı indirme dosyası olarak bu MP4 dosyaları (örneğin, yüksek bit hızı sürüm) kullanabilirsiniz.

## <a id="encoding_with_dotnet"></a>Media Services .NET SDK'sı ile kodlama

Aşağıdaki kod örneği, aşağıdaki görevleri gerçekleştirmek için Media Services .NET SDK'sını kullanır:

- Bir kodlama işi oluşturun.
- Medya Kodlayıcısı standart Kodlayıcı başvuru alın.
- Bir kodlama görev işe ekleme ve kullanılacağını belirtin **Uyarlamalı akış** hazır. 
- Kodlanmış varlık içeren bir çıkış varlığı oluşturun.
- İş ilerleme durumunu denetlemek için olay işleyici ekleyin.
- İşi göndermek.

#### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

#### <a name="example"></a>Örnek

```
using System;
using System.Configuration;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Threading;

namespace AdaptiveStreamingMESPresest
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

            // Get an uploaded asset.
            var asset = _context.Assets.FirstOrDefault();

            // Encode and generate the output using the "Adaptive Streaming" preset.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");

            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                    break;
                default:
                    break;
            }
        }
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
    }
}
```

## <a id="output"></a>Çıktı

Bu bölümde üç örnekleri ile kodlama sonucunda MES tarafından üretilen çıkış Katmanlar gösterilmiştir **Uyarlamalı akış** hazır. 

### <a name="example-1"></a>Örnek 1
Yükseklik "1080" ve "29.970" kare hızı kaynağıyla 6 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bitrate(Kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Örnek 2
Yükseklik "720" ve "23.970" kare hızı kaynağıyla 5 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bitrate(Kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Örnek 3
Yükseklik "360" ve "29.970" kare hızı kaynağıyla 3 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bitrate(Kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|
## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Media Services kodlama a genel bakış](media-services-encode-asset.md)

