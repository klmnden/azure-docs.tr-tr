---
title: Media Encoder Standard ve .NET kullanarak küçük resim oluşturma
description: Bu konuda, .NET bir varlığı kodlama ve aynı anda Media Encoder Standard kullanarak küçük resim oluşturma için nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: b8dab73a-1d91-4b6d-9741-a92ad39fc3f7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 6bc29c098bcf7ef1d1a2e2532a00c95f0ec7e927
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61244238"
---
# <a name="how-to-generate-thumbnails-using-media-encoder-standard-with-net"></a>Media Encoder Standard ve .NET kullanarak küçük resim oluşturma 

Giriş video gelen bir veya daha fazla küçük resim oluşturma Media Encoder Standard kullanabilirsiniz [JPEG](https://en.wikipedia.org/wiki/JPEG), [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics), veya [BMP](https://en.wikipedia.org/wiki/BMP_file_format) resim dosya biçimlerinde. Yalnızca görüntü üretmek görevler gönderebilir veya kodlama ile küçük resim oluşturma birleştirebilirsiniz. Bu makalede, bu tür senaryolar için birkaç örnek XML ve JSON küçük resim hazır sağlar. Makalenin sonunda, var olan bir [örnek kod](#code_sample) eden kodlama görevi gerçekleştirmek için Media Services .NET SDK'sını kullanmayı gösterir.

Örnek hazır kullanılan öğeleri üzerinde daha fazla ayrıntı için gözden geçirmeniz gereken [Media Encoder Standard şeması](media-services-mes-schema.md).

Gözden geçirdiğinizden emin olun [konuları](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) bölümü.
    
## <a name="example-of-a-single-png-file-preset"></a>Örnek bir "tek PNG dosyası" hazır

Aşağıdaki JSON ve XML hazır burada Kodlayıcı "ilginç" bir çerçeve bulma sırasında bir en yüksek çaba girişimlerde tek çıkış PNG dosyadan ilk birkaç saniye video, giriş oluşturmak için kullanılabilir. Bu giriş videosunun boyutları eşleşecek yani % 100 çıkış resim boyutları ayarlanan unutmayın. Ayrıca, nasıl "Çıkış" içindeki "Format" ayarı "Codec" bölümünde "PngLayers" kullanımını eşleşmesi gereken unutmayın. 

### <a name="json-preset"></a>JSON hazır

```json
    {
      "Version": 1.0,
      "Codecs": [
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "PngImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        }
      ]
    }
```
    
### <a name="xml-preset"></a>XML hazır

```xml
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <PngImage Start="{Best}">
          <PngLayers>
            <PngLayer>
              <Width>100%</Width>
              <Height>100%</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>
```

## <a name="example-of-a-series-of-jpeg-images-preset"></a>Örnek "JPEG görüntülerinin serisi" hazır

Zaman damgaları 5, 10 görüntü kümesi oluşturmak için aşağıdaki JSON ve XML hazır kullanılabilir % %15,..., % 95'burada görüntü boyutu belirtildi uygulanacak giriş zaman çizelgesi, video giriş aylık.

### <a name="json-preset"></a>JSON hazır

```json
    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "5%",
          "Step": "10%",
          "Range": "96%",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }
```

### <a name="xml-preset"></a>XML hazır
    
```xml
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="5%" Step="10%" Range="96%">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>
```

## <a name="example-of-a-one-image-at-a-specific-timestamp-preset"></a>Örnek olarak "belirli bir zaman damgası, bir görüntü" hazır

Aşağıdaki JSON ve XML hazır, video giriş 30 saniye işaretinde tek bir JPEG görüntüsünü oluşturmak için kullanılabilir. Bu önceden ayarlanmış süresi 30 saniyeden fazla olması için giriş videosunun bekliyor (başka bir iş başarısız).

### <a name="json-preset"></a>JSON hazır

```json
    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "00:00:30",
          "Step": "1",
          "Range": "1",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }
```

### <a name="xml-preset"></a>XML hazır
```xml
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="00:00:30" Step="00:00:01" Range="00:00:01">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>
```

## <a name="example-of-a-thumbnails-at-different-resolutions-preset"></a>"Küçük resimleri farklı çözünürlükte" hazır örneği

Aşağıdaki önceden ayarlanmış bir görevde farklı çözünürlükte küçük resimler oluşturmak için kullanılabilir. Örnekte, konumlar %5 %15,..., % 95'giriş zaman çizelgesi Kodlayıcı – bir giriş görüntü çözünürlüğünü ve diğer % %50 100 iki görüntü oluşturur.

{Çözümleme} makrosu dosya adında kullanımına dikkat edin; Bu, genişlik ve yükseklik çıkış görüntüleri dosya adını oluştururken hazır kodlama bölümünde belirtilen kullanmak için Kodlayıcı gösterir. Ayrıca farklı görüntülerin arasında kolayca ayırt etmenize yardımcı olur

### <a name="json-preset"></a>JSON hazır

```json
    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
        {
          "Quality": 90,
          "Type": "JpgLayer",
          "Width": "100%",
          "Height": "100%"
        },
        {
          "Quality": 90,
          "Type": "JpgLayer",
          "Width": "50%",
          "Height": "50%"
        }

          ],
          "Start": "5%",
          "Step": "10%",
          "Range": "96%",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Resolution}_{Index}{Extension}",
          "Format": {
        "Type": "JpgFormat"
          }
        }
      ]
    }
```

### <a name="xml-preset"></a>XML hazır
```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
    <Encoding>
    <JpgImage Start="5%" Step="10%" Range="96%"><JpgImage Start="00:00:01" Step="00:00:15">
      <JpgLayers>
       <JpgLayer>
        <Width>100%</Width>
        <Height>100%</Height>
        <Quality>90</Quality>
       </JpgLayer>
       <JpgLayer>
        <Width>50%</Width>
        <Height>50%</Height>
        <Quality>90</Quality>
       </JpgLayer>
      </JpgLayers>
    </JpgImage>
    </Encoding>
    <Outputs>
      <Output FileName="{Basename}_{Resolution}_{Index}{Extension}">
        <JpgFormat/>
      </Output>
    </Outputs>
    </Preset>
```

## <a name="example-of-generating-a-thumbnail-while-encoding"></a>Kodlama sırasında bir küçük resim oluşturma örneği

Yukarıdaki örneklerde tüm görüntüleri yalnızca oluşturan bir kodlama görevi nasıl gönderebilirsiniz ele olsa da ayrıca, küçük resim oluşturma ile görüntü/ses kodlama birleştirebilirsiniz. Söyleyin aşağıdaki JSON ve XML hazır **Media Encoder Standard** kodlama sırasında küçük resim oluşturma için.

### <a id="json"></a>JSON hazır
Şeması hakkında daha fazla bilgi için bkz. [bu](https://msdn.microsoft.com/library/mt269962.aspx) makalesi.

```json
    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
    
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Resolution}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }
```

### <a id="xml"></a>XML hazır
Şeması hakkında daha fazla bilgi için bkz. [bu](https://msdn.microsoft.com/library/mt269962.aspx) makalesi.

```csharp
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>100%</Width>
              <Height>100%/Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Resolution}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>   
```

## <a id="code_sample"></a>Video kodlayın ve .NET ile küçük resim oluşturma

Aşağıdaki kod örneği, aşağıdaki görevleri gerçekleştirmek için Media Services .NET SDK'sını kullanır:

* Bir kodlama işi oluşturun.
* Medya Kodlayıcısı standart Kodlayıcı bir başvuru alın.
* Hazır yük [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) veya [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) içeren küçük resim oluşturma için gereken bilgileri yanı sıra önceden kodlama. Bunu kaydetmek [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) veya [JSON](media-services-dotnet-generate-thumbnail-with-mes.md#json) bir dosya ve kullanım dosyasını yüklemek için aşağıdaki kod.
  
        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
* Tek bir kodlama görevi işe ekleyin. 
* Kodlanacak giriş varlığı belirtin.
* Kodlanmış varlığı içeren bir çıkış varlık oluşturun.
* İş ilerleme durumunu denetlemek için bir olay işleyicisi ekleyin.
* İşi Gönder.

Bkz: [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) makale için geliştirme ortamınızı kurma konusunda yönergeler.

```csharp
using System;
using System.Configuration;
using System.IO;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Threading;

namespace EncodeAndGenerateThumbnails
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

        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
        Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

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

            // Encode and generate the thumbnails.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Thumbnail Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Load the XML (or JSON) from the local file.
            string configuration = File.ReadAllText("ThumbnailPreset_JSON.json");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard Thumbnail task",
                    processor,
                    configuration,
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

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Aşağıdaki maddeler geçerlidir:

* Başlangıç/adım/aralığı için açık zaman damgaları kullanımını giriş kaynağı en az 1 dakika uzunluğunda olduğunu varsayar.
* Png/jpg/BmpImage öğeleri başlatma, adım ve dize öznitelikleri aralığı – bunlar olarak yorumlanabilir:
  
  * Örneğin "Başlangıç" negatif olmayan tamsayılar olmaları durumunda çerçeve numarası: "120",
  * Süre olarak % tam sayıysa kaynağı, örneğin "Başlat" göreli: "%15", VEYA
  * Ss zaman damgası... biçimi. Örneğin "Başlat": "00:01:00"
    
    Karışık ve yazarken gösterimler Lütfen eşleşen.
    
    Ayrıca, başlangıç özel makro de destekler: {, hangi içerik Not ilk "ilginç" çerçevesini belirlemeye çalışır en iyi}: (Başlangıç {iyi} olarak ayarlandığında adım ve aralığı göz ardı edilir)
  * Varsayılanlar: Başlangıç: {en iyi}
* Çıkış biçimi için her görüntü biçimi açıkça sağlanması gerekir: Jpg/Png/BmpFormat. MES JpgVideo JpgFormat için mevcut olduğunda, vb. ile eşleşir. Yeni bir görüntü codec bileşeni belirli makrosu OutputFormat sunar: {Index} olması gereken sunar (bir kez ve yalnızca bir kez) görüntü için Çıkış biçimleri.

## <a name="next-steps"></a>Sonraki adımlar

Denetleyebilirsiniz [işi ilerleme](media-services-check-job-progress.md) kodlama işinin beklemedeyken.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Media Services Encoding genel bakış](media-services-encode-asset.md)

