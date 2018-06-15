---
title: Medya Kodlayıcısı Premium iş akışı biçimleri ve codec bileşenleri | Microsoft Docs
description: Bu konuda Medya Kodlayıcısı Premium iş akışı biçimleri biçimleri ve codec bileşenleri hakkında genel bir bakış sağlar
services: media-services
documentationcenter: ''
author: juliako
manager: erik43
editor: ''
ms.assetid: b197fce8-3b9b-4189-8d08-486810c0426f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: eb1cadec240dc7f6e3ac5b8932d66c3d55c76e42
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2018
ms.locfileid: "29150078"
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>Medya Kodlayıcısı Premium iş akışı biçimleri ve codec bileşenleri
> [!NOTE]
> Premium Kodlayıcı sorular için e-posta mepd@microsoft.com.
> 
> Bu konuda tartışılan Medya Kodlayıcısı Premium iş akışı medya işlemcisi Çin'de kullanılamaz. 
> 
> 

Bu belge giriş ve çıkış dosya biçimlerini ve genel Önizleme sürümü tarafından desteklenen codec bileşenleri listesini içeren **Medya Kodlayıcısı Premium iş akışı** Kodlayıcı.

[Medya Kodlayıcısı Premium Worflow giriş biçimlerini ve codec bileşenleri](#input_formats)

[Medya Kodlayıcısı Premium Worflow Çıkış biçimleri ve codec bileşenleri](#output_formats)

**Medya Kodlayıcısı Premium iş akışı** destekler kapalı açıklanan açıklamalı alt yazı [bu](#closed_captioning) bölümü. 

## <a id="input_formats"></a>Medya Kodlayıcısı Premium iş akışı giriş biçimlerini ve codec bileşenleri
Aşağıdaki bölümde bu medya işlemcisi giriş olarak destekleyen codec bileşenleri ve dosya biçimlerini listeler.

### <a name="input-containerfile-formats"></a>Kapsayıcı/dosya biçimleri giriş
* Adobe® Flash® F4V
* MXF/SMPTE 377M
* GXF
* MPEG-2 aktarım akışları
* MPEG-2 Program akışları
* MPEG-4/MP4
* Windows Media/ASF
* AVI (sıkıştırılmamış 8 bit/10 bit)

### <a name="input-video-codecs"></a>Görüntü codec bileşenleri giriş
* AVC 8 bit/10-en fazla 4 bit: AVCIntra dahil olmak üzere 2:2
* Avid DNxHD (in MXF)
* DVCPro/DVCProHD (in MXF)
* JPEG2000
* MPEG-2 (422 profili ve yüksek düzey kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitleri dahil)
* MPEG-1
* Windows Media Video/VC-1

### <a name="input-audio-codecs"></a>Giriş ses codec bileşenleri
* AES (SMPTE 331 M ve 302 M, AES3 2003)
* Dolby® E
* Dolby® Digital (AC3)
* AAC (AAC-LC, HE AAC ve AAC-HEv2; kadar 5.1)
* MPEG Layer 2
* MP3 (MPEG-1 ses Katman 3)
* Windows Media Ses
* WAV/PCM

## <a id="output_format"></a>Medya Kodlayıcısı Premium iş akışı Çıkış biçimleri ve codec bileşenleri
Aşağıdaki bölümde bu medya işlemcisi çıktısı olarak desteklenen codec bileşenleri ve dosya biçimlerini listeler.

### <a name="output-containerfile-formats"></a>Çıktı kapsayıcı/dosya biçimleri
* Adobe® Flash® F4V
* MXF (OP1a, XDCAM ve AS02)
* DPP (including AS11)
* GXF
* MPEG-4/MP4
* Windows Media/ASF
* AVI (sıkıştırılmamış 8 bit/10 bit)
* Kesintisiz akış dosyası biçimi (PIFF 1.3)
* MPEG-TS 

### <a name="output-video-codecs"></a>Çıktı görüntü codec bileşenleri
* AVC (H.264; 8 bit; yüksek profili kadar 5.2; 4 K Ultra HD; düzey AVC içi)
* Avid DNxHD (in MXF)
* DVCPro/DVCProHD (in MXF)
* MPEG-2 (422 profili ve yüksek düzey kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitleri dahil)
* MPEG-1
* Windows Media Video/VC-1
* JPEG küçük resim oluşturma

### <a name="output-audio-codecs"></a>Çıktı ses codec bileşenleri
* AES (SMPTE 331 M ve 302 M, AES3 2003)
* Dolby® Digital (AC3)
* Dolby® dijital Plus (E-AC3) 7.1 kadar
* AAC (AAC-LC, HE AAC ve AAC-HEv2; kadar 5.1)
* MPEG Layer 2
* MP3 (MPEG-1 ses Katman 3)
* Windows Media Ses

>[!NOTE]
>Dolby® dijital için (AC3) kodlamak, çıktı yalnızca bir ISO MP4 dosyasına yazılabilir.

## <a id="closed_captioning"></a>Kapalı açıklamalı alt yazıları için destek
Üzerinde alma **Medya Kodlayıcısı Premium iş akışı** destekler:

1. SCC dosyaları
2. SMPTE TT dosyaları
3. CEA-608/CEA-708 – kullanıcı verilerini (H.264 başlangıç akışları, ATSC/53, SCTE20 SHİ iletilerin) gerçekleştirilen veya bulunabilecek veri MXF/GXF dosyaları olarak gerçekleştirilen
4. STL alt başlık dosyaları

Çıktı, aşağıdaki seçenekler kullanılabilir:

1. CEA 608 CEA 708 çeviri için
2. CEA-608/CEA-708 geçirmek aracılığıyla (H.264 başlangıç akışları SHİ iletilerinde katıştırılmış veya olarak bulunabilecek verileri MXF dosyalarında taşınan)
3. SCC
4. SMPTE zaman aşımına metinden (kaynak CEA 608 SMPTE RP2052 başına; DFXP dosya oluşturma dahil)
5. SRT altyazısı dosyası
6. DVB altyazısı akışlar

Not: Yukarıdaki Çıkış biçimleri tüm Azure Media Services akış aracılığıyla teslim için desteklenir.

## <a name="known-issues"></a>Bilinen sorunlar
Giriş videonuzun içermiyorsa kapalı açıklamalı alt yazı, çıkış varlık hala boş bir TTML dosya içerir. 

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

