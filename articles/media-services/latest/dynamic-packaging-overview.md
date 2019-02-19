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
ms.date: 02/17/2019
ms.author: juliako
ms.openlocfilehash: 21b2d9bdef5e6974126c78b1005bd4786182c881
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2019
ms.locfileid: "56405737"
---
# <a name="dynamic-packaging"></a>Dinamik paketleme

Microsoft Azure Media Services, birçok medya kaynak dosya biçimleri akış biçimlerinde, medya teslim etmek için kullanılabilir ve içerik koruma için istemci teknolojileri çeşitli biçimlendirir (örneğin, iOS, XBOX, Silverlight, Windows 8). Bu istemciler farklı protokollere anlamak, örneğin HTTP canlı akışı (HLS) V4 biçiminde iOS gerektirir ve kesintisiz akış, Silverlight ve Xbox gerektirir. Uyarlamalı bit hızlı (Çoklu bit hızı) bir dizi varsa MP4 veya MPEG DASH, HLS veya kesintisiz akış anlamak istemcilerinin sunmak istediğiniz Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesi (ISO temel medya 14496-12) dosyaları, medya avantajlarından sürecektir Dinamik paketleme Hizmetleri.

Tüm paketleme, dinamik ile ihtiyacınız, bir dizi Uyarlamalı bit hızı MP4 dosyaları veya uyarlamalı bit hızlı kesintisiz akış dosyaları içeren bir varlık oluşturmaktır. Ardından bildirim veya parça isteğindeki talep üzerine akış belirtilen biçime bağlı olarak sunucu, akışı seçtiğiniz protokolde almanızı sağlayacaktır. Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Aşağıdaki diyagramda, geleneksel kodlama ve statik paketleme iş akışı gösterilmektedir.

![Statik kodlama](./media/dynamic-packaging-overview/media-services-static-packaging.png)

Dinamik paketleme iş akışı aşağıdaki diyagramda gösterilmiştir.

![Dinamik kodlama](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="common-scenario"></a>Yaygın bir senaryo

1. (Bir ara dosyayı olarak adlandırılır) bir giriş dosyasını karşıya yükleyin. Örneğin, H.264, MP4 veya WMV (desteklenen biçimler listesi için bkz. [Media Encoder Standard tarafından desteklenen biçimleri](media-encoder-standard-formats.md).
2. Mezzanine dosyanızı Uyarlamalı bit hızı kümelerine H.264 MP4 kodlayın.
3. Hızı Uyarlamalı MP4 kümesine içeren varlığı yayımlayın.
4. Erişim ve içerik akışı için akış URL'leri oluşturun.

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Dinamik paketleme tarafından desteklenen ses codec bileşenleri

İle dinamik paketleme destekler MP4 dosyaları (veya kesintisiz akış dosyaları) ses kodlanmış içeren [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, v2 HE AAC), [Dolby dijital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Gelişmiş AC 3 veya E-AC3) veya [ DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Express, DTS LBR, DTS HD kayıpsız DTS HD).

> [!Note]
> Dinamik paketleme içeren dosyaları desteklemez [Dolby dijital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) ses (eski codec olmadığı).

## <a name="next-steps"></a>Sonraki adımlar

[Karşıya yükleme, kodlama, video akışı](stream-files-tutorial-with-api.md)
