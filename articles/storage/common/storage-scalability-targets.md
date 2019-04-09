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
ms.openlocfilehash: 96322c730300e360ed03f4b623db2a7f18825f55
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59267710"
---
# <a name="azure-storage-scalability-and-performance-targets-for-storage-accounts"></a>Depolama hesapları için Azure depolama ölçeklenebilirlik ve performans hedefleri

Bu makalede, Azure depolama hesapları için ölçeklenebilirlik ve performans hedefleri ayrıntıları. Burada listelenen ölçeklenebilirlik ve performans hedefleri yüksek kaliteli hedefler, ancak ulaşılabilir. Tüm durumlarda, istek hızı ve bant genişliği, Depolama tarafından gerçekleştirilen hesap üzerinde saklanan nesneleri kullanılan, erişim desenlerini boyutuna bağlıdır ve iş yükü türüne uygulamanızı gerçekleştirir.

Hizmetinizin performansını gereksinimlerinizi karşılayıp karşılamadığını belirlemek için test etmeyi unutmayın. Mümkünse, ani artışlar trafiğinin oranını önlemek ve bölümler arasında trafiği iyi dağıtılmış olduğundan emin olun.

Uygulamanızın hangi iş yükünüz için bir bölüm işleyebilir, sınırına ulaştığında, Azure depolama hata kodu: 503 (Sunucu meşgul) veya hata kodu 500 (işlem zaman aşımı) yanıtlarını döndürmek başlar. 503 hatalarını oluşan, yeniden denemeler için bir üstel geri alma İlkesi kullanmak için uygulamanızı değiştirme göz önünde bulundurun. Üstel geri alma yükü azaltmak ve ani trafik bu bölüme kolaylaştırmak için bölüm sağlar.

## <a name="storage-account-scale-limits"></a>Depolama hesabı ölçek sınırları

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

### <a name="premium-files-scale-targets"></a>Premium dosyalar hedefleri ölçeklendirin

Premium dosyalar için dikkate alınması gereken sınırlamalar üç kategoriye ayrılır: depolama hesapları, paylaşımları ve dosyaları.

Örneğin: Tek bir paylaşım 100.000 IOPS değerlerine ulaşabilir ve tek bir dosyayı en fazla 5000 IOPS ölçeklendirme yapabilirsiniz. Bu nedenle, örneğin, üç bir paylaşımında dosyalar varsa, bu paylaşımdan alabilirsiniz IOPS üst sınırı olan 15.000.

#### <a name="premium-file-share-limits"></a>Premium dosya paylaşımı sınırları

> [!IMPORTANT]
> Depolama hesabı sınırları, tüm paylaşımlar için geçerlidir. Kadar ölçeklendirme en yüksek depolama hesapları için yalnızca depolama hesabı başına yalnızca bir paylaşım ise ulaşılabilir eşittir.

|Alan  |Hedef  |
|---------|---------|
|En küçük boyutu                        |100 GiB      |
|Maksimum boyut                        |100 TiB      |
|En küçük boyut artırma/azaltma    |1 giB      |
|Temel IOPS    |100.000 adede kadar GiB başına 1 IOPS|
|Patlaması IOPS    |100.000 adede kadar GiB başına 3 x IOPS|
|En düşük bant genişliği                     |100        |
|Bant Genişliği |0,1 5 GiB/sn kadar GiB başına MB/sn     |
|Anlık görüntü sayısı        |200       |

#### <a name="premium-file-limits"></a>Premium dosya sınırları

|Alan  |Hedef  |
|---------|---------|
|Boyut                  |1 TiB         |
|Dosya başına en fazla IOPS     |5.000         |
|Eşzamanlı işler    |2,000         |

### <a name="azure-file-sync-scale-targets"></a>Azure dosya eşitleme ölçek hedefleri

Azure dosya eşitleme ile sınırsız kullanım amacı tasarlanmıştır ancak sınırsız kullanım her zaman mümkün değildir. Aşağıdaki tabloda, Microsoft'un test sınırları gösterir ve de hangi hedeflerin sabit limitlerdir gösterir:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Azure kuyruk depolama ölçek hedefleri

[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Azure tablo depolama ölçek hedefleri

[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>Ayrıca Bkz.

- [Depolama Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/storage/)
- [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md)
- [Azure depolama çoğaltma](../storage-redundancy.md)
- [Microsoft Azure Depolama Performansı ve Ölçeklenebilirlik Onay Listesi](../storage-performance-checklist.md)