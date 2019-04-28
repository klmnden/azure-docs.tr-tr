---
title: Otomatik olarak bit hızı Merdiveni oluşturmak için Azure Medya Kodlayıcısı standart'ı kullanma | Microsoft Docs
description: Bu konuda giriş çözünürlük ve bit hızı göre hızı Merdivenini otomatik olarak oluşturmak için medya Kodlayıcı standart (MES) kullanmayı gösterir. Giriş çözünürlük ve bit hızı hiçbir zaman aşacak. Örneğin, giriş ise 3 MB/sn, çıkış, 720p 720 p en iyi şekilde kalır ve 3 MB/sn düşük fiyatları başlar.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: bbaf4d490fcebb4cd741a9b83ffc5d7e85699755
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61224353"
---
#  <a name="use-azure-media-encoder-standard-to-auto-generate-a-bitrate-ladder"></a>Otomatik olarak bit hızı Merdiveni oluşturmak için Azure Medya Kodlayıcısı standart'ı kullanın  

## <a name="overview"></a>Genel Bakış

Bu makalede, medya Kodlayıcı standart (MES) giriş çözünürlük ve bit hızı göre hızı Merdiveni (bit hızı çözümleme çiftleri) otomatik olarak oluşturmak için nasıl kullanılacağı gösterilmektedir. Otomatik olarak oluşturulan ön ayarın hiçbir zaman giriş çözünürlük ve bit hızı aşacak. Örneğin, giriş 3 MB/sn hızında 720 p, çıkış 720 p en iyi şekilde kalır ve 3 MB/sn düşük fiyatları başlar.

### <a name="encoding-for-streaming-only"></a>Yalnızca akış kodlama

Amacınız, akış için yalnızca kaynak videonuzu kodlamanız ise, "Uyarlamalı bir kodlama görevi oluştururken önceden akış" kullanmanız gerekir. Kullanırken **Uyarlamalı akış** hazır MES Kodlayıcı akıllıca hızı Merdiveni cap. Ancak, hizmeti kullanmak için kaç Katmanlar belirler. bu yana kodlama, maliyetleri denetlemek ve hangi çözünürlükte mümkün olmayacaktır. Sonucu olarak kodlama ile MES tarafından üretilen çıkış katmanları örneklerini gördüğünüz **Uyarlamalı akış** bu makalenin sonunda hazır. Ses ve video varlık MP4 dosyalarını içeren çıktı aralıklı değil.

### <a name="encoding-for-streaming-and-progressive-download"></a>Akış ve aşamalı indirme kodlama

Amacınız, kaynak videonuzu akışla kodlanacak yanı sıra MP4 dosyalarını aşamalı indirmek için üretmek için ise, "içerik Uyarlamalı Çoklu bit hızı bir kodlama görevi oluştururken önceden MP4" kullanmanız gerekir. Kullanırken **içerik Uyarlamalı Çoklu bit hızı MP4** hazır MES Kodlayıcı kodlama yukarıdaki, aynı mantığı uygular, ancak artık çıktı varlığına MP4 dosyaları ses burada içerir ve video araya eklemeli. Aşamalı indirme dosya olarak bu MP4 dosyaları (örneğin, en yüksek hızı sürüm) birini kullanabilirsiniz.

## <a id="encoding_with_dotnet"></a>Media Services .NET SDK ile kodlama

Aşağıdaki kod örneği, aşağıdaki görevleri gerçekleştirmek için Media Services .NET SDK'sını kullanır:

- Bir kodlama işi oluşturun.
- Medya Kodlayıcısı standart Kodlayıcı bir başvuru alın.
- Bir kodlama görevi işe ekleyin ve kullanılacağını belirtin **Uyarlamalı akış** hazır. 
- Kodlanmış varlığı içeren bir çıkış varlık oluşturun.
- İş ilerleme durumunu denetlemek için bir olay işleyicisi ekleyin.
- İşi Gönder.

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

## <a id="output"></a>Çıkış

Bu bölümde sonucunda kodlama ile MES tarafından üretilen çıkış katmanları, üç örnek gösterir **Uyarlamalı akış** hazır. 

### <a name="example-1"></a>Örnek 1
Yükseklik "1080" ve "29.970" kare hızını kaynağıyla 6 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bitrate(kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Örnek 2
Yükseklik "720" ve "23.970" kare hızını kaynağıyla 5 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bitrate(kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Örnek 3
Yükseklik "360" ve "29.970" kare hızını kaynağıyla 3 video katmanları üretir:

|Katman|Yükseklik|Genişlik|Bitrate(kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|
## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Media Services Encoding genel bakış](media-services-encode-asset.md)

