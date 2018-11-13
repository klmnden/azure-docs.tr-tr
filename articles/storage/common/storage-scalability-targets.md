---
title: Azure Depolama ölçeklenebilirlik ve performans hedefleri
description: Gelen ve giden bant genişliği, standart Azure depolama hesapları için kapasite ve istek hızı dahil olmak üzere ölçeklenebilirlik ve performans hedefleri hakkında bilgi edinin.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 11/08/2018
ms.author: rogarana
ms.component: common
ms.openlocfilehash: 93e09f3ab6780eb9ce7fa29b4554b53d796b6837
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51564962"
---
# <a name="azure-storage-scalability-and-performance-targets-for-standard-storage-accounts"></a>Standart depolama hesapları için Azure depolama ölçeklenebilirlik ve performans hedefleri

Bu makalede ölçeklenebilirlik ve performans hedefleri standart Azure depolama hesapları için ayrıntıları. Burada listelenen ölçeklenebilirlik ve performans hedefleri yüksek kaliteli hedefler, ancak ulaşılabilir. Tüm durumlarda, istek hızı ve bant genişliği, Depolama tarafından gerçekleştirilen hesap üzerinde saklanan nesneleri kullanılan, erişim desenlerini boyutuna bağlıdır ve iş yükü türüne uygulamanızı gerçekleştirir. 

Hizmetinizin performansını gereksinimlerinizi karşılayıp karşılamadığını belirlemek için test etmeyi unutmayın. Mümkünse, ani artışlar trafiğinin oranını önlemek ve bölümler arasında trafiği iyi dağıtılmış olduğundan emin olun.

Uygulamanızın hangi iş yükünüz için bir bölüm işleyebilir, sınırına ulaştığında, Azure depolama hata kodu: 503 (Sunucu meşgul) veya hata kodu 500 (işlem zaman aşımı) yanıtlarını döndürmek başlar. 503 hatalarını oluşan, yeniden denemeler için bir üstel geri alma İlkesi kullanmak için uygulamanızı değiştirme göz önünde bulundurun. Üstel geri alma yükü azaltmak ve ani trafik bu bölüme kolaylaştırmak için bölüm sağlar.

## <a name="standard-storage-account-scale-limits"></a>Standart depolama hesabı ölçek sınırları
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

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
* [Depolama fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/storage/)
* [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md)
* [Azure depolama çoğaltma](../storage-redundancy.md)
* [Microsoft Azure Depolama Performansı ve Ölçeklenebilirlik Onay Listesi](../storage-performance-checklist.md)

