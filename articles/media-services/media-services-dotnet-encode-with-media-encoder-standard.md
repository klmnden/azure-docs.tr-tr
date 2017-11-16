---
title: "Bir varlık Medya Kodlayıcısı .NET kullanarak standart ile kodlamak | Microsoft Docs"
description: "Bu makalede .NET medya Kodlayıcı standart kullanarak bir varlık kodlama için nasıl kullanılacağı gösterilmektedir."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 03431b64-5518-478a-a1c2-1de345999274
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: juliako;anilmur
ms.openlocfilehash: cce668007030672aff7af60c70339c1e079c75b1
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Bir varlık Medya Kodlayıcısı .NET kullanarak standart ile kodlamak
Kodlama işleri, Media Services’da en sık gerçekleştirilen işlemler arasındadır. Kodlama işleri oluşturarak, medya dosyalarını bir kodlamadan diğerine dönüştürebilirsiniz. Kodlarken, Media Services yerleşik medya kodlayıcı kullanabilirsiniz. Bir Media Services iş ortağı tarafından sağlanan bir kodlayıcı de kullanabilirsiniz; üçüncü taraf kodlayıcılar Azure Market üzerinden kullanılabilir. 

Bu makalede .NET varlıklarınızı ile Medya Kodlayıcısı standart (MES) kodlamak için nasıl kullanılacağı gösterilmektedir. Medya Kodlayıcısı standart yapılandırılmış kodlayıcılar hazır ayarlarından birini kullanarak açıklanan [burada](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Her zaman Kaynak dosyalarınız Uyarlamalı bit hızlı MP4 kümesi kodlamak ve ardından istenen biçim kullanmaya kümesi dönüştürmek için önerilen [dinamik paketleme](media-services-dynamic-packaging-overview.md). 

Çıkış varlığına depolama şifrelenmiş ise, varlık teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için bkz: [varlık teslim ilkesini yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md).

> [!NOTE]
> MES giriş dosya adının ilk 32 karakter içeren bir ada sahip bir çıktı dosyası oluşturur. Ad önceden ayarlanmış dosyasında belirtilen üzerinde temel alır. Örneğin, "dosya adı": "{Basename} _ {Index} {uzantısı}". {Basename} girdi dosyası adı ilk 32 karakteri tarafından değiştirilir.
> 
> 

### <a name="mes-formats"></a>MES biçimleri
[Biçimleri ve codec bileşenleri](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a>MES Ön ayarları
Medya Kodlayıcısı standart yapılandırılmış kodlayıcılar hazır ayarlarından birini kullanarak açıklanan [burada](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Giriş ve çıkış meta verileri
Bir giriş varlık (veya varlıklar) MES kullanarak kodlarken bir çıkış varlığı başarılı elde tamamlama, görev kodlayın. Çıkış varlığına, video, ses, küçük resimleri, bildirim, kullandığınız kodlama hazır ayar göre vb. içerir.

Çıkış varlığına da giriş varlık hakkındaki meta verileri ile bir dosya içeriyor. Meta veri XML dosyasının adı şu biçimdedir: < asset_id > _metadata.xml (örneğin, 41114ad3-eb5e - 4 c 57-8 d 92-5354e2b7d4a4_metadata.xml), burada < asset_id >, giriş varlık AssetID değeri. Bu giriş meta veri XML Şeması açıklanan [burada](media-services-input-metadata-schema.md).

Çıkış varlığına da çıkış varlığına hakkındaki meta verileri ile bir dosya içeriyor. Meta veri XML dosyasının adı şu biçimdedir: < source_file_name > _manifest.xml (örneğin, BigBuckBunny_manifest.xml). Bu çıktı meta veri XML açıklanmıştır ve şema [burada](media-services-output-metadata-schema.md).

İki meta veri dosyalarının herhangi birini incelemek isterseniz, bir SAS Bulucu oluşturun ve dosyayı yerel bilgisayarınıza yükleyin. SAS Bulucu oluşturmanız ve Media Services .NET SDK uzantıları kullanarak dosya indirme hakkında bir örnek bulabilirsiniz.

## <a name="download-sample"></a>Örnek indirme
Alma ve MES ile kodlamak nasıl oluşturulduğunu gösteren bir örnek çalıştırın [burada](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="net-sample-code"></a>.NET örnek kod

Aşağıdaki kod örneği, aşağıdaki görevleri gerçekleştirmek için Media Services .NET SDK'sını kullanır:

* Bir kodlama işi oluşturun.
* Medya Kodlayıcısı standart Kodlayıcı başvuru alın.
* Kullanılacak belirtin [Uyarlamalı akış](media-services-autogen-bitrate-ladder-with-mes.md) hazır. 
* Tek bir kodlama görev projeye ekleyin. 
* Kodlanacak giriş varlık belirtin.
* Kodlanmış varlık içeren bir çıkış varlığı oluşturun.
* İş ilerleme durumunu denetlemek için olay işleyici ekleyin.
* İşi göndermek.

#### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

#### <a name="example"></a>Örnek 

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
                    ConfigurationManager.AppSettings["AADTenantDomain"];
                private static readonly string _RESTAPIEndpoint =
                    ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];

                // Field for service context.
                private static CloudMediaContext _context = null;

                private static readonly string _supportFiles =
                    Path.GetFullPath(@"../..\Media");

                static void Main(string[] args)
                {
                    var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
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


## <a name="advanced-encoding-features-to-explore"></a>Özellikleri keşfetmek için gelişmiş kodlama
* [Küçük resimler oluşturma](media-services-dotnet-generate-thumbnail-with-mes.md)
* [Kodlama sırasında küçük resimler oluşturma](media-services-dotnet-generate-thumbnail-with-mes.md#example-of-generating-a-thumbnail-while-encoding)
* [Kodlama sırasında kırpma videolar](media-services-crop-video.md)
* [Kodlama hazır ayarlarını özelleştirme](media-services-custom-mes-presets-with-dotnet.md)
* [Yer paylaşımı veya bir görüntü ile video Filigran](media-services-advanced-encoding-with-mes.md#overlay)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
[.NET ile medya Kodlayıcı standart kullanarak küçük resim oluşturmak nasıl](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services kodlama genel bakış](media-services-encode-asset.md)

