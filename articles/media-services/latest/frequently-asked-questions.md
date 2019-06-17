---
title: Azure Media Services v3 hakkında sık sorulan sorular | Microsoft Docs
description: Bu makalede, Azure Media Services v3 sık sorulan sorular için yanıtlar sağlanır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/24/2019
ms.author: juliako
ms.openlocfilehash: 98e8c0ccd150776341e644f7565696e8fbd63e99
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65556280"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Azure Media Services v3 sık sorulan sorular

Bu makalede, Azure Media Services (AMS) v3 sık sorulan sorular için yanıtlar sağlanır.

## <a name="v3-apis"></a>V3 API'ler

### <a name="what-azure-roles-can-perform-actions-on-azure-media-services-resources"></a>Hangi Azure rolleri, Azure Media Services kaynaklardaki eylemler gerçekleştirebilir miyim? 

Bkz: [Media Services hesapları için rol tabanlı erişim denetimi (RBAC)](rbac-overview.md).

### <a name="how-do-i-configure-media-reserved-units"></a>Medya ayrılmış birimleri nasıl yapılandırabilirim?

Ses analizi ve Video analizi işleri, Media Services v3 tarafından tetiklenen veya Video Indexer için 10 S3 MRU hesabınızla sağlama önemle tavsiye edilir. 10'dan fazla S3 MRU gerekiyorsa, kullanarak bir destek bileti açın [Azure portalında](https://portal.azure.com/).

Ayrıntılar için bkz [medya CLI ile işlemeyi ölçeklendirme](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>İşlem videolar için önerilen yöntem nedir?

Kullanım [dönüştüren](https://docs.microsoft.com/rest/api/media/transforms) kodlama veya videoları analiz için ortak görevler yapılandırmak için. Her **dönüştürme** bir tarif veya bir iş akışı, video veya ses dosyalarını işlemek için görevler açıklanmaktadır. A [iş](https://docs.microsoft.com/rest/api/media/jobs) uygulamak için Media Services için fiili istek **dönüştürme** belirli bir giriş video veya ses içeriği için. Dönüştürme oluşturulduktan sonra Media Services API'leri ve yayımlanan SDK'ları hiçbirini kullanarak işleri gönderebilirsiniz. Daha fazla bilgi için [Dönüşümler ve İşler](transforms-jobs-concept.md) konusuna bakın.

### <a name="how-does-pagination-work"></a>Sayfalandırma nasıl çalışır?

Sayfalandırma kullanırken, sonraki bağlantısını toplamasını ve belirli bir sayfa bağımlı olmadan her zaman kullanmalısınız. Ayrıntılar ve örnekler için bkz. [filtreleme, sıralama, sayfalama](entities-overview.md).

### <a name="what-features-are-not-yet-available-in-azure-media-services-v3"></a>Hangi özellikleri henüz Azure Media Services v3 sürümünde kullanılamıyor?

Ayrıntılar için bkz [özellik v2 API'leri göre boşlukları](migrate-from-v2-to-v3.md#feature-gaps-with-respect-to-v2-apis).

## <a name="live-streaming"></a>Canlı akış 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Sonu/videolar ve resim ekleme sırasında canlı akış maskeleme görüntülerini?

Media Services v3 Canlı kodlama henüz video veya resim maskeleme görüntülerini ekleme sırasında canlı akış desteklemez. 

Kullanabileceğiniz bir [Canlı şirket içi Kodlayıcı](recommended-on-premises-live-encoders.md) kaynak video geçmek için. Birçok uygulama, Telestream Wirecast, değiştirici Studio (iOS üzerinde), OBS Studio (ücretsiz bir uygulama) ve çok daha fazlası gibi kaynakları, geçiş olanağı sağlar.

## <a name="content-protection"></a>İçerik koruma

### <a name="how-and-where-to-get-jwt-token-before-using-it-to-request-license-or-key"></a>Nasıl ve nereye isteği lisans veya anahtar için kullanmadan önce JWT belirteci almak?

1. Üretim için bir güvenli belirteç Hizmetleri (bir HTTPS isteği sonra JWT belirtecini verir STS) (web hizmeti) olması gerekir. Test için gösterilen kod kullanabileceğinizi **GetTokenAsync** içinde tanımlanan yöntem [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs).
2. Oynatıcı bir kullanıcı sts'ye böyle bir belirteç için doğrulandıktan sonra istekte bulunmak ve belirteç değeri olarak atamanız gerekir. Kullanabileceğiniz [Azure Media Player API'sine](https://amp.azure.net/libs/amp/latest/docs/).

* STS, hem simetrik hem de asimetrik anahtar ile çalışan bir örnek için bkz. Lütfen [ https://aka.ms/jwt ](https://aka.ms/jwt). 
* Böyle JWT belirteci kullanarak Azure Media Player temel bir yürütücü örneği için bkz [ https://aka.ms/amtest ](https://aka.ms/amtest) (belirteç giriş görmek için "player_settings" bağlantıyı genişletme).

### <a name="how-do-you-authorize-requests-to-stream-videos-with-aes-encryption"></a>Nasıl video akışı AES şifreleme ile istek yetki veriyor musunuz?

STS (güvenli belirteç hizmeti) yararlanmak için doğru yaklaşımdır bakın:

STS kullanıcı profili bağlı olarak farklı talepler (örneğin, "Premium kullanıcı", "Temel kullanıcı", "Ücretsiz deneme kullanıcı") ekleyin. JWT'nin farklı Taleplerde ile kullanıcı farklı içeriğini görebilir. Elbette, farklı içerik/varlık için karşılık gelen RequiredClaims ContentKeyPolicyRestriction olacaktır.

Yapılandırma, kullanım Azure medya Hizmetleri API'leri lisans/anahtar teslim ve varlıklarınızı şifreleme (gösterildiği gibi [Bu örnek](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES/Program.cs)).

Daha fazla bilgi için bkz.

- [İçerik korumaya genel bakış](content-protection-overview.md)
- [Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md)

## <a name="media-services-v2-vs-v3"></a>Media Services v2 vs v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>V3 kaynakları yönetmek için Azure portalını kullanabilir miyim?

Şu anda, v3 kaynaklarını yönetmek için Azure portalını kullanamıyorsunuz. [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks) birini kullanın.

### <a name="is-there-an-assetfile-concept-in-v3"></a>V3 sürümünde AssetFile kavramı vardır?

AssetFiles AMS API'den depolama SDK'sı bağımlılık Media Services ayırmak için kaldırıldı. Artık depolama, Media Services değil depolamada ait bilgileri tutar. 

Daha fazla bilgi için [Media Services v3 geçiş](migrate-from-v2-to-v3.md).

### <a name="where-did-client-side-storage-encryption-go"></a>İstemci tarafı depolama şifrelemesi nereye gitti?

Şimdi, (varsayılan olarak açık) bir sunucu tarafı depolama şifrelemesi kullanmak için önerilir. Daha fazla bilgi için [bekleyen veriler için Azure depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="next-steps"></a>Sonraki adımlar

[Media Services v3 genel bakış](media-services-overview.md)
