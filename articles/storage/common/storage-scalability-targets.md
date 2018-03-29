---
title: Azure Storage ölçeklenebilirlik ve performans hedefleri | Microsoft Docs
description: Azure depolama kapasitesi, istek hızı ve her iki standart ve premium depolama hesapları için gelen ve giden bant genişliği de dahil olmak üzere, ölçeklenebilirlik ve performans hedefleri hakkında bilgi edinin. Bölümler içinde Azure Storage hizmetlerinin her biri için performans hedefleri anlayın.
services: storage
documentationcenter: na
author: roygara
manager: jeconnoc
editor: tysonn
ms.assetid: be721bd3-159f-40a1-88c1-96418537fe75
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 10/24/2017
ms.author: rogarana
ms.openlocfilehash: e393bb9e7615b893699caf5a931ede5803046892
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Azure Depolama Ölçeklenebilirlik ve Performans Hedefleri
## <a name="overview"></a>Genel Bakış
Bu makalede, Azure Storage ölçeklenebilirlik ve performans konuları açıklanmaktadır. Diğer Azure sınırları özeti için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../../azure-subscription-service-limits.md).

> [!NOTE]
> Tüm depolama hesaplarının yeni düz ağ topolojisine çalıştırın ve oluşturuldukları zaman bağımsız olarak, bu makalede açıklanan ölçeklenebilirlik ve performans hedefleri destekler. Ölçeklenebilirlik ve Azure depolama düz ağ mimarisi üzerinde daha fazla bilgi için bkz: [Microsoft Azure Storage: yüksek oranda kullanılabilir bulut depolama hizmet güçlü tutarlılık](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 

> [!IMPORTANT]
> Burada listelenen ölçeklenebilirlik ve performans hedefleri Gelişmiş hedefleri, ancak ulaşılabilir. Uygulamanızı iş yükü türünü gerçekleştirir ve tüm durumlarda, istek hızı ve bant genişliği, depolama birimi tarafından elde saklanan nesneleri kullanılan, erişim desenlerini boyutuna bağlı hesap bağlıdır. Hizmetinizin performansını gereksinimlerinizi karşılayıp karşılamadığını belirlemek için test emin olun. Mümkünse, trafiğinin oranını içindeki ani ani önlemek ve bölümler trafiği iyi dağıtılmış olduğundan emin olun.
> 
> Uygulamanızın ne bir bölüm için iş yükünü işleyebilir, sınırına ulaştığında, hata kodu 503 (Sunucu meşgul) ya da hata kodu 500 (işlemi zaman aşımı) yanıtları döndürmek Azure Storage başlar. Bu hatalar ortaya çıkan, uygulamanız için yeniden deneme üstel geri alma İlkesi kullanmalısınız. Üstel geri alma yükü azaltmak ve o bölümün trafiğinin ani çıkışı kolaylaştırmak için bölüm olanak tanır.
> 
> 

Uygulamanızın gereksinimlerine tek bir depolama hesabı ölçeklenebilirlik hedeflerini aşarsa, birden çok depolama hesabı kullanmak için uygulamanızı oluşturabilirsiniz. Ardından bu depolama hesapları arasında veri nesnelerinizi bölüm. Bkz: [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) birim fiyatlandırma hakkında bilgi için.

## <a name="scalability-targets-for-a-storage-account"></a>Bir depolama hesabı için ölçeklenebilirlik hedefleri
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Azure Blob Depolama ölçek hedefleri
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Azure dosyaları ölçek hedefleri
Azure dosyaları ve Azure dosya eşitleme ölçek ve performans hedefleri hakkında daha fazla bilgi için bkz: [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Azure dosya eşitleme ölçek hedefleri
[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Azure kuyruk depolama ölçek hedefleri
[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Azure tablo depolama ölçek hedefleri
[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>Ayrıca Bkz.
* [Depolama fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/storage/)
* [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../../azure-subscription-service-limits.md)
* [Azure Storage çoğaltma](../storage-redundancy.md)
* [Microsoft Azure Depolama Performansı ve Ölçeklenebilirlik Onay Listesi](../storage-performance-checklist.md)
* [Microsoft Azure Storage: Yüksek oranda kullanılabilir depolama sahip bulut hizmeti güçlü tutarlılık](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

