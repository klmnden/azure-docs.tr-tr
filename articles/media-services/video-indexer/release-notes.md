---
title: Azure Media Services Video Indexer'ın sürüm notları | Microsoft Docs
description: İle en son gelişmeleri güncel kalmak için bu makalede, Azure Media Services Video Indexer'ın en son güncelleştirmeleri sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: na
ms.topic: article
ms.date: 06/25/2019
ms.author: juliako
ms.openlocfilehash: f1c5f43316292f17547b84d970a0f03a1a2c366f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454028"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Azure Media Services Video Indexer'ın sürüm notları

İle en son gelişmeleri güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

* En son sürümleri
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlev

## <a name="june-2019"></a>Haziran 2019

### <a name="video-indexer-deployed-to-japan-east"></a>Video Indexer Japonya Doğu'ya dağıtılır

Artık, bir Video Indexer Ücretli hesap Japonya Doğu bölgesinde de oluşturabilirsiniz.

### <a name="create-and-repair-account-api-preview"></a>Oluşturun ve hesabı API'si (Önizleme) onarın

Eklenen olanak tanıyan yeni bir API [Azure medya hizmeti bağlantı uç noktası veya anahtar güncelleştirme](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Paid-Account-Azure-Media-Services?&groupBy=tag).

### <a name="improve-error-handling-on-upload"></a>Hata işleme karşıya yükleme sırasında geliştirin 

Açıklayıcı bir iletisi, temel alınan Azure Media Services hesabı'nın hatalı yapılandırılması durumunda döndürülür.

### <a name="player-timeline-keyframes-preview"></a>Oynatıcı zaman çizelgesinde ana kareleri Önizleme 

Bu gibi durumlarda, her zaman için resim önizlemesi artık oyuncunun zaman çizelgesinde görebilirsiniz.

### <a name="editor-semi-select"></a>Düzenleyici yarı seçim

Artık belirli Insight zaman çerçevesini düzenleyicide seçme sonucu olarak seçilen tüm ınsights önizlemesini görebilirsiniz.

## <a name="may-2019"></a>Mayıs 2019

### <a name="update-custom-language-model-from-closed-caption-file"></a>Kapalı açıklamalı alt yazı dosyası özel dil modeli güncelleştirme

[Özel dil modeli oluşturma](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?&groupBy=tag) ve [güncelleştirme özel dil modelleri](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Language-Model?&groupBy=tag) API'leri artık VTT, SRT, destek ve TTML dosya biçimleri dil modelleri için giriş olarak.

Çağrılırken [güncelleştirme Video transkripti API](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Transcript?&pattern=transcript), döküm otomatik olarak eklenir. Video ile ilişkili eğitim modeli de otomatik olarak güncelleştirilir. Dil Modellerinizi eğitmek ve özelleştirme konusunda daha fazla bilgi için bkz: [Video Indexer ile bir dil modelini özelleştirin](customize-language-model-overview.md).

### <a name="new-download-transcript-formats--txt-and-csv"></a>Yeni indirme döküm biçimleri – TXT ve CSV

Desteklenen zaten kapalı açıklamalı alt yazı biçimi ek olarak (SRT, VTT ve TTML), Video Indexer, TXT ve CSV biçimlerde transkript yükleniyor artık desteklemektedir.

## <a name="next-steps"></a>Sonraki adımlar

[Genel bakış](video-indexer-overview.md)
