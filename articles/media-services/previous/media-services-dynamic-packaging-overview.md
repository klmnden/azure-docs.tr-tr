---
title: Azure Media Services dinamik paketlemeye genel bakış | Microsoft Docs
description: Konu sağlar ve dinamik paketleme genel bakış.
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
ms.date: 11/15/2018
ms.author: juliako
ms.openlocfilehash: eccb141101e4d402fcc79fe5dd433f2fc3382e27
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263854"
---
# <a name="dynamic-packaging"></a>Dinamik paketleme

Microsoft Azure Media Services, birçok medya kaynak dosya biçimleri akış biçimlerinde, medya teslim etmek için kullanılabilir ve içerik koruma için istemci teknolojileri çeşitli biçimlendirir (örneğin, iOS, XBOX, Silverlight, Windows 8). Bu istemciler farklı protokollere anlamak, örneğin HTTP canlı akışı (HLS) V4 biçiminde iOS gerektirir ve kesintisiz akış, Silverlight ve Xbox gerektirir. Uyarlamalı bit hızlı (Çoklu bit hızı) bir dizi varsa MP4 veya MPEG DASH, HLS veya kesintisiz akış anlamak istemcilerinin sunmak istediğiniz Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesi (ISO temel medya 14496-12) dosyaları, medya avantajlarından sürecektir Dinamik paketleme Hizmetleri.

Dinamik paketleme ile tek ihtiyacınız olan Uyarlamalı bit hızlı MP4 dosyası ya da Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesi içeren bir varlık oluşturmaktır. Ardından bildirim veya parça isteğindeki talep üzerine akış belirtilen biçime bağlı olarak sunucu, akışı seçtiğiniz protokolde almanızı sağlayacaktır. Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Aşağıdaki diyagramda, geleneksel kodlama ve statik paketleme iş akışı gösterilmektedir.

![Statik kodlama](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Dinamik paketleme iş akışı aşağıdaki diyagramda gösterilmiştir.

![Dinamik kodlama](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="common-scenario"></a>Yaygın bir senaryo
1. (Bir ara dosyayı olarak adlandırılır) bir giriş dosyasını karşıya yükleyin. Örneğin, H.264, MP4 veya WMV (desteklenen biçimler listesi için bkz. [Media Encoder Standard tarafından desteklenen biçimleri](media-services-media-encoder-standard-formats.md).
2. Mezzanine dosyanızı Uyarlamalı bit hızı kümelerine H.264 MP4 kodlayın.
3. İsteğe bağlı Bulucu oluşturarak hızı Uyarlamalı MP4 kümesine içeren varlığı yayımlayın.
4. Erişim ve içerik akışı için akış URL'leri oluşturun.

## <a name="preparing-assets-for-dynamic-streaming"></a>Varlıklar dinamik akış için hazırlama
Varlığınız dinamik akış için hazırlamak için aşağıdaki seçenekleriniz vardır:

- [Ana dosya karşıya yükleme](media-services-dotnet-upload-files.md).
- [H.264 MP4 bit hızı Uyarlamalı kümeleri oluşturmak için medya Kodlayıcı standart Kodlayıcı kullanın](media-services-dotnet-encode-with-media-encoder-standard.md).
- [İçeriğinizi Stream](media-services-deliver-content-overview.md).

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Dinamik paketleme tarafından desteklenen ses codec bileşenleri

İle dinamik paketleme destekler MP4 dosyaları (veya kesintisiz akış dosyaları) ses kodlanmış içeren [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, v2 HE AAC), [Dolby dijital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Gelişmiş AC 3 veya E-AC3) veya [ DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Express, DTS LBR, DTS HD kayıpsız DTS HD).

> [!Note]
> Dinamik paketleme içeren dosyaları desteklemez [Dolby dijital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) ses (eski codec olmadığı).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

