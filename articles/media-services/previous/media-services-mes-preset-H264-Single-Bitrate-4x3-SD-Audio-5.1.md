---
title: H264 Tekli bit hızı 4 x 3 SD ses 5.1 | Microsoft Docs
description: Konusuna genel bir fikir veren **H264 tekli bit hızı 4 x 3 SD ses 5.1** görev hazır.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: e55dd302-2f42-46b5-ae17-bd3c72329c03
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: juliako
ms.openlocfilehash: 53c429bfc32da4fd122036f259f9604d1cab875e
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50246613"
---
# <a name="h264-single-bitrate-4x3-sd-audio-51"></a>H264 Tekli bit hızı 4 x 3 SD ses 5.1
`Media Encoder Standard` kodlama işi oluştururken kullanabileceğiniz hazır kodlama kümesi tanımlar. Kullanabilir bir `preset name` hangi biçimine medya dosyanızı kodlamak istediğiniz belirtmek için. Veya kendi JSON veya XML tabanlı hazır (UTF-8 veya UTF-16 kodlamasını kullanıyor. oluşturabilirsiniz Ardından kodlayıcıya hazır özel geçirmeniz gerekir. Bu tarafından desteklenen tüm hazır adlarının listesi için `Media Encoder Standard` Kodlayıcı bkz [Media Encoder Standard için görev ön ayarları](media-services-mes-presets-overview.md).  
  
 Bu konu başlığı altında gösterilir `H264 Single Bitrate 4x3 SD Audio 5.1` XML ve JSON biçiminde hazır.  
  
 Bu önceden ayarlanmış bir bit hızı 1800 KB/sn ve AAC 5.1 ses ile tek bir MP4 dosyası üretir. Profili hakkında ayrıntılı bilgi için örnekleme hızını, vb. Bu bit hızı, hazır, XML veya JSON aşağıda tanımlanan inceleyin. Her öğeye anlamına gelir ve geçerli değerlerini her öğe için açıklamalar için bkz [Media Encoder Standard şeması](media-services-mes-schema.md).  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:02</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>1800</Bitrate>  
          <Width>640</Width>  
          <Height>480</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>1800</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>AACLC</Profile>  
      <Channels>6</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>384</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:02",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 1800,  
          "MaxBitrate": 1800,  
          "BufferWindow": "00:00:05",  
          "Width": 640,  
          "Height": 480,  
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
      "Channels": 6,  
      "SamplingRate": 48000,  
      "Bitrate": 384,  
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
```
