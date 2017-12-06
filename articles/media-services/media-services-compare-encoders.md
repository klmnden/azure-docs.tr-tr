---
title: "İsteğe bağlı medya kodlayıcılar üzerinde Azure karşılaştırması | Microsoft Docs"
description: "Bu konuda kodlama özelliklerini karşılaştırır ** Medya Kodlayıcısı standart ** ve ** Medya Kodlayıcısı Premium iş akışı **."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: a79437c0-4832-423a-bca8-82632b2c47cc
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 72b4a7b746d446e47b52cf34726a50dd52eaba97
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="comparison-of-azure-on-demand-media-encoders"></a>İsteğe bağlı medya kodlayıcılar üzerinde Azure karşılaştırması

Bu konuda kodlama özelliklerini karşılaştırır **Medya Kodlayıcısı standart** ve **Medya Kodlayıcısı Premium iş akışı**.

## <a name="video-and-audio-processing-capabilities"></a>Video ve ses işleme özellikleri

Aşağıdaki tabloda Medya Kodlayıcısı standart (MES) ve Medya Kodlayıcısı Premium iş akışı (MEPW) arasında işlevlerini karşılaştırılmaktadır. 

|Özellik|Media Encoder Standard|Media Encoder Premium İş Akışı|
|---|---|---|
|Kodlama sırasında koşullu mantık Uygula<br/>(giriş HD ise, örneğin, ardından 5.1 ses kodlama)|Hayır|Evet|
|Kapalı açıklamalı alt yazı|Hayır|[Evet](media-services-premium-workflow-encoder-formats.md#closed_captioning)|
|[Dolby® profesyonel yüksek ses düzeltme](http://www.dolby.com/us/en/technologies/dolby-professional-loudness-solutions.pdf)<br/> iletişim kutusu Intelligence™ ile|Hayır|Evet|
|Titreşim, ters telesine|Temel|Yayın Kalitesi|
|Siyah kenarlıklar algılayacak ve <br/>(pillarboxes, letterboxes)|Hayır|Evet|
|Küçük resim oluşturma|[Evet](media-services-dotnet-generate-thumbnail-with-mes.md)|[Evet](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)|
|Kırpma/kırpma ve videoları Dikiş|[Evet](media-services-advanced-encoding-with-mes.md#trim_video)|Evet|
|Ses veya video yer paylaşımları|[Evet](media-services-advanced-encoding-with-mes.md#overlay)|[Evet](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-1--overlay-an-image-on-top-of-the-video)|
|Grafik yer paylaşımları|Görüntü kaynaklardan|Resim ve metin kaynaklardan|
|Birden çok ses dili izler|Sınırlı|[Evet](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-2--multiple-audio-language-encoding)|

## <a id="billing"></a>Her Kodlayıcı tarafından kullanılan faturalama ölçer
| Ortam işlemci adı | Geçerli fiyatlandırma | Notlar |
| --- | --- | --- |
| **Media Encoder Standard** |KODLAYICI |Toplam süreyi dakika cinsinden belirtilen hızda çıktı olarak üretilen tüm medya dosyalarının göre kodlama görevleri ücretlendirilecek [burada][1], KODLAYICI sütununun altında. |
| **Media Encoder Premium İş Akışı** |PREMIUM KODLAYICI |Toplam süreyi dakika cinsinden belirtilen hızda çıktı olarak üretilen tüm medya dosyalarının göre kodlama görevleri ücretlendirilecek [burada][1], PREMIUM KODLAYICI sütununun altında. |

## <a name="input-containerfile-formats"></a>Kapsayıcı/dosya biçimleri giriş
| Kapsayıcı/dosya biçimleri giriş | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| Adobe® Flash® F4V |Evet |Evet |
| MXF/SMPTE 377M |Evet |Evet |
| GXF |Evet |Evet |
| MPEG-2 aktarım akışları |Evet |Evet |
| MPEG-2 Program akışları |Evet |Evet |
| MPEG-4/MP4 |Evet |Evet |
| Windows Media/ASF |Evet |Evet |
| AVI (sıkıştırılmamış 8 bit/10 bit) |Evet |Evet |
| 3GPP/3GPP2 |Evet |Hayır |
| Kesintisiz akış dosyası biçimi (PIFF 1.3) |Evet |Hayır |
| [Microsoft Dijital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) |Evet |Hayır |
| Matroska/WebM |Evet |Hayır |
| QuickTime (.mov) |Evet |Hayır |

## <a name="input-video-codecs"></a>Görüntü codec bileşenleri giriş
| Görüntü codec bileşenleri giriş | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AVC 8 bit/10-en fazla 4 bit: AVCIntra dahil olmak üzere 2:2 |8 bit 4:2:0. ve 4:2:2 |Evet |
| Hırslı DNxHD (içinde MXF) |Evet |Evet |
| DVCPro/DVCProHD (içinde MXF) |Evet |Evet |
| JPEG2000 |Evet |Evet |
| MPEG-2 (422 profili ve yüksek düzey kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitleri dahil) |En fazla 422 profili |Evet |
| MPEG-1 |Evet |Evet |
| Windows Media Video/VC-1 |Evet |Evet |
| Canopus denetim merkezini/HQX |Hayır |Hayır |
| MPEG-4 bölüm 2 |Evet |Hayır |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Evet |Hayır |
| Apple ProRes 422 |Evet |Hayır |
| Apple ProRes 422 LT |Evet |Hayır |
| Apple ProRes 422 denetim merkezini |Evet |Hayır |
| Apple ProRes Proxy |Evet |Hayır |
| Apple ProRes 4444 |Evet |Hayır |
| Apple ProRes 4444 XQ |Evet |Hayır |

## <a name="input-audio-codecs"></a>Giriş ses codec bileşenleri
| Giriş ses codec bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AES (SMPTE 331 M ve 302 M, AES3 2003) |Hayır |Evet |
| Dolby® E |Hayır |Evet |
| Dolby® dijital (AC3) |Hayır |Evet |
| Dolby® dijital artı (E-AC3) |Hayır |Evet |
| AAC (AAC-LC, HE AAC ve AAC-HEv2; kadar 5.1) |Evet |Evet |
| MPEG Katman 2 |Evet |Evet |
| MP3 (MPEG-1 ses Katman 3) |Evet |Evet |
| Windows Media Ses |Evet |Evet |
| WAV/PCM |Evet |Evet |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Evet |Hayır |
| [Geçerli](https://en.wikipedia.org/wiki/Opus_\(audio_format\)) |Evet |Hayır |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Evet |Hayır |

## <a name="output-containerfile-formats"></a>Çıktı kapsayıcı/dosya biçimleri
| Çıktı kapsayıcı/dosya biçimleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
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

## <a name="output-video-codecs"></a>Çıktı görüntü codec bileşenleri
| Çıktı görüntü codec bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AVC (H.264; 8 bit; yüksek profili kadar 5.2; 4 K Ultra HD; düzey AVC içi) |Yalnızca 8 bit 4:2:0 |Evet |
| HEVC (H.265; 8 bit ve 10-bit;)  |Hayır |Evet |
| Hırslı DNxHD (içinde MXF) |Hayır |Evet |
| MPEG-2 (422 profili ve yüksek düzey kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitleri dahil) |Hayır |Evet |
| MPEG-1 |Hayır |Evet |
| Windows Media Video/VC-1 |Hayır |Evet |
| JPEG küçük resim oluşturma |Evet |Evet |
| PNG küçük resim oluşturma |Evet |Evet |
| BMP küçük resim oluşturma |Evet |Hayır |

## <a name="output-audio-codecs"></a>Çıktı ses codec bileşenleri
| Çıktı ses codec bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AES (SMPTE 331 M ve 302 M, AES3 2003) |Hayır |Evet |
| Dolby® dijital (AC3) |Hayır |Evet |
| Dolby® dijital Plus (E-AC3) 7.1 kadar |Hayır |Evet |
| AAC (AAC-LC, HE AAC ve AAC-HEv2; kadar 5.1) |Evet |Evet |
| MPEG Katman 2 |Hayır |Evet |
| MP3 (MPEG-1 ses Katman 3) |Hayır |Evet |
| Windows Media Ses |Hayır |Evet |

>[!NOTE]
>Dolby® dijital için (AC3) kodlamak, çıktı yalnızca bir ISO MP4 dosyasına yazılabilir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler
* [Medya Kodlayıcısı standart hazır özelleştirerek gelişmiş kodlama görevleri gerçekleştirme](media-services-custom-mes-presets-with-dotnet.md)
* [Kotalar ve kısıtlamaları](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
