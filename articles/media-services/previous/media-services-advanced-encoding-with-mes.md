---
title: MES ön Ayarları özelleştirerek Gelişmiş encoding gerçekleştirme | Microsoft Docs
description: Bu konuda, Media Encoder Standard görev ön Ayarları özelleştirerek Gelişmiş encoding gerçekleştirme gösterilmektedir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 2a4ade25-e600-4bce-a66e-e29cf4a38369
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: fadf1aa54f525fb3d4c414161583f8a89f2e4c05
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61230262"
---
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a>MES ön Ayarları özelleştirerek Gelişmiş encoding gerçekleştirme 

## <a name="overview"></a>Genel Bakış

Bu konuda, Media Encoder Standard hazır ayarlarını özelleştirme gösterilmektedir. [Kullanarak Media Encoder özel önayarların kullanılmasına Standard ile kodlama](media-services-custom-mes-presets-with-dotnet.md) konu .NET bir kodlama görevi ve bu görevi yürüten bir iş oluşturmak için nasıl kullanılacağını gösterir. Önceden ayarlanmış özelleştirildikten sonra özel önayarların kullanılmasına kodlama görevine sağlayın. 

Aşağıdaki XML örneklerde gösterildiği gibi bir XML ön ayarı kullanıyorsanız, öğelerin sırasını doğru yazdığınızdan emin olun (örneğin, KeyFrameInterval SceneChangeDetection gelmelidir).

> [!NOTE] 
> Media Encoder Standard'ın gelişmiş Media Services v2 özelliklerin çoğu şu anda v3 sürümünde kullanılabilir değil. Daha fazla bilgi için [özellik boşlukları](https://docs.microsoft.com/azure/media-services/latest/migrate-from-v2-to-v3#feature-gaps-with-respect-to-v2-apis).

## <a name="support-for-relative-sizes"></a>Göreli boyutları için destek

Küçük resimleri oluştururken, her zaman çıkış genişliğini ve yüksekliğini piksel cinsinden belirtmek gerekmez. [% 1,..., % 100] aralığında bir yüzde belirtebilirsiniz.

### <a name="json-preset"></a>JSON hazır
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a>XML hazır
    <Width>100%</Width>
    <Height>100%</Height>

## <a id="thumbnails"></a>Küçük resim oluşturma

Bu bölümde, küçük resim oluşturan bir hazır özelleştirme işlemi gösterilmektedir. Aşağıda tanımlanan hazır, küçük resim oluşturma için gereken bilgilerin yanı sıra, dosya kodlamak istediğiniz şekli hakkında bilgi içerir. MES ön ayarları belgelenen birini alabilir [bu](media-services-mes-presets-overview.md) bölümünde ve küçük resim oluşturan kodu ekleyin.  

> [!NOTE]
> **SceneChangeDetection** aşağıdaki hazır ayarı yalnızca ayarlanabilir bir tekli bit hızı için video Kodladığınız gerekiyorsa true olarak. Bir Çoklu bit hızına sahip video ve kümesi kodlama **SceneChangeDetection** için true, kodlayıcı bir hata döndürür.  
>
>

Şeması hakkında daha fazla bilgi için bkz. [bu](media-services-mes-schema.md) konu.

Gözden geçirdiğinizden emin olun [konuları](#considerations) bölümü.

### <a id="json"></a>JSON hazır
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
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
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
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a id="xml"></a>XML hazır
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
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıdaki maddeler geçerlidir:

* Başlangıç/adım/aralığı için açık zaman damgaları kullanımını giriş kaynağı en az 1 dakika uzunluğunda olduğunu varsayar.
* Png/jpg/BmpImage öğeleri başlatma, adım ve dize öznitelikleri aralığı – bunlar olarak yorumlanabilir:

  * Örneğin "Başlangıç" negatif olmayan tamsayılar olmaları durumunda çerçeve numarası: "120",
  * Süre olarak % tam sayıysa kaynağı, örneğin "Başlat" göreli: "%15", VEYA
  * Ss zaman damgası... Örneğin "Başlangıç" biçimi: "00:01:00"

    Karışık ve yazarken gösterimler Lütfen eşleşen.

    Ayrıca, başlangıç özel makro de destekler: {, hangi içerik Not ilk "ilginç" çerçevesini belirlemeye çalışır en iyi}: (Başlangıç {iyi} olarak ayarlandığında adım ve aralığı göz ardı edilir)
  * Varsayılan olarak: Başlangıç: {en iyi}
* Çıkış biçimi için her görüntü biçimi açıkça sağlanması gerekir: Jpg/Png/BmpFormat. MES JpgVideo JpgFormat için mevcut olduğunda, vb. ile eşleşir. Yeni bir görüntü codec bileşeni belirli makrosu OutputFormat sunar: {Index} olması gereken sunar (bir kez ve yalnızca bir kez) görüntü için Çıkış biçimleri.

## <a id="trim_video"></a>(Kırpma) bir videoyu Kırp
Bu bölümde, küçük resim veya giriş sözde Ara dosyayı ya da isteğe bağlı dosya olduğu giriş Videoyu Kırp Kodlayıcı hazır değiştirilmesi hakkında konuşuyor. Kodlayıcı, küçük veya trim yakalanan veya bir canlı akıştan Arşivlenmiş bir varlık için de kullanılabilir – için bu Ayrıntılar kullanılabilir [bu blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Videolarınızı kırpmak için belgelenen MES ön ayarları birini alabilir [bu](media-services-mes-presets-overview.md) bölüm ve değiştirme **kaynakları** (aşağıda gösterildiği gibi) öğesi. StartTime değerinin giriş videosunun mutlak zaman damgalarının eşleşmesi gerekir. Giriş videosunun ilk karesine 12:00:10.000 bir zaman damgası varsa, örneğin, ardından StartTime en az olmalıdır 12:00:10.000 ve büyük. Aşağıdaki örnekte, giriş videosunun başlayan bir zaman damgası sıfır olduğunu varsayıyoruz. **Kaynakları** hazır başlangıcına yerleştirilmelidir.

### <a id="json"></a>JSON hazır
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a name="xml-preset"></a>XML hazır
Videolarınızı kırpmak için belgelenen MES ön ayarları birini alabilir [burada](media-services-mes-presets-overview.md) ve değiştirme **kaynakları** (aşağıda gösterildiği gibi) öğesi.

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
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
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

## <a id="overlay"></a>Bir katman oluşturma

Media Encoder Standard, mevcut bir videoya bir yansımayı kaplama olanak tanır. Şu anda aşağıdaki biçimleri desteklenmektedir: png, jpg, GIF ve bmp. Aşağıda tanımlanan önceden ayarlanmış bir video yer paylaşımı temel bir örnektir.

Önceden oluşturulmuş bir dosya tanımlamanın yanı sıra, ayrıca Media Services'ın görüntüyü kaplama istediğiniz varlığı hangi dosyasında katmana görüntü ve video kaynağı dosyasıdır bilmeniz izin vermek vardır. Video dosyası olmak zorundadır **birincil** dosya.

.NET kullanıyorsanız, tanımlanan .NET örnek aşağıdaki iki işlevi ekleyin [bu](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) konu. **UploadMediaFilesFromFolder** işlevi dosyaları bir klasöründen (örneğin, BigBuckBunny.mp4 ve Image001.png) yükler ve varlık birincil dosyada mp4 dosyasını ayarlar. **EncodeWithOverlay** işlevi kullanır (örneğin, aşağıdaki hazır) geçilen özel hazır dosya kodlama görevi oluşturur.


    static public IAsset UploadMediaFilesFromFolder(string folderPath)
    {
        IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
    
        foreach (var af in asset.AssetFiles)
        {
            // The following code assumes 
            // you have an input folder with one MP4 and one overlay image file.
            if (af.Name.Contains(".mp4"))
                af.IsPrimary = true;
            else
                af.IsPrimary = false;
    
            af.Update();
        }
    
        return asset;
    }

    static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
    {
        // Declare a new job.
        IJob job = _context.Jobs.Create("Media Encoder Standard Job");
        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(customPresetFileName);

        // Create a task
        ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input assets to be encoded.
        // This asset contains a source file and an overlay file.
        task.InputAssets.Add(assetSource);

        // Add an output asset to contain the results of the job. 
        task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
        job.Submit();
        job.GetExecutionProgressTask(CancellationToken.None).Wait();

        return job.OutputMediaAssets[0];
    }


> [!NOTE]
> Geçerli sınırlamalar:
>
> Katmana opaklık ayarı desteklenmiyor.
>
> Video kaynak dosyanızı ve katmana görüntü dosyası aynı varlığı olması gerekir ve video dosyasını bu varlık birincil dosyasında olarak ayarlanması gerekir.
>
>

### <a name="json-preset"></a>JSON hazır
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a name="xml-preset"></a>XML hazır
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


## <a id="silent_audio"></a>Hiç ses giriş sahip olduğunda bir sessiz bir ses kaydı ekleme
Yalnızca video ve ses yok içeren Kodlayıcı girdi gönderdiğiniz varsayılan olarak, çıktı varlık yalnızca video veriler içeren dosyalar içerir. Bazı oynatıcıları gibi çıkış akışları işlemek mümkün olmayabilir. Bu senaryodaki çıkış sessiz bir ses kaydı eklemek için Kodlayıcı zorlamak için bu ayarı kullanabilirsiniz.

Hiç ses giriş varsa, sessiz bir ses kaydı içeren bir varlık oluşturmak için Kodlayıcı zorlamak için "InsertSilenceIfNoAudio" değerini belirtin.

Herhangi bir kısmında belgelenen MES ön ayarları uygulayabileceğiniz [bu](media-services-mes-presets-overview.md) bölüm ve aşağıdaki değişikliği yapın:

### <a name="json-preset"></a>JSON hazır
    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

### <a name="xml-preset"></a>XML hazır
    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

## <a id="deinterlacing"></a>XML'deki otomatik taramayı devre dışı bırak
Müşteriler, bunlar taramalı içeriğinin otomatik olarak serbest aralıklı olmasını istiyorsanız herhangi bir şey yapmanız gerekmez. XML'deki otomatik taramayı (varsayılan) olduğunda MES aralıklı çerçeve otomatik algılanması yapar ve yalnızca aralıklı olarak işaretlenmiş çerçeveler XML'deki interlaces.

XML'deki otomatik taramayı kapatabilirsiniz. Bu seçenek önerilmez.

### <a name="json-preset"></a>JSON hazır
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

### <a name="xml-preset"></a>XML hazır
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


## <a id="audio_only"></a>Yalnızca ses hazır
Bu bölüm iki yalnızca ses MES ön ayarları gösterir: AAC ses ve AAC kaliteli ses.

### <a name="aac-audio"></a>AAC ses
    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a name="aac-good-quality-audio"></a>AAC kaliteli ses
    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <a id="concatenate"></a>İki veya daha fazla video dosyaları birleştirme

Aşağıdaki örnekte, iki veya daha fazla video dosyaları birleştirmek için önceden ayarlanmış nasıl oluşturabileceğiniz gösterilmektedir. Bir üst bilgi veya bir tanıtım için ana video eklemek istediğinizde en yaygın senaryodur. Video dosyaları birlikte düzenlenmekte olan özellikleri (görüntü çözünürlüğünü, kare hızı, ses sayısı, vb.) paylaştığınızda hedeflenen kullanılır. Videoları farklı kare hızları, ya da ses parçaları farklı sayıda karıştırmamaya ilgileniriz.

>[!NOTE]
>Giriş video klipleri çözümleme bakımından tutarlı olduğunu birleştirme özelliğinin geçerli tasarım bekliyor kare hızını vb. 

### <a name="requirements-and-considerations"></a>Gereksinimleri ve konular

* Giriş videoları yalnızca bir ses parçası olmalıdır.
* Giriş video tümü aynı kare hızı olması gerekir.
* Ayrı varlıklarına videolarınızı karşıya yükleyin ve videoları her varlık birincil dosyasında ayarlamanız gerekir.
* Videolarınızı süresi bilmeniz gerekir.
* Önceden oluşturulmuş örneklere varsayar tüm giriş videoları ile sıfır, bir zaman damgası başlatın. Videoları farklı başlangıç zaman damgası, varsa, Canlı arşivlerinizin normalde olduğu gibi StartTime değerleri değiştirmeniz gerekir.
* JSON önceden ayarlanmış açık başvuruların giriş varlıkları AssetID değerlerini sağlar.
* Örnek kod, JSON hazır "C:\supportFiles\preset.json" gibi yerel bir dosyaya kaydedilir varsayar. Ayrıca, iki varlıklar iki video dosyaları karşıya yükleyerek oluşturulduğunu ve sonuç AssetID değerleri bildiğiniz varsayılır.
* JSON ve kod parçacığı önceden ayarlanmış iki video dosyaları birleştirme, bir örnek gösterir. İkiden fazla videolara tarafından genişletebilirsiniz:

  1. Arama görev. Tekrar tekrar sıraya göre daha fazla video eklemek için InputAssets.Add().
  2. Karşılık gelen yapmak için JSON "Kaynaklar" öğesinde aynı sırada daha fazla giriş ekleyerek düzenler.

### <a name="net-code"></a>.NET kodu

    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();

    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");

    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);

    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job.
    // This output is specified as AssetCreationOptions.None, which
    // means the output asset is not encrypted.
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);

    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

### <a name="json-preset"></a>JSON hazır

Birleştirmek istediğiniz varlıkların kimlikleri ve her video için uygun zaman diliminin ile önceden özel güncelleştirin.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
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
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <a id="crop"></a>Media Encoder Standard ile videoları kırpma
Bkz: [Media Encoder Standard ile videoları kırpma](media-services-crop-video.md) konu.

## <a id="no_video"></a>Giriş video sahip olduğunda bir video kaydı ekleme

Yalnızca ses ve video, içeren Kodlayıcı girdi gönderdiğiniz varsayılan olarak, çıktı varlık yalnızca ses veriler içeren dosyalar içerir. Azure Media Player dahil, bazı oyuncuların (bkz [bu](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) bu tür akışlar işlemek mümkün olmayabilir. Bu senaryonun çıktısında tek renkli bir video izlemek eklemek için Kodlayıcı zorlamak için bu ayarı kullanabilirsiniz.

> [!NOTE]
> Kodlayıcı çıkışı video izleme eklemek için zorlama çıkış boyutunu artırır, varlık ve böylece ücret kodlama görevi. Bu sonuç artışı aylık ücretlerinizle büyüklükteki bir etkiye sahip olduğunu doğrulamak için testleri çalıştırmanız gerekir.
>

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Yalnızca en düşük hızı video ekleme

Bir Çoklu bit hızı gibi önceden kodlama kullanmakta olduğunuz varsayalım ["H264 Çoklu bit hızı 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) video dosyaları ve yalnızca ses dosyaları bir karışımını içeren akış, tüm giriş kataloğunuzu kodlamak için. Video, giriş sahip olduğunda bu senaryoda, her çıkış hızı video ekleme aksine en düşük hızı yalnızca tek renkli bir video izleme eklemek için Kodlayıcı zorlamak isteyebilirsiniz. Bunu başarmak için kullanmanız gerekir **InsertBlackIfNoVideoBottomLayerOnly** bayrağı.

Herhangi bir kısmında belgelenen MES ön ayarları uygulayabileceğiniz [bu](media-services-mes-presets-overview.md) bölüm ve aşağıdaki değişikliği yapın:

#### <a name="json-preset"></a>JSON hazır
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML hazır

XML kullanırken koşulu kullanmak için bir özniteliği olarak "InsertBlackIfNoVideoBottomLayerOnly" = **H264Video** öğesi ve koşul için bir özniteliği olarak "InsertSilenceIfNoAudio" = **AACAudio**.

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideoBottomLayerOnly">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .
```

### <a name="inserting-video-at-all-output-bitrates"></a>Bit hızlarına dönüştürme çıktısını hiç video ekleme
Bir Çoklu bit hızı gibi önceden kodlama kullanmakta olduğunuz varsayalım ["H264 Çoklu bit hızı 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) video dosyaları ve yalnızca ses dosyaları bir karışımını içeren akış, tüm giriş kataloğunuzu kodlamak için. Bu senaryoda, video, giriş sahip olduğunda tek renkli bir video izleyin, hiç çıkış bit hızlarında eklemek için Kodlayıcı zorlamak isteyebilirsiniz. Bu, çıkış sağlar varlıklardır video izleyen ve ses parçaları sayısı ile ilgili tüm homojen. Bunu başarmak için "InsertBlackIfNoVideo" bayrak belirtmeniz gerekir.

Herhangi bir kısmında belgelenen MES ön ayarları uygulayabileceğiniz [bu](media-services-mes-presets-overview.md) bölüm ve aşağıdaki değişikliği yapın:

#### <a name="json-preset"></a>JSON hazır
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML hazır

XML kullanırken koşulu kullanmak için bir özniteliği olarak "InsertBlackIfNoVideo" = **H264Video** öğesi ve koşul için bir özniteliği olarak "InsertSilenceIfNoAudio" = **AACAudio**.

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideo">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .  
```

## <a id="rotate_video"></a>Bir video Döndür
[Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) döndürme açısı tarafından 0/90/180/270 destekler. Varsayılan "Auto" nerede gelen video dosyasına döndürme meta verileri algılamak ve için dengelemek için çalışır, davranıştır. Şunlar **kaynakları** öğe içinde tanımlanan hazır birine [bu](media-services-mes-presets-overview.md) bölümü:

### <a name="json-preset"></a>JSON hazır
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...
### <a name="xml-preset"></a>XML hazır
    <Sources>
           <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Ayrıca bkz [bu](media-services-mes-schema.md#PreserveResolutionAfterRotation) döndürme maaş tetiklendiğinde Kodlayıcısı önayarını genişlik ve yükseklik ayarlarında nasıl yorumlayacağını hakkında daha fazla bilgi için konu.

"0" değeri, dönüş meta verileri, varsa, video girişte yok saymak için Kodlayıcı belirtmek için kullanabilirsiniz.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Media Services Encoding genel bakış](media-services-encode-asset.md)
