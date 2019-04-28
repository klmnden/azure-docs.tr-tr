---
title: H264 Android tekli bit hızı 720p | Microsoft Docs
description: Konusuna genel bir fikir veren **H264 tekli bit hızı 720p Android** görev hazır.
author: WenJason
manager: digimobile
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 4f9569a3-5aca-4fea-8242-024925a8af90
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 03/19/2019
ms.date: 04/01/2019
ms.author: v-jay
ms.openlocfilehash: da44cf33882d2658b20f117053d486177117a5a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61131027"
---
# <a name="h264-single-bitrate-720p-for-android"></a>Android için H264 Single Bitrate 720p
`Media Encoder Standard` kodlama işi oluştururken kullanabileceğiniz hazır kodlama kümesi tanımlar. Kullanabilir bir `preset name` hangi biçimine medya dosyanızı kodlamak istediğiniz belirtmek için. Veya kendi JSON veya XML tabanlı hazır (UTF-8 veya UTF-16 kodlamasını kullanıyor. oluşturabilirsiniz Ardından kodlayıcıya hazır özel geçirmeniz gerekir. Bu tarafından desteklenen tüm hazır adlarının listesi için `Media Encoder Standard` Kodlayıcı bkz [Media Encoder Standard için görev ön ayarları](media-services-mes-presets-overview.md).  
  
Bu konu başlığı altında gösterilir `H264 Single Bitrate 720p for Android` XML ve JSON biçiminde hazır.  
  
Bu hazır oluşturan tek bir MP4 dosyası ile 2000 KB/sn ve stereo AAC bir bit hızı. Profili hakkında ayrıntılı bilgi için örnekleme hızını, vb. Bu bit hızı, hazır, XML veya JSON aşağıda tanımlanan inceleyin. Bu hazır anlamına gelir ve geçerli değerler her hangi bir öğenin her öğe için açıklamalar için bkz [Media Encoder Standard şeması](media-services-mes-schema.md) konu.  
  
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
