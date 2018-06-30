---
title: Azure Media Services v3 sık sorulan sorular | Microsoft Docs
description: Bu makalede Azure Media Services v3 sık sorulan sorular için yanıtlar verir.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/29/2018
ms.author: juliako
ms.openlocfilehash: 098a34aba8e5ce23f64d4bb07e3b9622aa2adb8e
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110429"
---
# <a name="azure-media-services-v3-preview-frequently-asked-questions"></a>Azure Media Services v3 (Önizleme) sık sorulan sorular

Bu makalede Azure Media Services (AMS) v3 sık sorulan sorular için yanıtlar verir.

## <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>V3 kaynakları yönetmek için Azure portalını kullanabilir miyim?

Henüz değil. Desteklenen Sdk'lardan birini kullanabilirsiniz. Öğreticiler ve bu belge kümesi örneklerinde bakın.

## <a name="is-there-an-api-for-configuring-media-reserved-units"></a>Medya ayrılmış birimleri yapılandırmak için bir API var?

Media Services ekip RUs v3 ortadan kaldırır. Ancak gerekli hizmet iş tamamlanmadı. O zamana kadar müşteriler RUs ayarlamak için Azure portal veya AMS v2 API'leri kullanmak zorunda (açıklandığı gibi [medyayı işleme ölçeklendirme](../previous/media-services-scale-media-processing-overview.md). 

Kullanırken **VideoAnalyzerPreset** ve/veya **AudioAnalyzerPreset**, Media Services hesabınız için 10 S3 medya ayrılmış birimleri ayarlayın.

## <a name="does-v3-asset-have-no-assetfile-concept"></a>V3 varlık AssetFile kavram var mı?

AssetFiles Media Services depolama SDK'sı bağımlılık ayırmak için AMS API öğesinden kaldırıldı. Şimdi depolama, Media Services olmayan depolama alanına ait bilgileri tutar. 

## <a name="where-did-client-side-storage-encryption-go"></a>İstemci-tarafı depolama şifrelemesi nerede?

Şimdi (varsayılan olarak etkindir) sunucu tarafı depolama şifrelemesi öneririz. Daha fazla bilgi için bkz: [bekleyen veri için Azure depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="what-is-the-recommended-upload-method"></a>Önerilen karşıya yükleme yöntemi nedir?

Http (s) kullanımını alır öneririz. Daha fazla bilgi için bkz: [http (s) alma](job-input-from-http-how-to.md).

## <a name="how-does-pagination-work"></a>Sayfa numaralandırma nasıl çalışır?

Media Services $top OData desteklemek için kaynaklar destekler ancak $top için geçirilen değer değerinden 1000 (örneğin, sayfa numaralandırma sayfa boyutu) olmalıdır.

Bu, küçük bir örnek öğelerinin $top (örneğin, 100 en son kullanılan öğeler) kullanarak ya da almak veya ancak sayfalandırma kullanarak tüm öğeleri sayfa için sağlar. 

Media Services verilerde disk belleği olan bir kullanıcı tarafından belirtilen desteklemediği sayfa boyutu.

Daha fazla bilgi için bkz: [filtreleme, sıralama, disk belleği](assets-concept.md#filtering-ordering-paging)

## <a name="how-to-retrieve-an-entity-in-media-services-v3"></a>Media Services v3 varlık alma

V3 üzerinde oluşturulan yönetimi ve işlemleri işlevselliği kullanıma sunan bir birleşik API yüzeyi temel **Azure Resource Manager**. İle uyumlu olarak **Azure Resource Manager**, kaynak adları her zaman benzersizdir. Bu nedenle kaynaklarınızda benzersiz tanıtıcı dizeleri (GUID gibi) kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Media Services v3 genel bakış](media-services-overview.md)
