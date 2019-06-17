---
title: Azure Media Services dinamik paketlemeye genel bakış | Microsoft Docs
description: Konu dinamik paketleme genel bir bakış sağlar.
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
ms.date: 03/21/2019
ms.author: juliako
ms.openlocfilehash: 4b4f2ec779c37f78b371c27df80c354eccb41e7a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64869425"
---
# <a name="dynamic-packaging"></a>Dinamik paketleme

> [!div class="op_single_selector" title1="Media Services, kullanmakta olduğunuz sürümünü seçin:"]
> * [Sürüm 3](../latest/dynamic-packaging-overview.md)
> * [Sürüm 2](media-services-dynamic-packaging-overview.md)

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

Microsoft Azure Media Services, birçok medya kaynak dosya biçimleri akış biçimlerinde, medya teslim etmek için kullanılabilir ve içerik koruma için istemci teknolojileri çeşitli biçimlendirir (örneğin, iOS, XBOX, Silverlight, Windows 8). Bu istemciler farklı protokollere anlamak, örneğin HTTP canlı akışı (HLS) V4 biçiminde iOS gerektirir ve kesintisiz akış, Silverlight ve Xbox gerektirir. Uyarlamalı bit hızlı (Çoklu bit hızı) bir dizi varsa MP4 veya MPEG DASH, HLS veya kesintisiz akış anlamak istemcilerinin sunmak istediğiniz Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesi (ISO temel medya 14496-12) dosyaları, medya avantajlarından sürecektir Dinamik paketleme Hizmetleri.

Tüm paketleme, dinamik ile ihtiyacınız, bir dizi Uyarlamalı bit hızı MP4 dosyaları veya uyarlamalı bit hızlı kesintisiz akış dosyaları içeren bir varlık oluşturmaktır. Ardından bildirim veya parça isteğindeki talep üzerine akış belirtilen biçime bağlı olarak sunucu, akışı seçtiğiniz protokolde almanızı sağlayacaktır. Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

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

Dinamik paketleme ile kodlanmış bir ses içeren MP4 dosyalarını destekler [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, v2 HE AAC), [Dolby dijital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus)(Gelişmiş AC 3 veya E-AC3) Dolby Atmos veya [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Express, DTS LBR, DTS HD kayıpsız DTS HD). Akış Dolby Atmos içeriği yaygın akış biçimi (CSF) ya da ortak medya uygulama biçim (CMAF) ile MPEG-DASH Protokolü parçalanmış MP4 gibi standartları ve aracılığıyla HTTP canlı akışı (HLS) CMAF ile desteklenir.

> [!NOTE]
> Dinamik paketleme içeren dosyaları desteklemez [Dolby dijital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) ses (eski codec olmadığı).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

