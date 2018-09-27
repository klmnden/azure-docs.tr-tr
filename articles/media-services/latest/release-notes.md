---
title: Azure Media Services v3 sürüm notları | Microsoft Docs
description: İle en son gelişmeleri güncel kalmak için bu makalede, Azure Media Services v3 en yeni güncelleştirmeleri sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/25/2018
ms.author: juliako
ms.openlocfilehash: ed2550c1df4645933fb968c54ee536995c810136
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219338"
---
# <a name="azure-media-services-v3-preview-release-notes"></a>Azure Media Services v3 (Önizleme) sürüm notları 

İle en son gelişmeleri güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

* En son sürümleri
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlev
* Değişiklikleri planları

## <a name="may-07-2018"></a>07 Mayıs 2018

### <a name="net-sdk"></a>.NET SDK'sı

.Net SDK'sı aşağıdaki özellikler mevcuttur:

1. **Dönüşümler** ve **işleri** kodlayın veya medya içeriği çözümlemek için. Örnekler için bkz [Stream dosyaları](stream-files-tutorial-with-api.md) ve [Çözümle](analyze-videos-tutorial-with-api.md).
2. **StreamingLocators** yayımlama ve son kullanıcı cihazlarına içerik akışı yapmak için
3. **StreamingPolicies** ve **ContentKeyPolicies** içerik sunarken anahtar teslim ve content protection (DRM) yapılandırmak için.
4. **LiveEvents** ve **LiveOutputs** alma ve canlı akış içeriğini arşivleme yapılandırmak için.
5. **Varlıklar** depolamak ve Azure Depolama'da medya içeriği yayımlamak için. 
6. **Akış** yapılandırmak ve dinamik paketleme, şifreleme ve canlı ve isteğe bağlı medya içeriği akışı ölçeklendirin.

### <a name="known-issues"></a>Bilinen sorunlar

* Bir iş gönderirken HTTPS URL'leri, SAS URL'lerini veya yolları kullanarak dosyaları Azure Blob Depolama alanında bulunan kaynak videonuzu içe almak için belirtebilirsiniz. Şu anda AMS v3 HTTPS URL'leri öbekli aktarım kodlamasını desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Genel Bakış](media-services-overview.md)
