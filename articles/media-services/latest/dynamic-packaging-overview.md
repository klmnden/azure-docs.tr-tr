---
title: Azure Media Services dinamik paketlemeye genel bakış | Microsoft Docs
description: Konu, Media Services dinamik paketleme genel bir bakış sağlar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2019
ms.author: juliako
ms.openlocfilehash: 02af95de3793f1d56204b17b0a3d91efbb285e55
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2019
ms.locfileid: "56726423"
---
# <a name="dynamic-packaging"></a>Dinamik paketleme

Microsoft Azure Media Services, birçok medya kaynak dosya biçimleri akış biçimlerinde, medya teslim etmek için kullanılabilir ve çeşitli istemci teknolojiler (örneğin, iOS ve XBOX gibi) için içerik koruma biçimlendirir. Bu istemciler farklı protokollere anlamak, kesintisiz akış, bir HTTP canlı akışı (HLS) biçimi ve Xbox gerektiren iOS örneğin gerektirir. (Çoklu bit hızı) bit hızı Uyarlamalı bir kümeniz varsa MP4 (ISO temel medya 14496-12) dosyaları veya HLS, MPEG DASH veya kesintisiz akış anlamak istemcilerinin sunmak istediğiniz Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesini dinamik avantajlarından faydalanabilirsiniz Paketleme. Paketleme video çözümü belirsiz olduğundan, SD/HD/UHD - 4K desteklenir.

[Akış uç noktaları](streaming-endpoint-concept.md) istemci oyuncular medya içeriği teslim etmek için kullanılan Media Services dinamik paketleme hizmetidir. Dinamik paketleme, tüm akış Uç noktalara (standart veya Premium) standarttır bir özelliktir. Hiçbir ek yok. Bu özellik, Media Services v3 ile ilişkili maliyeti. Dinamik paketleme ile tek ihtiyacınız olan bildirim dosyalarında Uyarlamalı bit hızı MP4 dosyaları kümesini içeren bir varlık. Daha sonra bildirimi veya parça isteğindeki belirtilen biçime bağlı olarak, akışın seçtiğiniz protokolde alırsınız. Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Media Services'de, isteğe bağlı veya canlı akış olup olmadığını dinamik paketleme kullanılır.

Dinamik paketleme iş akışı aşağıdaki diyagramda gösterilmiştir.

![Dinamik kodlama](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

## <a name="common-video-on-demand-workflow"></a>Video isteğe bağlı ortak iş akışı

Dinamik paketleme kullanıldığı akış iş akışı ortak bir Media Services verilmiştir.

1. (Bir ara dosyayı olarak adlandırılır) bir giriş dosyasını karşıya yükleyin. Örneğin, H.264, MP4 veya WMV (desteklenen biçimler listesi için bkz. [Media Encoder Standard tarafından desteklenen biçimleri](media-encoder-standard-formats.md).
2. Mezzanine dosyanızı Uyarlamalı bit hızı kümelerine H.264 MP4 kodlayın.
3. Hızı Uyarlamalı MP4 kümesine içeren varlığı yayımlayın.
4. Farklı biçimlerde (HLS, Dash ve kesintisiz akış) hedef URL'leri oluşturun. Akış uç noktası doğru bildirimi ve bu farklı biçimleri için istekleri hizmet ilgileniriz.
 
## <a name="video-codecs-supported-by-dynamic-packaging"></a>Dinamik paketleme tarafından desteklenen video codec bileşenleri

Dinamik paketleme ile kodlanmış bir video içeren MP4 dosyalarını destekler [H.264](https://en.m.wikipedia.org/wiki/H.264/MPEG-4_AVC) (MPEG-4 AVC veya AVC1) [H.265](https://en.m.wikipedia.org/wiki/High_Efficiency_Video_Coding) (HEVC, hev1 veya hvc1).

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Dinamik paketleme tarafından desteklenen ses codec bileşenleri

Dinamik paketleme ile kodlanmış bir ses içeren MP4 dosyalarını destekler [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, v2 HE AAC), [Dolby dijital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Gelişmiş AC 3 veya E-AC3) veya [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Hızlı, DTS LBR, DTS HD kayıpsız DTS HD).

> [!NOTE]
> Dinamik paketleme içeren dosyaları desteklemez [Dolby dijital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) ses (eski codec olmadığı).

## <a name="next-steps"></a>Sonraki adımlar

[Karşıya yükleme, kodlama, video akışı](stream-files-tutorial-with-api.md)

