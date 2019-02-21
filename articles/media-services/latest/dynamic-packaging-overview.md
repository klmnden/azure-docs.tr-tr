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
ms.date: 02/19/2019
ms.author: juliako
ms.openlocfilehash: d1d07402bca5f01cf63d0b039c085e46bb0f0d62
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447931"
---
# <a name="dynamic-packaging"></a>Dinamik paketleme

Microsoft Azure Media Services, birçok medya kaynak dosya biçimleri akış biçimlerinde, medya teslim etmek için kullanılabilir ve çeşitli istemci teknolojiler (örneğin, iOS ve XBOX gibi) için içerik koruma biçimlendirir. Bu istemciler farklı protokollere anlamak, kesintisiz akış, bir HTTP canlı akışı (HLS) biçimi ve Xbox gerektiren iOS örneğin gerektirir. Uyarlamalı bit hızlı (Çoklu bit hızı) bir dizi varsa MP4 (ISO temel medya 14496-12) dosyaları veya bir dizi, HLS, MPEG DASH veya kesintisiz akış anlamak istemcilerinin sunmak istediğiniz Uyarlamalı bit hızlı kesintisiz akış dosyaları, medya avantajlarından sürecektir Dinamik paketleme Hizmetleri.

Dinamik paketleme ile tek ihtiyacınız olan Uyarlamalı bit hızı MP4 dosyaları kümesini içeren bir varlık oluşturmaktır. Ardından bildirim veya parça isteğindeki talep üzerine akış belirtilen biçime bağlı olarak sunucu, akışı seçtiğiniz protokolde almanızı sağlayacaktır. Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Aşağıdaki diyagramda, geleneksel kodlama ve statik paketleme iş akışı gösterilmektedir.

![Statik kodlama](./media/dynamic-packaging-overview/media-services-static-packaging.png)

Dinamik paketleme iş akışı aşağıdaki diyagramda gösterilmiştir.

![Dinamik kodlama](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="dynamic-packaging-workflow"></a>Dinamik paketleme iş akışı

1. (Bir ara dosyayı olarak adlandırılır) bir giriş dosyasını karşıya yükleyin. Örneğin, H.264, MP4 veya WMV (desteklenen biçimler listesi için bkz. [Media Encoder Standard tarafından desteklenen biçimleri](media-encoder-standard-formats.md).
2. Mezzanine dosyanızı Uyarlamalı bit hızı kümelerine H.264 MP4 kodlayın.
3. Hızı Uyarlamalı MP4 kümesine içeren varlığı yayımlayın.
4. Erişim ve içerik akışı için akış URL'leri oluşturun.

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Dinamik paketleme tarafından desteklenen ses codec bileşenleri

Dinamik paketleme ile kodlanmış bir ses içeren MP4 dosyalarını destekler [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, v2 HE AAC), [Dolby dijital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Gelişmiş AC 3 veya E-AC3) veya [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Hızlı, DTS LBR, DTS HD kayıpsız DTS HD).

> [!NOTE]
> Dinamik paketleme içeren dosyaları desteklemez [Dolby dijital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) ses (eski codec olmadığı).

## <a name="next-steps"></a>Sonraki adımlar

[Karşıya yükleme, kodlama, video akışı](stream-files-tutorial-with-api.md)
