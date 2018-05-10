---
title: Azure Media Services v3 sürüm notları | Microsoft Docs
description: İle en son gelişmeler hakkında güncel kalmak için bu makalede, Azure Media Services v3 üzerindeki en son güncelleştirmeleri sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: fc6c5ba6cd97c261dd44eade33bf21e8d1b74bf0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="azure-media-services-v3-preview-release-notes"></a>Azure Media Services v3 (Önizleme) sürüm notları 

İle en son gelişmeler hakkında güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

* En son sürümleri
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı bırakılan işlevsellik
* Değişiklikleri planları

## <a name="may-07-2018"></a>07 May 2018

### <a name="net-sdk"></a>.NET SDK'sı

.Net SDK aşağıdaki özellikler mevcuttur:

1. **Dönüşümleri** ve **işleri** kodlamak veya medya içeriği çözümlemek için. Örnekler için bkz: [akış dosyalarını](stream-files-tutorial-with-api.md) ve [Çözümle](analyze-videos-tutorial-with-api.md).
2. **StreamingLocators** yayımlama ve son kullanıcı cihazlarına içerik akışı
3. **StreamingPolicies** ve **ContentKeyPolicies** anahtar teslim ve içerik koruma (DRM) içerik teslim edilirken yapılandırmak için.
4. **LiveEvents** ve **LiveOutputs** alma ve canlı akış içeriğini arşivleme yapılandırmak için.
5. **Varlıklar** depolamak medya içerik ve Azure depolama alanında yayınlamak için. 
6. **Akış** yapılandırmak ve dinamik paketleme, şifreleme ve canlı ve isteğe bağlı medya içeriği akış ölçeklendirmek için.

### <a name="known-issues"></a>Bilinen sorunlar

Bilinen bir sorun:

Bir HTTPS kaynak içeriği işaret eden URL (JobInputHttp) işlemiyle gönderirken HTTP sunucusu 'HEAD' isteği desteklediğinden emin olun. Aksi takdirde, işin reddedilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Genel Bakış](media-services-overview.md)
