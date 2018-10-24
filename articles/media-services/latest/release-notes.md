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
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: db68f979239a5783338d99360209ae231a75c936
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945044"
---
# <a name="azure-media-services-v3-release-notes"></a>Azure Media Services v3 sürüm notları 

İle en son gelişmeleri güncel kalmak için bu makalede, ile hakkında bilgi sağlar:

* En son sürümleri
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlev
* Değişiklikleri planları

## <a name="october-2018---ga"></a>Ekim 2018 - GA

Bu bölümde, Azure Media Services (AMS) Ekim güncelleştirmeleri açıklanmaktadır.

### <a name="rest-v3-ga-release"></a>REST v3 GA sürümü

[REST v3 GA sürümü](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01) Canlı için daha fazla API'ları içeren hesabı/varlık düzeyi bildirim filtresi ve DRM desteği.

#### <a name="azure-resource-management"></a>Azure Kaynak Yönetimi 

Azure kaynak yönetimi için destek, birleşik yönetim ve işlemler (artık her şeyi tek bir yerde) API sağlar.

Bu sürüm ile başlayarak, Canlı olaylar oluşturmak için Resource Manager şablonlarını kullanabilirsiniz.

#### <a name="improvement-of-asset-operations"></a>Varlık işlemleri iyileştirme 

Aşağıdaki geliştirmeleri kullanıma sunulan:

- Http (s) URL'leri ya da Azure Blob Depolama SAS URL'lerini alın.
- Varlıklar için kendi kapsayıcı adları belirt 
- Azure işlevleri ile özel iş akışları oluşturmak için daha kolay çıkış desteği.

#### <a name="new-transform-object"></a>Yeni dönüştürme nesnesi

Yeni **dönüştürme** kodlama model nesnesi basitleştirir. Yeni nesne oluşturma ve kodlama Resource Manager şablonları ve hazır paylaşma daha kolay hale getirir. 

#### <a name="azure-active-directory-authentication-and-rbac"></a>Azure Active Directory kimlik doğrulaması ve RBAC

Azure AD kimlik doğrulaması ve rol tabanlı Access Control (RBAC) güvenli dönüşümleri, LiveEvents, içerik anahtarı ilkeleri veya rol veya kullanıcılar tarafından varlıklar Azure AD'de etkinleştirin.

#### <a name="client-sdks"></a>İstemci SDK'ları  

Media Services v3 sürümünde desteklenen diller: .NET Core, Java, Node.js, Ruby, Typescript, Python, Go.

#### <a name="live-encoding-updates"></a>Canlı kodlama güncelleştirmeler

Aşağıdaki Canlı kodlama güncelleştirmeler yapılmıştır:

- Yeni düşük gecikme süresi modu için Canlı (10 saniye için uçtan uca).
- Geliştirilmiş RTMP desteği (daha fazla kararlılık ve daha fazla kaynak Kodlayıcı desteği).
- Güvenli RTMPS alın.

    Bir Livestream oluşturduğunuzda, artık 4 alma URL'leri alma. 4 alma URL'leri neredeyse aynıdır, yalnızca bir bağlantı noktası numarası bölümü farklı aynı akış belirteci (AppID) sahip. URL'lerin ikisinin, birincil ve yedek RTMPS için. 
- 24 saatlik biçim dönüştürme desteği. 
- RTMP SCTE35 aracılığıyla ad sinyal desteği geliştirildi.

#### <a name="improved-event-grid-support"></a>Event Grid için geliştirilmiş destek

İyileştirmeleri desteği aşağıdaki Event Grid görebilirsiniz:

- Logic Apps ve Azure işlevleri ile daha kolay geliştirme için Azure EventGrid tümleştirmesi. 
- Olayları kodlama, Canlı Kanallar ve daha fazlası için abone olun.

### <a name="cmaf-support"></a>CMAF desteği

(İOS 11 +) Apple HLS ve MPEG-DASH için CMAF ve 'cbcs' şifreleme desteği CMAF destekleyen oynatıcılar.

### <a name="video-indexer"></a>Video Indexer

Video Indexer GA sürümü Ağustos'ta Duyuruldu. Yeni şu anda desteklenen özellikler hakkında bilgi için [Video Indexer nedir](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json). 

### <a name="plans-for-changes"></a>Değişiklikleri planları

#### <a name="azure-cli-20"></a>Azure CLI 2.0
 
İşlemler (Live, içerik anahtar ilkeleri, hesabı/varlık filtreleri, akış ilkeleri dahil) tüm özellikler dahildir Azure CLI 2.0 modülü yakında kullanıma sunulacaktır. 

### <a name="known-issues"></a>Bilinen sorunlar

API Önizleme varlık veya AccountFilters için kullanılan müşteriler aşağıdaki sorunundan etkilendiğinizi.

Varlıklar veya hesap filtreleri 09/28 ile 10 arasında oluşturduğunuz, / 12 ile Media Services v3 CLI veya API'leri, gereken tüm varlık ve AccountFilters kaldırın ve bunları sürüm çakışması nedeniyle yeniden oluşturun. 

## <a name="may-2018---preview"></a>Mayıs 2018 - Önizleme

### <a name="net-sdk"></a>.NET SDK'sı

.Net SDK'sı aşağıdaki özellikler mevcuttur:

* **Dönüşümler** ve **işleri** kodlayın veya medya içeriği çözümlemek için. Örnekler için bkz [Stream dosyaları](stream-files-tutorial-with-api.md) ve [Çözümle](analyze-videos-tutorial-with-api.md).
* **StreamingLocators** yayımlama ve son kullanıcı cihazlarına içerik akışı yapmak için
* **StreamingPolicies** ve **ContentKeyPolicies** içerik sunarken anahtar teslim ve content protection (DRM) yapılandırmak için.
* **LiveEvents** ve **LiveOutputs** alma ve canlı akış içeriğini arşivleme yapılandırmak için.
* **Varlıklar** depolamak ve Azure Depolama'da medya içeriği yayımlamak için. 
* **Akış** yapılandırmak ve dinamik paketleme, şifreleme ve canlı ve isteğe bağlı medya içeriği akışı ölçeklendirin.

### <a name="known-issues"></a>Bilinen sorunlar

* Bir iş gönderirken HTTPS URL'leri, SAS URL'lerini veya yolları kullanarak dosyaları Azure Blob Depolama alanında bulunan kaynak videonuzu içe almak için belirtebilirsiniz. AMS v3 şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklememektedir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Genel Bakış](media-services-overview.md)
