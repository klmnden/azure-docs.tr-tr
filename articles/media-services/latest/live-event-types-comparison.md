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
ms.openlocfilehash: 9952a7bbac1eb79de0d3425f839e3bd30196844e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60322293"
---
# <a name="live-event-types-comparison"></a>Canlı olay türlerini karşılaştırma

Azure Media Services, bir [canlı olay](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: live encoding ve doğrudan. 

## <a name="types-comparison"></a>Tür karşılaştırması 

Aşağıdaki tabloda, iki canlı olay türlerinin özellikleri karşılaştırılır.

| Özellik | Doğrudan Canlı etkinlik | Standart Canlı etkinlik |
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
| Çıkış video en yüksek ekran çözünürlüğü|Aynı giriş|720p (30'da kare/saniye)|
| Giriş protokolleri|RTMP, parçalanmış-MP4 (kesintisiz akış)|RTMP, parçalanmış-MP4 (kesintisiz akış)|
| Fiyat|Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesine tıklayın|Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesine tıklayın|
| Maksimum Çalıştırma süresi| 24 saat x 365 gün Canlı doğrusal | En fazla 24 saat|
| Veri özelliği aracılığıyla katıştırılmış CEA 608/708 geçirmek için açıklamalı alt yazılar|Evet|Evet|
| Maskeleme görüntülerini ekleme desteği|Hayır|Hayır|
| API aracılığıyla sinyal ad desteği| Hayır|Hayır|
| Sinyal SCTE-35 bant içi iletiler ad desteği|Evet|Evet|
| Akış katkısı kısa takılması kurtarma olanağı|Evet|Hayır (canlı olay slating giriş verileri olmadan 6 + saniye sonra başlayacak)|
| Tekdüzen olmayan giriş GOPs desteği|Evet|Hayır-giriş GOP süresi sabit olmalıdır|
| Değişken kare hızı girişi için destek|Evet|Yok – giriş kare hızı düzeltilmesi gerekir. Küçük farklılıklar, örneğin, yüksek bir hareket sahneler sırasında izin verilir. Ancak, katkı akış kare hızını (örneğin, 15 çerçeveler/sn) bırakılamıyor.|
| Otomatik akışı kapatmaya Canlı zaman giriş olayının kaybolur|Hayır|12 çalışan hiçbir LiveOutput ise saat sonra|

## <a name="system-presets"></a>Sistem önayarlarını

Gerçek zamanlı kodlama (**Standart** Canlı Etkinlik ile) kullanıldığında gelen akışın birden fazla bit hızı veya katman halinde kodlanma biçimi, kodlama ön ayarı tarafından belirlenir. Şu anda yalnızca izin verilen değer için hazır *Default720p* (varsayılan).

**Default720p** video aşağıdaki 6 katmanlara kodlar.

### <a name="output-video-stream"></a>Çıkış Video Stream

| Bit hızı | Genişlik | Yükseklik | MaxFPS | Profil | Çıkış Stream adı |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Yüksek |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Yüksek |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Yüksek |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Yüksek |Video_512x288_850kbps |
| 550 |384 |216 |30 |Yüksek |Video_384x216_550kbps |
| 200 |340 |192 |30 |Yüksek |Video_340x192_200kbps |

> [!NOTE]
> Özel gerçek zamanlı kodlama ön ayarı kullanmanız gerekiyorsa lütfen amshelp@microsoft.com üzerinden bizimle iletişime geçin. İstediğiniz çözünürlüklerin ve bit hızlarının yer aldığı tabloyu paylaşmanız gerekir. 720p çözünürlükte yalnızca bir katman ve toplamda en fazla 6 katman bulunduğundan emin olun.

### <a name="output-audio-stream"></a>Ses çıkış Stream

128 Kb/sn, 48 kHz örnekleme hızı anda stereo AAC-LC ses kodlanır.

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış genel bakış](live-streaming-overview.md)
