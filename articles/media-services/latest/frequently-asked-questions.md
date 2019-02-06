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
ms.date: 02/05/2019
ms.author: juliako
ms.openlocfilehash: be4c08bc31c8811655230ab89b48271f4c2b3164
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55756589"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Azure Media Services v3 sık sorulan sorular

Bu makalede, Azure Media Services (AMS) v3 sık sorulan sorular için yanıtlar sağlanır.

## <a name="v3-apis"></a>V3 API'ler

### <a name="how-do-i-configure-media-reserved-units"></a>Medya ayrılmış birimleri nasıl yapılandırabilirim?

Ses analizi ve Video analizi işleri, Media Services v3 tarafından tetiklenen veya Video Indexer için 10 S3 MRU hesabınızla sağlama önemle tavsiye edilir. 10'dan fazla S3 MRU gerekiyorsa, kullanarak bir destek bileti açın [Azure portalında](https://portal.azure.com/).

Ayrıntılar için bkz [medya CLI ile işlemeyi ölçeklendirme](media-reserved-units-cli-how-to.md).

### <a name="what-is-the-recommended-method-to-process-videos"></a>İşlem videolar için önerilen yöntem nedir?

Http (s) video işaret eden bir URL kullanarak işleri gönderme önerilir. Daha fazla bilgi için [http (s) alma](job-input-from-http-how-to.md). Bunu işlenmeden önce giriş video ile bir varlık oluşturmak için gerekli değildir.

### <a name="how-does-pagination-work"></a>Sayfalandırma nasıl çalışır?

Media Services $top OData desteklemek için kaynakları destekler ancak $top için geçirilen değer daha az 1000'den (örneğin, sayfalandırma sayfa boyutu) olmalıdır.

Bu, ya da öğeleri $top (örneğin, 100 en son öğe) kullanarak küçük bir örnek almak veya sayfalandırma kullanarak tüm öğeleri ancak sayfa için sağlar. 

Media Services ile bir kullanıcı tarafından belirtilen sayfalama verilerde desteklemiyor sayfa boyutu.

Daha fazla bilgi için [filtreleme, sıralama, sayfalama](entities-overview.md).

### <a name="how-to-retrieve-an-entity-in-media-services-v3"></a>Media Services v3 bir varlıkta almak nasıl?

V3 üzerinde oluşturulan hem yönetim hem de işlemler işlevselliği kullanıma sunma birleştirilmiş bir API yüzeyi temel **Azure Resource Manager**. Dağıtabilirler **Azure Resource Manager**, kaynak adları her zaman benzersizdir. Bu nedenle, kaynağınız için benzersiz tanımlayıcı dizeleri (örneğin, GUID'leri) kullanabilirsiniz.

## <a name="live-streaming"></a>Canlı akış 

###  <a name="how-to-insert-breaksvideos-and-image-slates-during-live-stream"></a>Sonu/videolar ve resim ekleme sırasında canlı akış maskeleme görüntülerini?

Media Services v3 Canlı kodlama henüz video veya resim maskeleme görüntülerini ekleme sırasında canlı akış desteklemez. 

Kullanabileceğiniz bir [Canlı şirket içi Kodlayıcı](recommended-on-premises-live-encoders.md) kaynak video geçmek için. Birçok uygulama, Telestream Wirecast, değiştirici Studio (iOS üzerinde), OBS Studio (ücretsiz bir uygulama) ve çok daha fazlası gibi kaynakları, geçiş olanağı sağlar.

## <a name="media-services-v2-vs-v3"></a>Media Services v2 vs v3 

### <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>V3 kaynakları yönetmek için Azure portalını kullanabilir miyim?

Henüz değil. Desteklenen Sdk'lardan birini kullanabilirsiniz. Öğreticiler ve bu belge kümesindeki örnekler bakın.

### <a name="is-there-an-assetfile-concept-in-v3"></a>V3 sürümünde AssetFile kavramı vardır?

AssetFiles AMS API'den depolama SDK'sı bağımlılık Media Services ayırmak için kaldırıldı. Artık depolama, Media Services değil depolamada ait bilgileri tutar. 

Daha fazla bilgi için [Media Services v3 geçiş](migrate-from-v2-to-v3.md).

### <a name="where-did-client-side-storage-encryption-go"></a>İstemci tarafı depolama şifrelemesi nereye gitti?

Şimdi, (varsayılan olarak açık) bir sunucu tarafı depolama şifrelemesi kullanmak için önerilir. Daha fazla bilgi için [bekleyen veriler için Azure depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Media Services v3 genel bakış](media-services-overview.md)
