---
title: Azure Media Services dinamik paketlemeye genel bakış | Microsoft Docs
description: Dinamik paketleme genel bakış ve konu sağlar.
author: Juliako
manager: cfowler
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 0d9e4f54-5daa-45c1-bfaa-cf09ca89b812
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 8f05015da1f66331413086c0e27c25cd5da75f5c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788392"
---
# <a name="dynamic-packaging"></a>Dinamik paketleme
## <a name="overview"></a>Genel Bakış
Microsoft Azure Media Services, çoğu ortam kaynak dosya biçimleri biçimleri, medya teslim etmek için kullanılabilir ve içerik koruma biçimleri çeşitli istemci teknolojiler (örneğin, iOS, XBOX, Silverlight, Windows 8). Bu istemciler farklı protokollere anlamanıza, örneğin bir HTTP canlı akışı (HLS) V4 biçiminde iOS gerektirir ve Silverlight ve Xbox kesintisiz akış gerektirir. Uyarlamalı bit hızlı (Çoklu bit hızı) kümesi varsa MP4 (ISO temel medya 14496-12) dosyalarını veya MPEG DASH, HLS veya kesintisiz akış anlamak istemcilere sunmak istiyorsanız Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesi, medya avantajlarından almalıdır Dinamik paketleme Hizmetleri.

Dinamik paketleme ile tek ihtiyacınız olan Uyarlamalı bit hızlı MP4 veya uyarlamalı bit hızlı kesintisiz akış dosyaları kümesini içeren bir varlık oluşturmaktır. Ardından, istekteki bildirimini veya parça, isteğe bağlı Akış belirtilen biçime bağlı olarak sunucusu akışı seçtiğiniz protokolde almak güvence altına alır. Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Aşağıdaki diyagramda, geleneksel kodlama ve statik paketleme iş akışı gösterilmektedir.

![Statik kodlama](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Aşağıdaki diyagramda, dinamik paketleme iş akışı gösterilmektedir.

![Dinamik kodlama](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


## <a name="common-scenario"></a>Yaygın bir senaryo
1. (Bir ara dosyayı olarak adlandırılır) bir giriş dosyasını karşıya yükleyin. Örneğin, H.264, MP4 veya WMV (desteklenen biçimler listesi için bkz [biçimleri desteklenen Medya Kodlayıcısı standart tarafından](media-services-media-encoder-standard-formats.md).
2. Mezzanine dosyanızı H.264 MP4 bit hızı Uyarlamalı kümesine kodlayın.
3. Uyarlamalı bit hızı MP4 isteğe bağlı Bulucu oluşturarak kümesine içeren varlığı yayımlayın.
4. Erişim ve içeriğinizin akışını akış URL'lerini oluşturun.

## <a name="preparing-assets-for-dynamic-streaming"></a>Varlıklar dinamik akış için hazırlama
Varlığınızı dinamik akış için hazırlamak için iki seçeneğiniz vardır:

1. [Ana dosya karşıya yükleme](media-services-dotnet-upload-files.md).
2. [Medya Kodlayıcısı standart Kodlayıcı H.264 MP4 bit hızı Uyarlamalı kümeleri oluşturmak için kullanmak](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Kullanıcınıza içeriğinizin akışını](media-services-deliver-content-overview.md).

## <a id="unsupported_formats"></a>Dinamik paketleme tarafından desteklenmeyen biçimleri
Aşağıdaki kaynak dosya biçimlerini dinamik paketleme tarafından desteklenmez.

* Dolby dijital mp4 dosyaları.
* Dolby dijital kesintisiz dosyalar.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

