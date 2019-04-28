---
title: Azure üzerinde isteğe bağlı medya kodlayıcılarına karşılaştırması | Microsoft Docs
description: Bu konuda kodlama yeteneklerini karşılaştırır **Media Encoder Standard** ve **Media Encoder Premium iş akışı**.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: a79437c0-4832-423a-bca8-82632b2c47cc
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako;anilmur
ms.openlocfilehash: bb827b80f79a53f30074b9230efe3e2049471051
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61465720"
---
# <a name="comparison-of-azure-on-demand-media-encoders"></a>Azure üzerinde isteğe bağlı medya kodlayıcılarına karşılaştırması  

Bu konuda kodlama yeteneklerini karşılaştırır **Media Encoder Standard** ve **Media Encoder Premium iş akışı**.

## <a name="video-and-audio-processing-capabilities"></a>Video ve ses işleme özellikleri

Aşağıdaki tabloda, Media Encoder Premium iş akışı (MEPW) ile Medya Kodlayıcısı standart (MES) arasındaki işlevlerini karşılaştırılmaktadır. 

|Özellik|Media Encoder Standard|Media Encoder Premium İş Akışı|
|---|---|---|
|Kodlama sırasında koşullu mantığı uygulama<br/>(giriş HD ise, örneğin, ardından 5.1 ses kodlama)|Hayır|Evet|
|Kapalı Açıklamalı Altyazı|Hayır|[Evet](media-services-premium-workflow-encoder-formats.md#closed_captioning)|
|[Dolby® profesyonel ses yüksekliği düzeltme](https://www.dolby.com/us/en/technologies/dolby-professional-loudness-solutions.pdf)<br/> iletişim kutusu Intelligence™ ile|Hayır|Evet|
|Titreşim, ters telesine|Temel|Yayın kalitesinde|
|Algılama ve siyah kenarlıklar kaldırma <br/>(pillarboxes letterboxes)|Hayır|Evet|
|Küçük resim oluşturma|[Evet](media-services-dotnet-generate-thumbnail-with-mes.md)|[Evet](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)|
|Kırpma/kırpma ve videolar birleştirme|[Evet](media-services-advanced-encoding-with-mes.md#trim_video)|Evet|
|Ses veya görüntü yer paylaşımları|[Evet](media-services-advanced-encoding-with-mes.md#overlay)|[Evet](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-1--overlay-an-image-on-top-of-the-video)|
|Grafik yer paylaşımları|Görüntü kaynaklardan|Resim ve metin kaynaklardan|
|Çoklu ses dili izler|Sınırlı|[Evet](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-2--multiple-audio-language-encoding)|

## <a id="billing"></a>Her bir kodlayıcı tarafından kullanılan faturalandırma ölçümü
| Medya işlemci adı | Geçerli fiyatlandırma | Notlar |
| --- | --- | --- |
| **Media Encoder Standard** |KODLAYICI |Belirtilen oranlar, çıktı olarak üretilen tüm medya dosyalarını gösteren dakika cinsinden toplam süre temel kodlama görevleri ücretlendirilir [burada][1], KODLAYICI sütununun altında. |
| **Media Encoder Premium İş Akışı** |PREMIUM KODLAYICI |Belirtilen oranlar, çıktı olarak üretilen tüm medya dosyalarını gösteren dakika cinsinden toplam süre temel kodlama görevleri ücretlendirilir [burada][1], PREMIUM KODLAYICI sütununun altında. |

## <a name="input-containerfile-formats"></a>Giriş kapsayıcısı/dosya biçimleri
| Giriş kapsayıcısı/dosya biçimleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| Adobe® Flash® F4V |Evet |Evet |
| MXF/SMPTE 377M |Evet |Evet |
| GXF |Evet |Evet |
| MPEG-2 aktarım akışı |Evet |Evet |
| MPEG-2 Program akışları |Evet |Evet |
| MPEG-4/MP4 |Evet |Evet |
| Windows Media/ASF |Evet |Evet |
| AVI (sıkıştırılmamış 8 bit/10 bit) |Evet |Evet |
| 3GPP/3GPP2 |Evet |Hayır |
| Kesintisiz akış dosyası biçimi (PIFF 1.3) |Evet |Hayır |
| [Microsoft Dijital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) |Evet |Hayır |
| Matroska/WebM |Evet |Hayır |
| QuickTime (.mov) |Evet |Hayır |

## <a name="input-video-codecs"></a>Giriş video codec bileşenleri
| Giriş video codec bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AVC 8 bit/10-en fazla 4 bit: 2:2, avcıntra |8 bit 4:2:0. ve 4:2:2 |Evet |
| Avid DNxHD (mxf biçiminde) |Evet |Evet |
| DVCPro/DVCProHD (in MXF) |Evet |Evet |
| JPEG2000 |Evet |Evet |
| MPEG-2 (422 profili ve yüksek düzeye kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitler dahil) |422 profiline kadar |Evet |
| MPEG-1 |Evet |Evet |
| Windows Media Video/VC-1 |Evet |Evet |
| Canopus HQ/HQX |Hayır |Hayır |
| MPEG-4 bölüm 2 |Evet |Hayır |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Evet |Hayır |
| Apple ProRes 422 |Evet |Hayır |
| Apple ProRes 422 LT |Evet |Hayır |
| Apple ProRes 422 HQ |Evet |Hayır |
| Apple ProRes Proxy |Evet |Hayır |
| Apple ProRes 4444 |Evet |Hayır |
| Apple ProRes 4444 XQ |Evet |Hayır |
| HEVC/H.265|Ana profili|Ana ve ana 10 profili|

## <a name="input-audio-codecs"></a>Giriş ses codec bileşenleri
| Giriş ses codec bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AES (SMPTE 331 M ve 302 M, AES3-2003) |Hayır |Evet |
| Dolby® E |Hayır |Evet |
| Dolby® Digital (AC3) |Hayır |Evet |
| Dolby® dijital artı (E-AC3) |Hayır |Evet |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; en fazla 5.1) |Evet |Evet |
| MPEG Katman 2 |Evet |Evet |
| MP3 (MPEG-1 Ses Katmanı 3) |Evet |Evet |
| Windows Media Ses |Evet |Evet |
| WAV/PCM |Evet |Evet |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Evet |Hayır |
| [Geçerli](https://en.wikipedia.org/wiki/Opus_\(audio_format\)) |Evet |Hayır |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Evet |Hayır |

## <a name="output-containerfile-formats"></a>Çıkış kapsayıcısı/dosya biçimleri
| Çıkış kapsayıcısı/dosya biçimleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| Adobe® Flash® F4V |Hayır |Evet |
| MXF (OP1a, XDCAM ve AS02) |Hayır |Evet |
| DPP (AS11 dahil) |Hayır |Evet |
| GXF |Hayır |Evet |
| MPEG-4/MP4 |Evet |Evet |
| MPEG-TS |Evet |Evet |
| Windows Media/ASF |Hayır |Evet |
| AVI (sıkıştırılmamış 8 bit/10 bit) |Hayır |Evet |
| Kesintisiz akış dosyası biçimi (PIFF 1.3) |Hayır |Evet |

## <a name="output-video-codecs"></a>Çıkış video codec bileşenleri
| Çıkış Video codec bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AVC (H.264; 8-bit; yüksek profiline kadar 5.2; 4 K Ultra HD; düzeyi AVC içi) |Yalnızca 8 bit 4:2:0 |Evet |
| HEVC (H.265; 8-bit ve 10-bit;)  |Hayır |Evet |
| Avid DNxHD (mxf biçiminde) |Hayır |Evet |
| MPEG-2 (422 profili ve yüksek düzeye kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitler dahil) |Hayır |Evet |
| MPEG-1 |Hayır |Evet |
| Windows Media Video/VC-1 |Hayır |Evet |
| JPEG küçük resim oluşturma |Evet |Evet |
| PNG küçük resim oluşturma |Evet |Evet |
| BMP küçük resim oluşturma |Evet |Hayır |

## <a name="output-audio-codecs"></a>Çıkış ses codec bileşenleri
| Çıkış ses codec bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AES (SMPTE 331 M ve 302 M, AES3-2003) |Hayır |Evet |
| Dolby® Digital (AC3) |Hayır |Evet |
| Dolby® dijital artı (E-AC3) kadar 7.1 |Hayır |Evet |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; en fazla 5.1) |Evet |Evet |
| MPEG Katman 2 |Hayır |Evet |
| MP3 (MPEG-1 Ses Katmanı 3) |Hayır |Evet |
| Windows Media Ses |Hayır |Evet |

>[!NOTE]
>Dolby® dijital için (AC3) kodlama, çıkış yalnızca bir ISO MP4 dosyasına yazılabilir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler
* [Gelişmiş kodlama görevleri, Media Encoder Standard hazır ayarlarını özelleştirerek gerçekleştirin](media-services-custom-mes-presets-with-dotnet.md)
* [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
