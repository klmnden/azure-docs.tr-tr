---
title: Azure Media Services Livestream türleri | Microsoft Docs
description: Bu makale Livestream türünü karşılaştırmak ayrıntılı bir tablo.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/01/2019
ms.author: juliako
ms.openlocfilehash: bd4899374c06246ddd4d5fa81d0f6e3a6a1e7017
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075016"
---
# <a name="live-event-types-comparison"></a>Canlı olay türlerini karşılaştırma

Azure Media Services, bir [canlı olay](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: live encoding ve doğrudan. 

## <a name="types-comparison"></a>Tür karşılaştırması 

Aşağıdaki tablo, canlı olay türlerinin özellikleri karşılaştırılır.

| Özellik | Doğrudan Canlı etkinlik | Standart veya Premium1080p Canlı etkinlik |
| --- | --- | --- |
| Tekli bit hızı girişi Çoklu bit hızlarında buluta halinde kodlanır |Hayır |Evet |
| Katkı için en yüksek ekran çözünürlüğü akışı |4K (4096 x 2160 en 60 kare/sn) |1080p (1920 x 1088 en 30 kare/sn)|
| Önerilen en yüksek katmanlarında akışı katkı|12 adede kadar|Bir ses|
| En yüksek katmanlarında çıkış| Aynı giriş|En fazla 6 (sistem önayarlarını aşağıya bakın)|
| Akış katkı en fazla toplam bant genişliği|60 Mbps|Yok|
| Tek bir katman katkısı için maksimum hızı |20 Mbps|20 Mbps|
| Ses parçalarını birden çok dil desteği|Evet|Hayır|
| Desteklenen giriş video codec bileşenleri |H.264/AVC ve H.265/HEVC|H.264/AVC|
| Desteklenen çıkış video codec bileşenleri|Aynı giriş|H.264/AVC|
| Desteklenen video bit derinliği, giriş ve çıkış|10-bit dahil olmak üzere HDR 10/HLG kadar|8-bit|
| Desteklenen giriş ses codec bileşenleri|AAC-LC, HE-AAC v1, HE-AAC v2|AAC-LC, HE-AAC v1, HE-AAC v2|
| Desteklenen çıkış ses codec bileşenleri|Aynı giriş|AAC-LC|
| Çıkış video en yüksek ekran çözünürlüğü|Aynı giriş|Standart - 720p Premium1080p - 1080p|
| Giriş video en yüksek kare hızı|60 kare/saniye|Standart veya Premium1080p - 30 kare/saniye|
| Giriş protokolleri|RTMP, parçalanmış-MP4 (kesintisiz akış)|RTMP, parçalanmış-MP4 (kesintisiz akış)|
| Fiyat|Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesine tıklayın|Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesine tıklayın|
| Maksimum Çalıştırma süresi| 24 saat x 365 gün Canlı doğrusal | En fazla 24 saat|
| Veri özelliği aracılığıyla katıştırılmış CEA 608/708 geçirmek için açıklamalı alt yazılar|Evet|Evet|
| Maskeleme görüntülerini ekleme desteği|Hayır|Hayır|
| API aracılığıyla sinyal ad desteği| Hayır|Hayır|
| Sinyal SCTE-35 bant içi iletiler ad desteği|Evet|Evet|
| Akış katkısı kısa takılması kurtarma olanağı|Evet|Kısmi|
| Tekdüzen olmayan giriş GOPs desteği|Evet|Hayır-giriş GOP süresi sabit olmalıdır|
| Değişken kare hızı girişi için destek|Evet|Yok – giriş kare hızı düzeltilmesi gerekir. Küçük farklılıklar, örneğin, yüksek bir hareket sahneler sırasında izin verilir. Ancak katkı akış kare hızı bırakılamıyor (örneğin, 15'e kare/saniye).|
| Otomatik akışı kapatmaya Canlı zaman giriş olayının kaybolur|Hayır|12 çalışan hiçbir LiveOutput ise saat sonra|

## <a name="system-presets"></a>Sistem önayarlarını

Çözünürlüklerine ve bit hızlarına dönüştürme gerçek zamanlı Kodlayıcı çıkışı bulunan göre belirlenir [presetName](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencoding). Kullanılıyorsa bir **standart** Canlı Kodlayıcı (LiveEventEncodingType.Standard) sonra *Default720p* hazır aşağıda açıklanan 6 çözümleme/bit oranı çiftleri kümesini belirtir. Aksi takdirde kullanıyorsanız bir **Premium1080p** Canlı Kodlayıcı (LiveEventEncodingType.Premium1080p) sonra *Default1080p* çıkış çözümleme/bit oranı çiftleri kümesini hazır belirtir. 

### <a name="output-video-streams-for-default720p"></a>Video Çıkış akışları Default720p için

**Default720p** giriş videosunun aşağıdaki 6 katmanlara kodlar.

| Bit hızı | Genişlik | Yükseklik | MaxFPS | Profil | Çıkış Stream adı |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Yüksek |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Yüksek |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Yüksek |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Yüksek |Video_512x288_850kbps |
| 550 |384 |216 |30 |Yüksek |Video_384x216_550kbps |
| 200 |340 |192 |30 |Yüksek |Video_340x192_200kbps |

> [!NOTE]
> Canlı kodlama Önayarı özelleştirmek istiyorsanız lütfen Azure portalı üzerinden bir destek bileti açın. İstediğiniz çözünürlüklerin ve bit hızlarının yer aldığı tabloyu paylaşmanız gerekir. 720p çözünürlükte yalnızca bir katman ve toplamda en fazla 6 katman bulunduğundan emin olun. Ayrıca standart bir gerçek zamanlı Kodlayıcı için bir ön ayarı isteyen belirtin.

### <a name="output-video-streams-for-default1080p"></a>Video Çıkış akışları Default1080p için

**Default1080p** giriş videosunun aşağıdaki 6 katmanlara kodlar.

| Bit hızı | Genişlik | Yükseklik | MaxFPS | Profil | Çıkış Stream adı |
| --- | --- | --- | --- | --- | --- |
| 5500 |1920 |1080 |30 |Yüksek |Video_1920x1080_5500kbps |
| 3000 |1280 |720 |30 |Yüksek |Video_1280x720_3000kbps |
| 1600 |960 |540 |30 |Yüksek |Video_960x540_1600kbps |
| 800 |640 |360 |30 |Yüksek |Video_640x360_800kbps |
| 400 |480 |270 |30 |Yüksek |Video_480x270_400kbps |
| 200 |320 |180 |30 |Yüksek |Video_320x180_200kbps |

> [!NOTE]
> Canlı kodlama Önayarı özelleştirmek istiyorsanız lütfen Azure portalı üzerinden bir destek bileti açın. İstediğiniz çözünürlüklerin ve bit hızlarının yer aldığı tabloyu paylaşmanız gerekir. Yalnızca bir katmanında 1080 p ve en fazla 6 katmanları olduğundan emin olun. Ayrıca bir Premium1080p gerçek zamanlı Kodlayıcı için bir ön ayarı isteyen belirtin.

### <a name="output-audio-stream-for-default720p-and-default1080p"></a>Çıkış ses Stream Default720p ve Default1080p

Her ikisi için de *Default720p* ve *Default1080p* hazır ses stereo AAC-48 kHz oranını örnekleme LC için 128 Kb/sn, kodlanmış.

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış genel bakış](live-streaming-overview.md)
