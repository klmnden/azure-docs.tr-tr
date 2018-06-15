---
title: H264 Android Çoklu bit hızı 720p | Microsoft Docs
description: Konuyu genel bir fikir veren **H264 Çoklu bit hızı Android 720p** görev Önayar.
author: Juliako
manager: cfowler
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 4f9569a3-5aca-4fea-8242-024925a8af90
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 281ea6309831ad535ba62d67d0518e36e0c81df0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788567"
---
# <a name="h264-single-bitrate-720p-for-android"></a>H264 Çoklu bit hızı 720p Android için
`Media Encoder Standard` kodlama işler oluştururken kullanabileceğiniz hazır kodlama kümesi tanımlar. Kullanabilir bir `preset name` hangi biçimine medya dosyanızın kodlamak istediğiniz belirtmek için. Veya, kendi JSON veya XML tabanlı hazır (UTF-8 veya UTF-16 kodlamasını kullanıyor. oluşturabilirsiniz Ardından kodlayıcıya önceden özel geçirirsiniz. Bu tarafından desteklenen tüm hazır adlarının listesi için `Media Encoder Standard` encoder bkz [Medya Kodlayıcısı standart için görev hazır](media-services-mes-presets-overview.md).  
  
Bu konuda gösterilmektedir `H264 Single Bitrate 720p for Android` XML ve JSON biçiminde hazır.  
  
Bu önceden belirlenmiş üretir tek bir MP4 dosyası 2000 kbps stereo AAC ve bit hızı ile. Profili hakkında ayrıntılı bilgi için örnekleme hızını bu vb. bit hızı, önceden, XML veya JSON aşağıda tanımlanan inceleyin. Her öğe için hangi her bir öğe bu hazır anlamına gelir ve geçerli değerleri açıklamalar için bkz [Medya Kodlayıcısı standart şema](media-services-mes-schema.md) konu.  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:05</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>2000</Bitrate>  
          <Width>1280</Width>  
          <Height>720</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3.1</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>2000</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>AACLC</Profile>  
      <Channels>2</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>192</Bitrate>  
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
          "Level": "3.1",  
          "Bitrate": 2000,  
          "MaxBitrate": 2000,  
          "BufferWindow": "00:00:05",  
          "Width": 1280,  
          "Height": 720,  
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
      "Bitrate": 192,  
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
