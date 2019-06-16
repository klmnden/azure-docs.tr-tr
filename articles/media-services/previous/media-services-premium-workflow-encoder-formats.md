---
title: Media Encoder Premium iş akışı biçimleri ve codec bileşenleri | Microsoft Docs
description: Bu konu, Media Encoder Premium iş akışı biçimleri biçimleri ve codec bileşenleri hakkında genel bir bakış sağlar
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
ms.date: 03/19/2019
ms.author: juliako;anilmur
ms.openlocfilehash: 25f32750b612bb66f23eb19c378f7935689f3a73
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61463098"
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder Premium iş akışı biçimleri ve codec bileşenleri

> [!NOTE]
> Premium Kodlayıcı sorular için e-posta mepd@microsoft.com.
> 
> Bu konuda tartışılan Media Encoder Premium iş akışı medya işlemci Çin'de kullanılamaz. 

Bu belge, girdi ve çıktı dosya biçimlerini ve genel Önizleme sürümü tarafından desteklenen codec bileşenleri listesini içeren **Media Encoder Premium iş akışı** Kodlayıcı.

[Media Encoder Premium iş akışı giriş biçimleri ve codec bileşenleri](#input_formats)

Media Encoder Premium iş akışı Çıkış biçimleri ve codec bileşenleri

**Media Encoder Premium iş akışı** destekler Kapalı Açıklamalı Altyazı açıklanan [bu](#closed_captioning) bölümü. 

## <a id="input_formats"></a>Media Encoder Premium iş akışı giriş biçimleri ve codec bileşenleri

Aşağıdaki bölümde, girdi olarak bu medya işlemci desteklediği codec bileşenleri ve dosya biçimlerini listelenmektedir.

### <a name="input-containerfile-formats"></a>Giriş kapsayıcısı/dosya biçimleri

* Adobe® Flash® F4V
* MXF/SMPTE 377M
* GXF
* MPEG-2 aktarım akışı
* MPEG-2 Program akışları
* MPEG-4/MP4
* Windows Media/ASF
* AVI (sıkıştırılmamış 8 bit/10 bit)

### <a name="input-video-codecs"></a>Giriş video codec bileşenleri

* AVC 8 bit/10-en fazla 4 bit: 2:2, avcıntra
* Avid DNxHD (mxf biçiminde)
* DVCPro/DVCProHD (in MXF)
* HEVC/H.265, ana ve ana 10 profili
* JPEG2000
* MPEG-2 (422 profili ve yüksek düzeye kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitler dahil)
* MPEG-1
* Windows Media Video/VC-1

### <a name="input-audio-codecs"></a>Giriş ses codec bileşenleri

* AES (SMPTE 331 M ve 302 M, AES3-2003)
* Dolby® E
* Dolby® Digital (AC3)
* AAC (AAC-LC, AAC-HE ve AAC-HEv2; en fazla 5.1)
* MPEG Katman 2
* MP3 (MPEG-1 Ses Katmanı 3)
* Windows Media Ses
* WAV/PCM

## <a id="output_format"></a>Media Encoder Premium iş akışı Çıkış biçimleri ve codec bileşenleri

Aşağıdaki bölümde, bu ortam işlemciden çıktı olarak desteklenen codec bileşenleri ve dosya biçimlerini listelenmektedir.

### <a name="output-containerfile-formats"></a>Çıkış kapsayıcısı/dosya biçimleri

* Adobe® Flash® F4V
* MXF (OP1a, XDCAM ve AS02)
* DPP (AS11 dahil)
* GXF
* MPEG-4/MP4
* Windows Media/ASF
* AVI (sıkıştırılmamış 8 bit/10 bit)
* Kesintisiz akış dosyası biçimi (PIFF 1.3)
* MPEG-TS 

### <a name="output-video-codecs"></a>Çıkış Video codec bileşenleri

* AVC (H.264; 8-bit; yüksek profiline kadar 5.2; 4 K Ultra HD; düzeyi AVC içi)
* Avid DNxHD (mxf biçiminde)
* DVCPro/DVCProHD (in MXF)
* MPEG-2 (422 profili ve yüksek düzeye kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitler dahil)
* MPEG-1
* Windows Media Video/VC-1
* JPEG küçük resim oluşturma
* HEVC (H.265; 8 bitlik ve ana ve ana 10 profili 10 bit)

  Belirli senaryolarda HDR 10 için lütfen desteğe mepd@microsoft.com daha fazla bilgi için


### <a name="output-audio-codecs"></a>Çıkış ses codec bileşenleri

* AES (SMPTE 331 M ve 302 M, AES3-2003)
* Dolby® Digital (AC3)
* Dolby® dijital artı (E-AC3) kadar 7.1
* AAC (AAC-LC, AAC-HE ve AAC-HEv2; en fazla 5.1)
* MPEG Katman 2
* MP3 (MPEG-1 Ses Katmanı 3)
* Windows Media Ses

>[!NOTE]
>Dolby® dijital için (AC3) kodlama, çıkış yalnızca bir ISO MP4 dosyasına yazılabilir.

## <a id="closed_captioning"></a>Kapalı açıklamalı alt yazı desteği

Üzerinde içe alma, **Media Encoder Premium iş akışı** destekler:

1. SCC dosyaları
2. TT SMPTE dosyaları
3. CEA-608/CEA-708 – kullanıcı verilerini (H.264 başlangıç akışları, ATSC/53, SCTE20 SHİ iletiler) gerçekleştirilen veya bulunabilecek veri MXF/GXF dosyaları olarak gerçekleştirilen
4. STL alt yazı dosyaları

Çıkışta, aşağıdaki seçenekler kullanılabilir:

1. CEA-608 CEA-708 kapalı çeviri için
2. CEA-608/CEA-708 kapalı geçirmek aracılığıyla (H.264 başlangıç akışları SHİ iletilerinde gömülü veya MXF dosyaları olarak bulunabilecek verilerde gerçekleştirilen)
3. SCC
4. Zaman aşımına SMPTE metin (kaynaktan CEA-608 SMPTE RP2052 başına; DFXP dosya oluşturma dahil)
5. SRT alt yazı dosyası
6. DVB alt konu başlığını akışları

> [!NOTE]
> Azure Media Services akış ile teslim için tüm yukarıdaki Çıkış biçimleri desteklenir.

## <a name="known-issues"></a>Bilinen sorunlar

Girdi videonuzun içermiyorsa Kapalı Açıklamalı Altyazı, çıktı varlık hala boş bir TTML dosyası içerir. 

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

