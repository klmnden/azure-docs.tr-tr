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
ms.date: 11/27/2018
ms.author: juliako
ms.openlocfilehash: 7ff48962d01a83e8c9fce380d92fbc196ff96533
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585577"
---
# <a name="liveevent-types-comparison"></a>Livestream türlerini karşılaştırma

Azure Media Services, bir [Livestream](https://docs.microsoft.com/rest/api/media/liveevents) iki türden biri olabilir: live encoding ve doğrudan. 

## <a name="types-comparison"></a>Türlerini karşılaştırma 

Aşağıdaki tabloda, iki Livestream tür özellikleri karşılaştırılır.

| Özellik | Doğrudan Livestream | Standart (Temel) Livestream |
| --- | --- | --- |
| Tekli bit hızı girişi Çoklu bit hızlarında buluta halinde kodlanır |Hayır |Evet |
| Katkı için en yüksek ekran çözünürlüğü akışı |4K (4096 x 2160 en 60 kare/sn) |1080p (1920 x 1088 en 30 kare/sn)|
| Önerilen en yüksek katmanlarında akışı katkı|12 adede kadar|Bir ses|
| En yüksek katmanlarında çıkış| Aynı giriş|En fazla 7|
| Akış katkı en fazla toplam bant genişliği|60 MB/sn|Yok|
| Tek bir katman katkısı için maksimum hızı |20 MB/sn|20 MB/sn|
| Ses parçalarını birden çok dil desteği|Evet|Hayır|
| Desteklenen giriş video codec bileşenleri |H.264/AVC ve H.265/HEVC|H.264/AVC|
| Desteklenen çıkış video codec bileşenleri|Aynı giriş|H.264/AVC|
| Desteklenen video bit derinliği, giriş ve çıkış|10-bit dahil olmak üzere HDR 10/HLG kadar|8-bit|
| Desteklenen giriş ses codec bileşenleri|AAC-LC, HE-AAC v1, HE-AAC v2|AAC-LC, HE-AAC v1, HE-AAC v2|
| Desteklenen çıkış ses codec bileşenleri|Aynı giriş|AAC-LC|
| Giriş protokolleri|RTMP, parçalanmış-MP4 (kesintisiz akış)|RTMP, parçalanmış-MP4 (kesintisiz akış)|
| Fiyat|Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesine tıklayın|Bkz: [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/media-services/) ve "Canlı Video" sekmesine tıklayın|
| Maksimum Çalıştırma süresi|24 x 365 doğrusal Canlı|7/24|
| Veri özelliği aracılığıyla katıştırılmış CEA 608/708 geçirmek için açıklamalı alt yazılar|Evet|Evet|
| Maskeleme görüntülerini ekleme desteği|Hayır|Hayır|
| API aracılığıyla sinyal ad desteği| Hayır|Hayır|
| Sinyal SCTE-35 bant içi iletiler ad desteği|Evet|Evet|
| Akış katkısı kısa takılması kurtarma olanağı|Evet|Hayır (Livestream slating giriş verileri olmadan 6 + saniye sonra başlayacak)|
| Tekdüzen olmayan giriş GOPs desteği|Evet|Hayır-giriş GOP süresi sabit olmalıdır|
| Değişken kare hızı girişi için destek|Evet|Yok – giriş kare hızı düzeltilmesi gerekir. Küçük farklılıklar, örneğin, yüksek bir hareket sahneler sırasında izin verilir. Ancak, katkı akış kare hızını (örneğin, 15 çerçeveler/sn) bırakılamıyor.|
| Otomatik akışı kapatmaya Livestream anahtar kullanıldığında, kaybolur|Hayır|12 çalışan hiçbir LiveOutput ise saat sonra|

## <a name="next-steps"></a>Sonraki adımlar

[Canlı akış genel bakış](live-streaming-overview.md)
