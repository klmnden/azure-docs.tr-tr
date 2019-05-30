---
title: Standart Kodlayıcı biçimleri ve codec bileşenleri - Azure
description: Bu konu, standart Kodlayıcı biçimleri ve codec bileşenleri genel bir bakış sağlar.
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
ms.date: 02/10/2019
ms.author: juliako;anilmur
ms.openlocfilehash: 730ff68e70999307417eea276761d56f4a44046a
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65520023"
---
# <a name="standard-encoder-formats-and-codecs"></a>Standart Kodlayıcı biçimleri ve codec bileşenleri

Bu makalede en yaygın içeri aktarma ve ile kullanabileceğiniz dışarı aktarma dosya biçimlerinin listesini içerir [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset). Kullanarak özel önayarların kullanılmasına oluşturma hakkında daha fazla bilgi için **StandardEncoderPreset**, bkz: [dönüşüm ile özel önayarın oluşturma](customize-encoder-presets-how-to.md).

## <a name="input-containerfile-formats"></a>Giriş kapsayıcısı/dosya biçimleri

| Dosya biçimleri (dosya uzantıları) | Desteklenen |
| --- | --- |
| FLV (H.264 ve AAC codec) (.flv) |Evet |
| MXF    (.mxf) |Evet |
| GXF    (.gxf) |Evet |
| MPEG2-PS, MPEG2-TS 3GP (.ts, .ps, .3gp, .3gpp, .mpg) |Evet |
| Windows Media Video (WMV) / ASF (.wmv, .asf) |Evet |
| AVI (sıkıştırılmamış 8 bit/10 bit) (.avi) |Evet |
| MP4 (.mp4, .m4a, .m4v)/ISMV (.isma, .ismv) |Evet |
| [Microsoft Dijital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr ms) |Evet |
| Matroska/WebM (.mkv) |Evet |
| WAVE/WAV (.wav) |Evet |
| QuickTime (.mov) |Evet |

> [!NOTE]
> 
> 

### <a name="audio-formats-in-input-containers"></a>Giriş-kapsayıcılarında ses biçimleri

Standart Kodlayıcı, giriş kapsayıcılarında aşağıdaki ses biçimlerinin taşınmasını destekler:

* Araya eklemeli stereo veya 5.1 örnekler içeren ses parçaları sahip MXF, GXF ve QuickTime dosyaları

veya

* Burada sesin ayrı PCM parçaları yürütülür, ancak kanal eşlemesinin (stero'ya veya 5. 1) dosya meta verilerinden çıkarılabildiği MXF, GXF ve QuickTime dosyaları

## <a name="input-video-codecs"></a>Giriş video codec bileşenleri
| Giriş video codec bileşenleri | Desteklenen |
| --- | --- |
| AVC 8 bit/10-en fazla 4 bit: 2:2, avcıntra |8 bit 4:2:0. ve 4:2:2 |
| Avid DNxHD (mxf biçiminde) |Evet |
| DVCPro/DVCProHD (in MXF) |Evet |
| Dijital video (DV) (AVI dosyalarında) |Evet |
| JPEG 2000 |Evet |
| MPEG-2 (422 profili ve yüksek düzeye kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitler dahil) |422 profiline kadar |
| MPEG-1 |Evet |
| VC-1/WMV9 |Evet |
| Canopus HQ/HQX |Hayır |
| MPEG-4 bölüm 2 |Evet |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Evet |
| YUV420 sıkıştırılmamış veya mezzanine |Evet |
| Apple ProRes 422 |Evet |
| Apple ProRes 422 LT |Evet |
| Apple ProRes 422 HQ |Evet |
| Apple ProRes Proxy |Evet |
| Apple ProRes 4444 |Evet |
| Apple ProRes 4444 XQ |Evet |
| HEVC/H.265| Ana profili|

## <a name="input-audio-codecs"></a>Giriş ses codec bileşenleri
| Giriş ses codec bileşenleri | Desteklenen |
| --- | --- |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; en fazla 5.1) |Evet |
| MPEG Katman 2 |Evet |
| MP3 (MPEG-1 Ses Katmanı 3) |Evet |
| Windows Media Ses |Evet |
| WAV/PCM |Evet |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Evet |
| [Geçerli](https://go.microsoft.com/fwlink/?LinkId=822667) |Evet |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Evet |
| AMR (Uyarlamalı çok ücret) |Evet |
| AES (SMPTE 331 M ve 302 M, AES3-2003) |Hayır |
| Dolby® E |Hayır |
| Dolby® Digital (AC3) |Hayır |
| Dolby® dijital artı (E-AC3) |Hayır |

## <a name="output-formats-and-codecs"></a>Çıkış biçimleri ve codec bileşenleri
Aşağıdaki tabloda, dışarı aktarma için desteklenen codec bileşenleri ve dosya biçimlerini listelenmektedir.

| Dosya Biçimi | Görüntü codec bileşeni | Ses kodek bileşeni |
| --- | --- | --- |
| MP4 <br/><br/>(Çoklu bit hızına sahip MP4 kapsayıcılar dahil) |H.264 (yüksek, ana ve temel profil) |AAC-LC, HE-AAC v1, HE-AAC v2 |
| MPEG2-TS |H.264 (yüksek, ana ve temel profil) |AAC-LC, HE-AAC v1, HE-AAC v2 |

## <a name="next-steps"></a>Sonraki adımlar

[Dönüşüm özel önayarın ile oluşturma](customize-encoder-presets-how-to.md)
