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
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: dccbc6e57e970ec7089f81fccb33b741b9c00e74
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376729"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Azure Media Services v3 sık sorulan sorular

Bu makalede, Azure Media Services (AMS) v3 sık sorulan sorular için yanıtlar sağlanır.

## <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>V3 kaynakları yönetmek için Azure portalını kullanabilir miyim?

Henüz değil. Desteklenen Sdk'lardan birini kullanabilirsiniz. Öğreticiler ve bu belge kümesindeki örnekler bakın.

## <a name="is-there-an-api-for-configuring-media-reserved-units"></a>Medya ayrılmış birimi yapılandırmak için bir API mı?

Medya Hizmetleri ekibi RU v3 sürümünde ortadan kaldırır. Ancak gerekli hizmet iş tamamlanmadı. O zamana kadar RU ayarlamak için Azure portalı veya AMS v2 API'leri kullanmayı imajlarını (açıklandığı [medya işlemeyi ölçeklendirme](../previous/media-services-scale-media-processing-overview.md). 

Kullanırken **VideoAnalyzerPreset** ve/veya **AudioAnalyzerPreset**, Media Services hesabınızı ayarlamak için 10 S3 medya ayrılmış birimi.

## <a name="does-v3-asset-have-no-assetfile-concept"></a>V3 varlık AssetFile konsepti var mı?

AssetFiles AMS API'den depolama SDK'sı bağımlılık Media Services ayırmak için kaldırıldı. Artık depolama, Media Services değil depolamada ait bilgileri tutar. 

## <a name="where-did-client-side-storage-encryption-go"></a>İstemci tarafı depolama şifrelemesi nereye gitti?

(Bu varsayılan olarak etkindir), sunucu tarafı depolama şifrelemesi artık öneririz. Daha fazla bilgi için [bekleyen veriler için Azure depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="what-is-the-recommended-upload-method"></a>Önerilen yükleme yöntemi nedir?

Http (s) kullanımını alır öneririz. Daha fazla bilgi için [http (s) alma](job-input-from-http-how-to.md).

## <a name="how-does-pagination-work"></a>Sayfalandırma nasıl çalışır?

Media Services $top OData desteklemek için kaynakları destekler ancak $top için geçirilen değer daha az 1000'den (örneğin, sayfalandırma sayfa boyutu) olmalıdır.

Bu, ya da öğeleri $top (örneğin, 100 en son öğe) kullanarak küçük bir örnek almak veya sayfalandırma kullanarak tüm öğeleri ancak sayfa için sağlar. 

Media Services ile bir kullanıcı tarafından belirtilen sayfalama verilerde desteklemiyor sayfa boyutu.

Daha fazla bilgi için [filtreleme, sıralama, sayfalama](assets-concept.md#filtering-ordering-paging)

## <a name="how-to-retrieve-an-entity-in-media-services-v3"></a>Media Services v3 bir varlıkta almak nasıl?

V3 üzerinde oluşturulan hem yönetim hem de işlemler işlevselliği kullanıma sunma birleştirilmiş bir API yüzeyi temel **Azure Resource Manager**. Dağıtabilirler **Azure Resource Manager**, kaynak adları her zaman benzersizdir. Bu nedenle kaynaklarınızda benzersiz tanıtıcı dizeleri (GUID gibi) kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Media Services v3 genel bakış](media-services-overview.md)
