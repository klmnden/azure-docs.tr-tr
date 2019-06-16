---
title: H264 Android tekli bit hızı yüksek kaliteli SD | Microsoft Docs
description: Konusuna genel bir fikir veren **H264 Çoklu bit hızı yüksek kaliteli SD Android** görev hazır.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 4a190f24-ec6a-47e7-8bca-6294e064b15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 217c4874f0375aeb4d80162af1b8453a3f7f625f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61463523"
---
# <a name="h264-single-bitrate-high-quality-sd-for-android"></a>Android için H264 Single Bitrate Yüksek Kaliteli SD
`Media Encoder Standard` kodlama işi oluştururken kullanabileceğiniz hazır kodlama kümesi tanımlar. Kullanabilir bir `preset name` hangi biçimine medya dosyanızı kodlamak istediğiniz belirtmek için. Veya kendi JSON veya XML tabanlı hazır (UTF-8 veya UTF-16 kodlamasını kullanıyor. oluşturabilirsiniz Ardından kodlayıcıya hazır özel geçirmeniz gerekir. Bu tarafından desteklenen tüm hazır adlarının listesi için `Media Encoder Standard` Kodlayıcı bkz [Media Encoder Standard için görev ön ayarları](media-services-mes-presets-overview.md).  
  
 Bu konu başlığı altında gösterilir `H264 Single Bitrate High Quality SD for Android` XML ve JSON biçiminde hazır.  
  
 Bir bit hızı stereo AAC ses ve 500 Kb/sn ile bu hazır oluşturan tek bir MP4 dosyası. Profili hakkında ayrıntılı bilgi için örnekleme hızını, vb. Bu bit hızı, hazır, XML veya JSON aşağıda tanımlanan inceleyin. Bu hazır anlamına gelir ve geçerli değerler her hangi bir öğenin her öğe için açıklamalar için bkz [Media Encoder Standard şeması](media-services-mes-schema.md) konu.  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:05</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>500</Bitrate>  
          <Width>480</Width>  
          <Height>360</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>500</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
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
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:05",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Baseline",  
          "Level": "3",  
          "Bitrate": 500,  
          "MaxBitrate": 500,  
          "BufferWindow": "00:00:05",  
          "Width": 480,  
          "Height": 360,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
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
```
