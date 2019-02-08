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
ms.openlocfilehash: a447c359c38c2173ea42b6d717067fc8b3a88f9a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875500"
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

Sayfalandırma kullanırken, sonraki bağlantısını toplamasını ve belirli bir sayfa bağımlı olmadan her zaman kullanmalısınız. Ayrıntılar ve örnekler için bkz. [filtreleme, sıralama, sayfalama](entities-overview.md).

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
