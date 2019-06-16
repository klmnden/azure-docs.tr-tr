---
title: Bir varlık kullanarak Media Encoder .NET Standard ile kodlama | Microsoft Docs
description: Bu makalede, Media Encoder Standard kullanarak bir varlığı kodlama .NET kullanmayı gösterir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 03431b64-5518-478a-a1c2-1de345999274
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako;anilmur
ms.openlocfilehash: d3eb2affe76374eb35ac724dff0204f43b567e09
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61225737"
---
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Bir varlık kullanarak Media Encoder .NET Standard ile kodlama  

Kodlama işleri, Media Services’da en sık gerçekleştirilen işlemler arasındadır. Kodlama işleri oluşturarak, medya dosyalarını bir kodlamadan diğerine dönüştürebilirsiniz. Kodlarken, Media Services yerleşik Medya Kodlayıcısı kullanabilirsiniz. Bir Media Services iş ortağı tarafından verilen bir kodlayıcı da kullanabilirsiniz; üçüncü taraf Kodlayıcı Azure Marketi aracılığıyla kullanılabilir. 

Bu makalede, .NET, varlıkları ile Medya Kodlayıcısı standart (MES) kodlama için nasıl kullanılacağını gösterir. Media Encoder Standard yapılandırılmış kodlayıcılar hazır birini kullanarak açıklanan [burada](https://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Her zaman kaynak dosyalarını Uyarlamalı bit hızı MP4 kümesi kodlar ve ardından istediğiniz biçimi kullanarak kümesi dönüştürmek için önerilen [dinamik paketleme](media-services-dynamic-packaging-overview.md). 

Şifrelenmiş depolama, çıktı varlık ise varlık teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için [varlık teslim ilkesini yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md).

> [!NOTE]
> MES, giriş dosya adının ilk 32 karakter içeren bir ada sahip bir çıktı dosyası üretir. Ad, hazır dosyasında belirtilen üzerinde temel alır. Örneğin, "dosya": "{Basename} _ {Index} {Extension}". {Basename} giriş dosyası adı ilk 32 karakterleriyle değiştirilir.
> 
> 

### <a name="mes-formats"></a>MES biçimleri
[Biçimleri ve codec bileşenleri](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a>MES Ön ayarları
Media Encoder Standard yapılandırılmış kodlayıcılar hazır birini kullanarak açıklanan [burada](https://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Giriş ve çıkış meta verileri
MES kullanarak giriş bir varlık (veya varlık) kodlarken bir çıktı varlığı başarılı elde tamamlama, söz konusu görev kodlayın. Çıktı varlığı video, ses, küçük resimler, bildirimi, kullandığınız kodlama Önayarı üzerinde temel vb. içerir.

Çıktı varlığına giriş varlığı hakkındaki meta veriler içeren bir dosya da içerir. Meta veri XML dosyasının adı şu biçimdedir: < asset_id > _metadata.xml (örneğin, 41114ad3-eb5e - 4 c 57 8 d 92-5354e2b7d4a4_metadata.xml), burada < asset_id >, giriş varlığı AssetID değeri. Bu giriş meta verileri XML Şeması açıklanan [burada](media-services-input-metadata-schema.md).

Çıktı varlığına bir dosyayla çıktı varlığına ilişkin meta verileri de içerir. Meta veri XML dosyasının adı şu biçimdedir: < source_file_name > _manifest.xml (örneğin, BigBuckBunny_manifest.xml). Bu çıkış meta verileri XML açıklanmıştır şemasını [burada](media-services-output-metadata-schema.md).

Ya da iki meta veri dosyaları incelemek isterseniz, SAS Bulucu oluşturun ve dosyayı yerel bilgisayarınıza indirin. Bir SAS Bulucu oluşturmanız ve Media Services .NET SDK uzantıları kullanarak bir dosyayı indirmek nasıl bir örnek bulabilirsiniz.

## <a name="download-sample"></a>Örnek indirme
Alın ve MES ile kodlama gösteren bir örnek çalıştırmak [burada](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki kod örneği, aşağıdaki görevleri gerçekleştirmek için Media Services .NET SDK'sını kullanır:

* Bir kodlama işi oluşturun.
* Medya Kodlayıcısı standart Kodlayıcı bir başvuru alın.
* Kullanılacağını belirtin [Uyarlamalı akış](media-services-autogen-bitrate-ladder-with-mes.md) hazır. 
* Tek bir kodlama görevi işe ekleyin. 
* Kodlanacak giriş varlığı belirtin.
* Kodlanmış varlığı içeren bir çıkış varlık oluşturun.
* İş ilerleme durumunu denetlemek için bir olay işleyicisi ekleyin.
* İşi Gönder.

#### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

#### <a name="example"></a>Örnek 

```csharp
using System;
using System.Linq;
using System.Configuration;
using System.IO;
using System.Threading;
using Microsoft.WindowsAzure.MediaServices.Client;

namespace MediaEncoderStandardSample
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

        private static readonly string _supportFiles =
            Path.GetFullPath(@"../..\Media");

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

            // Create a task with the encoding details, using a string preset.
            // In this case "Adaptive Streaming" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
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

## <a name="advanced-encoding-features-to-explore"></a>Özelliklerini keşfetmek için gelişmiş kodlama
* [Küçük resim oluşturma](media-services-dotnet-generate-thumbnail-with-mes.md)
* [Kodlama sırasında küçük resimler oluşturma](media-services-dotnet-generate-thumbnail-with-mes.md#example-of-generating-a-thumbnail-while-encoding)
* [Kodlama sırasında videoları kırpma](media-services-crop-video.md)
* [Kodlama Önayarları özelleştirme](media-services-custom-mes-presets-with-dotnet.md)
* [Yer paylaşımı veya bir video görüntü ile filigranı](media-services-advanced-encoding-with-mes.md#overlay)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
[.NET ile Media Encoder Standard kullanarak küçük resim oluşturma](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services kodlama genel bakış](media-services-encode-asset.md)

