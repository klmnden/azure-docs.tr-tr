---
title: Gelişmiş MES hazır özelleştirerek kodlama gerçekleştirmek | Microsoft Docs
description: Bu konu, Medya Kodlayıcısı standart görev hazır özelleştirerek gelişmiş kodlama gerçekleştirmek gösterilmiştir.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: 2a4ade25-e600-4bce-a66e-e29cf4a38369
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: juliako
ms.openlocfilehash: 9480e6f3f651611e5281968d6d1651bd39dda44f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a>Gelişmiş kodlama MES hazır ayarlarını özelleştirerek gerçekleştirin 

## <a name="overview"></a>Genel Bakış

Bu konu, Medya Kodlayıcısı standart hazır ayarlarını özelleştirme gösterilmektedir. [Medya Kodlayıcısı özel hazır ayarları kullanarak standart ile kodlama](media-services-custom-mes-presets-with-dotnet.md) konu .NET kodlama görev ve bu görevi yürüten bir iş oluşturmak için nasıl kullanılacağını gösterir. Bir hazır özelleştirildikten sonra kodlama göreve özel hazır sağlayın. 

>[!NOTE]
>XML örneklerinde aşağıda gösterildiği gibi bir XML hazır kullanıyorsanız, öğelerin sırasını doğru yazdığınızdan emin olun (örneğin, KeyFrameInterval SceneChangeDetection gelmelidir).
>

Bu konuda, aşağıdaki kodlama görevleri gerçekleştiren özel hazır gösterilmiştir.

## <a name="support-for-relative-sizes"></a>Göreli boyutları için destek

Küçük resimleri oluşturulurken her zaman çıkış genişlik ve yükseklik piksel cinsinden belirtmeniz gerekmez. [% 1,..., % 100] aralığında bir yüzde belirtebilirsiniz.

### <a name="json-preset"></a>JSON hazır
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a>XML hazır
    <Width>100%</Width>
    <Height>100%</Height>

## <a id="thumbnails"></a>Küçük resimler oluşturma

Bu bölümde, küçük resim oluşturur hazır özelleştirme gösterilmektedir. Aşağıda tanımlanan hazır küçük resimleri oluşturmak için gereken bilgileri yanı sıra, dosya kodlamak istediğiniz şekli hakkında bilgi içerir. Belgelenen MES hazır hiçbirini uygulayabileceğiniz [bu](media-services-mes-presets-overview.md) bölümünde ve küçük resim oluşturur kodu ekleyin.  

> [!NOTE]
> **SceneChangeDetection** aşağıdaki hazır ayarı yalnızca ayarlanabilir tek bit hızlı için video kodlama true olarak. Bir Çoklu bit hızlı video ve kümesi kodlaması, **SceneChangeDetection** için true, kodlayıcı bir hata döndürür.  
>
>

Şeması hakkında daha fazla bilgi için bkz: [bu](media-services-mes-schema.md) konu.

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
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
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

* Başlangıç/adım/aralığı için açık zaman damgaları kullanımını giriş kaynağı en az 1 dakika uzun olduğunu varsayar.
* Png/jpg/BmpImage öğeleri başlatma, adım ve dize öznitelikleri aralığı – bu olarak yorumlanacak:

  * Çerçeve sayısı negatif olmayan tamsayılar iseler, örneğin "Başlat": "120",
  * Göreli süresi % sonekine olarak ifade edilen, kaynak, örneğin "Başlat": "% 15", veya
  * Ss: dd: ifade edilen, zaman damgası... Biçim, örneğin "Başlat": "00: 01:00"

    Karışık ve yazarken gösterimler Lütfen eşleşmesi.

    Ayrıca, başlangıç özel makrosu de destekler: {, hangi içerik Not ilk "ilginç" çerçevesi belirlemeyi dener en iyi}: (adım ve aralık yok sayılır başlangıç {iyi} olarak ayarlandığında geçerlidir)
  * Varsayılan: Başlat: {en iyi}
* Çıktı biçimi her resim biçimi için açıkça sağlanması gerekiyor: Png/Jpg/BmpFormat. Varsa, MES JpgFormat JpgVideo vb. eşleşir. Yeni bir görüntü codec belirli makrosu OutputFormat sunar: {Index} olması gerekiyor hangi sunmak (bir kez ve yalnızca bir kez) görüntü Çıkış biçimleri.

## <a id="trim_video"></a>(Kırpma) video kırpma
Bu bölümde, küçük resim veya giriş sözde Ara dosyayı veya isteğe bağlı dosya olduğu giriş video kırpma Kodlayıcı hazır değiştirilmesi hakkında alınmaktadır. Kodlayıcı küçük veya yakalanan veya bir canlı akış alan Arşivlenmiş bir varlık kırpma için de kullanılabilir – bu ayrıntılarını kullanılabilir [bu blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

Videolarınızı kırpma için belgelenen MES hazır hiçbirini uygulayabileceğiniz [bu](media-services-mes-presets-overview.md) bölümünde ve değiştirme **kaynakları** (aşağıda gösterildiği gibi) öğesi. StartTime değerinin mutlak zaman damgaları giriş video ile eşleşmesi gerekir. Giriş videonun ilk çerçevesi 12:00:10.000 damgasıdır varsa, örneğin, ardından StartTime en az olmalıdır 12:00:10.000 büyük. Aşağıdaki örnekte, giriş videonuzun başlangıç zaman damgası sıfır olduğunu varsayalım. **Kaynakları** hazır başında yerleştirilmelidir.

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
Videolarınızı kırpma için belgelenen MES hazır hiçbirini uygulayabileceğiniz [burada](media-services-mes-presets-overview.md) ve değiştirme **kaynakları** (aşağıda gösterildiği gibi) öğesi.

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
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

## <a id="overlay"></a>Bir katmana oluşturma

Medya Kodlayıcısı standart bir yansımayı varolan bir video yer paylaşımı olanak tanır. Şu anda aşağıdaki biçimlerden desteklenir: png, jpg, GIF ve bmp. Aşağıda tanımlanan önceden ayarlanmış bir video katmana temel bir örnektir.

Önceden belirlenmiş bir dosya tanımlamanın yanı sıra, ayrıca hangi varlık katmana görüntü dosyasıdır ve hangi dosya video kaynağı görüntü kaplama istediğiniz bilmeniz Media Services izin vermek de vardır. Video dosyası olmak zorundadır **birincil** dosya.

.NET kullanıyorsanız, tanımlanan .NET örneği aşağıdaki iki işlevi ekleyin [bu](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) konu. **UploadMediaFilesFromFolder** işlevi varlık birincil dosyada mp4 dosyası ayarlar ve dosyaları bir klasöründen (örneğin, BigBuckBunny.mp4 ve Image001.png) yükler. **EncodeWithOverlay** işlevini kullanır, (örneğin, aşağıdaki hazır) geçirilen özel hazır dosya kodlama görevi oluşturmak için.


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
> Bir katmana opaklık ayarı desteklenmiyor.
>
> Kaynak video dosyanızı ve katmana resim dosyası aynı varlık olması gerekir ve video dosyasını bu varlık birincil dosya olarak ayarlanması gerekir.
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
                  "OverlayLoopCount": 1,
                  "InputLoop": true
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
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
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
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
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


## <a id="silent_audio"></a>Hiç ses giriş sahip olduğunda sessiz bir ses kaydı ekleme
Yalnızca video ve ses yok içeren Kodlayıcı girdi gönderirseniz, varsayılan olarak, çıkış varlık sonra yalnızca video verileri içeren dosyaları içerir. Bazı oynatıcıları gibi çıkış akışları işlemek mümkün olmayabilir. Bu senaryoda çıkış sessiz ses parça eklemek için Kodlayıcı zorlamak için bu ayarı kullanın.

Hiçbir ses giriş sahip olduğunda, sessiz bir ses parçası içeren bir varlık oluşturmak için Kodlayıcı zorlamak için "InsertSilenceIfNoAudio" değerini belirtin.

Kısmında belgelenen MES hazır hiçbirini uygulayabileceğiniz [bu](media-services-mes-presets-overview.md) bölümünde ve aşağıdaki değişikliği yapın:

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
Müşteriler taramalı içeriğinin otomatik olarak XML'deki aralıklı olmasını isterseniz herhangi bir şey yapmanız gerekmez. XML'deki otomatik tarama (varsayılan) olduğunda MES aralıklı çerçeveler otomatik algılanmasını yapar ve yalnızca aralıklı olarak işaretlenen çerçeveler XML'deki interlaces.

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
Bu bölüm iki salt ses MES hazır gösterir: AAC ses ve AAC iyi kaliteli ses.

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

Aşağıdaki örnekte, iki veya daha fazla video dosyaları birleştirmek için bir hazır nasıl oluşturacağınız gösterilmiştir. Bir üstbilgi ya da bir toplamı ana videoya eklemek istediğinizde en yaygın senaryodur. Birlikte düzenlenen video dosyaları özellikleri (ekran çözünürlüğü, kare hızı, ses parça sayısı, vb.) paylaştığınızda hedeflenen kullanılır. Videolar farklı çerçeve oranlarının ya da ses izleri farklı sayıda karıştırmak değil için dikkatli olun.

>[!NOTE]
>Giriş video klip çözümleme bakımından tutarlı birleştirme özelliğinin geçerli tasarımı bekliyor kare hızı vs. 

### <a name="requirements-and-considerations"></a>Gereksinimleri ve konular

* Giriş videolar yalnızca bir ses parçası olması gerekir.
* Giriş videolar tüm aynı kare hızı olması gerekir.
* Videolarınızı ayrı varlıklar karşıya yükleme ve videolar her varlık birincil dosya olarak ayarlamanız gerekir.
* Videolarınızı süresini bilmeniz gerekir.
* Aşağıdaki hazır örnekleri varsayar tüm giriş videoları sıfır damgasıyla başlatın. Farklı başlangıç zaman damgası, videolar varsa, genellikle dinamik arşivlerle olduğu gibi StartTime değerlerini değiştirmeniz gerekir.
* JSON hazır giriş varlıklar AssetID değerlerini açık başvurular yapar.
* Örnek kod, JSON hazır "C:\supportFiles\preset.json" gibi yerel bir dosyaya kaydedilir varsayar. Ayrıca, iki varlıklar iki video dosyaları karşıya yükleyerek oluşturulduğunu ve sonuç AssetID değerleri biliyorsanız varsayar.
* Kod parçacığını ve JSON hazır iki video dosyaları birleştirme örneği gösterir. Tarafından ikiden fazla videolara genişletebilirsiniz:

  1. Arama görev. Daha fazla videolar sırada sürekli olarak eklemek için InputAssets.Add().
  2. Karşılık gelen yapmadan JSON "Kaynaklar" öğesine aynı sırada daha fazla girdi ekleyerek düzenler.

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

Birleştirmek istediğiniz varlıklar kimliklerini ve her video için uygun zaman diliminin önceden özel güncelleştirin.

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

## <a id="crop"></a>Medya Kodlayıcısı standart ile kırpma video
Bkz: [kırpma Medya Kodlayıcısı standart ile video](media-services-crop-video.md) konu.

## <a id="no_video"></a>Video giriş sahip olduğunda bir video kaydı ekleme

Yalnızca ses ve video, içeren Kodlayıcı girdi gönderirseniz, varsayılan olarak, çıkış varlık sonra yalnızca ses verilerini içeren dosyaları içerir. Azure Media Player dahil olmak üzere bazı oynatıcıları (bkz [bu](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) gibi akışlarını işlemeye mümkün olmayabilir. Bu senaryo çıktısında tek renkli bir video izlemek eklemek için Kodlayıcı zorlamak için bu ayarı kullanın.

> [!NOTE]
> Encoder çıkış video izleme eklemek için zorlama çıktı boyutunu artırır, varlık ve böylece maliyeti ücrete kodlama görevi. Bu sonuç artış aylık ücretlerinizi uygun bir etkiye sahip olduğunu doğrulamak için testler.
>

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Yalnızca en düşük bit hızlı video ekleme

Bir Çoklu bit hızı gibi önceden kodlama kullanıyorsanız varsayalım ["H264 Çoklu bit hızı 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) video dosyaları ve yalnızca ses dosyaları bir karışımını içeren akış, tüm giriş kataloğunuzu kodlamak için. Hiçbir video giriş sahip olduğunda bu senaryoda, her çıktı bit hızı video ekleme aksine en düşük hızı yalnızca tek renkli bir video izlemek eklemek için Kodlayıcı zorlamak isteyebilirsiniz. Bunu başarmak için kullanmanız gerekir **InsertBlackIfNoVideoBottomLayerOnly** bayrağı.

Kısmında belgelenen MES hazır hiçbirini uygulayabileceğiniz [bu](media-services-mes-presets-overview.md) bölümünde ve aşağıdaki değişikliği yapın:

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

XML kullanırken koşulu kullanmak için bir öznitelik olarak "InsertBlackIfNoVideoBottomLayerOnly" = **H264Video** öğesi ve koşul için bir öznitelik olarak "InsertSilenceIfNoAudio" = **AACAudio**.

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

### <a name="inserting-video-at-all-output-bitrates"></a>Bit çıkış video hiç ekleme
Bir Çoklu bit hızı gibi önceden kodlama kullanıyorsanız varsayalım ["H264 Çoklu bit hızı 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) video dosyaları ve yalnızca ses dosyaları bir karışımını içeren akış, tüm giriş kataloğunuzu kodlamak için. Bu senaryoda, giriş hiçbir video olduğunda tek renkli bir video izlemek hiç çıkış bit eklemek için Kodlayıcı zorlamak isteyebilirsiniz. Bu, çıktı sağlar varlıklar video izler ve ses izleri sayısı göre tüm homojen. Bunun için "InsertBlackIfNoVideo" bayrağı belirtmeniz gerekir.

Kısmında belgelenen MES hazır hiçbirini uygulayabileceğiniz [bu](media-services-mes-presets-overview.md) bölümünde ve aşağıdaki değişikliği yapın:

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

XML kullanırken koşulu kullanmak için bir öznitelik olarak "InsertBlackIfNoVideo" = **H264Video** öğesi ve koşul için bir öznitelik olarak "InsertSilenceIfNoAudio" = **AACAudio**.

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
[Medya Kodlayıcısı standart](media-services-dotnet-encode-with-media-encoder-standard.md) 0/90/180/270 açıları tarafından dönüşünü destekler. "Otomatik" nerede gelen video dosyasında döndürme meta verileri algılamak ve bunun için dengelemek için çalışır, varsayılan davranıştır. Aşağıdakiler dahil **kaynakları** tanımlanan hazır ayarlarından birini öğesine [bu](media-services-mes-presets-overview.md) bölümü:

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

Ayrıca bkz [bu](media-services-mes-schema.md#PreserveResolutionAfterRotation) döndürme maaş tetiklendiğinde Kodlayıcı hazır genişlik ve yükseklik ayarlarını yorumlaması hakkında daha fazla bilgi için konu.

"0" değerini döndürme meta verileri, video giriş varsa yoksaymak için Kodlayıcı belirtmek için kullanabilirsiniz.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Media Services kodlama a genel bakış](media-services-encode-asset.md)
