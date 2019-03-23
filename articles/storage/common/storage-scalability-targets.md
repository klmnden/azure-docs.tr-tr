---
title: Azure depolama ölçeklenebilirlik ve performans hedefleri - depolama hesapları
description: Gelen ve giden bant genişliği, Azure depolama hesapları için kapasite ve istek hızı dahil olmak üzere ölçeklenebilirlik ve performans hedefleri hakkında bilgi edinin.
services: storage
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: ca9c4c959d21f26369600129f3897b7624dd84f2
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58371183"
---
# <a name="azure-storage-scalability-and-performance-targets-for-storage-accounts"></a>Depolama hesapları için Azure depolama ölçeklenebilirlik ve performans hedefleri

Bu makalede, Azure depolama hesapları için ölçeklenebilirlik ve performans hedefleri ayrıntıları. Burada listelenen ölçeklenebilirlik ve performans hedefleri yüksek kaliteli hedefler, ancak ulaşılabilir. Tüm durumlarda, istek hızı ve bant genişliği, Depolama tarafından gerçekleştirilen hesap üzerinde saklanan nesneleri kullanılan, erişim desenlerini boyutuna bağlıdır ve iş yükü türüne uygulamanızı gerçekleştirir.

Hizmetinizin performansını gereksinimlerinizi karşılayıp karşılamadığını belirlemek için test etmeyi unutmayın. Mümkünse, ani artışlar trafiğinin oranını önlemek ve bölümler arasında trafiği iyi dağıtılmış olduğundan emin olun.

Uygulamanızın hangi iş yükünüz için bir bölüm işleyebilir, sınırına ulaştığında, Azure depolama hata kodu: 503 (Sunucu meşgul) veya hata kodu 500 (işlem zaman aşımı) yanıtlarını döndürmek başlar. 503 hatalarını oluşan, yeniden denemeler için bir üstel geri alma İlkesi kullanmak için uygulamanızı değiştirme göz önünde bulundurun. Üstel geri alma yükü azaltmak ve ani trafik bu bölüme kolaylaştırmak için bölüm sağlar.

## <a name="standard-performance-storage-account-scale-limits"></a>Standart performans depolama hesabı ölçek sınırları

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

## <a name="premium-performance-storage-account-scale-limits"></a>Premium performans depolama hesabı ölçek sınırları

[!INCLUDE [azure-premium-limits](../../../includes/azure-storage-limits-premium.md)]

## <a name="storage-resource-provider-scale-limits"></a>Depolama kaynak sağlayıcısı ölçek sınırları

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Azure Blob Depolama ölçek hedefleri

[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Azure dosyaları ölçek hedefleri

Azure dosyaları ve Azure dosya eşitleme için ölçek ve performans hedefleri hakkında daha fazla bilgi için bkz. [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Azure dosya eşitleme ölçek hedefleri

Azure dosya eşitleme ile sınırsız kullanım amacı tasarlanmıştır ancak sınırsız kullanım her zaman mümkün değildir. Aşağıdaki tabloda, Microsoft'un test sınırları gösterir ve de hangi hedeflerin sabit limitlerdir gösterir:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Azure kuyruk depolama ölçek hedefleri

[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Azure tablo depolama ölçek hedefleri

[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>Ayrıca Bkz.

- [Depolama fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/storage/)
- [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md)
- [Azure depolama çoğaltma](../storage-redundancy.md)
- [Microsoft Azure Depolama Performansı ve Ölçeklenebilirlik Onay Listesi](../storage-performance-checklist.md)