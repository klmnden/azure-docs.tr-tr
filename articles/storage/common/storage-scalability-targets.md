---
title: Azure depolama ölçeklenebilirlik ve performans hedefleri | Microsoft Docs
description: Gelen ve giden bant genişliği hem de standart ve premium depolama hesapları için kapasite ve istek hızı dahil olmak üzere Azure depolama için ölçeklenebilirlik ve performans hedefleri hakkında bilgi edinin. Azure Depolama hizmetlerinin her biri bölümleri için performans hedeflerini anlayın.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 10/24/2017
ms.author: rogarana
ms.component: common
ms.openlocfilehash: 758871537b89a9c010cfaddf324e2208f9846afb
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51241336"
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Azure Depolama Ölçeklenebilirlik ve Performans Hedefleri
## <a name="overview"></a>Genel Bakış
Bu makalede, Azure depolama için ölçeklenebilirlik ve performans konuları açıklanır. Diğer Azure sınırları özeti için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md).

> [!NOTE]
> Tüm depolama hesapları, yeni bir düz ağ topolojisi üzerine çalıştırın ve oluşturuldukları zaman bağımsız olarak, bu makalede açıklanan ölçeklenebilirlik ve performans hedefleri destekler. Ölçeklenebilirlik ve Azure depolama düz ağ mimarisi üzerinde daha fazla bilgi için bkz: [Microsoft Azure Depolama: yüksek oranda kullanılabilir bulut depolama hizmet güçlü tutarlılıkla](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 

> [!IMPORTANT]
> Burada listelenen ölçeklenebilirlik ve performans hedefleri yüksek kaliteli hedefler, ancak ulaşılabilir. Tüm durumlarda, istek hızı ve bant genişliği, Depolama tarafından gerçekleştirilen hesap üzerinde saklanan nesneleri kullanılan, erişim desenlerini boyutuna bağlıdır ve iş yükü türüne uygulamanızı gerçekleştirir. Hizmetinizin performansını gereksinimlerinizi karşılayıp karşılamadığını belirlemek için test etmeyi unutmayın. Mümkünse, ani artışlar trafiğinin oranını önlemek ve bölümler arasında trafiği iyi dağıtılmış olduğundan emin olun.
> 
> Uygulamanızın hangi iş yükünüz için bir bölüm işleyebilir, sınırına ulaştığında, Azure depolama hata kodu: 503 (Sunucu meşgul) veya hata kodu 500 (işlem zaman aşımı) yanıtlarını döndürmek başlar. Bu hataların oluşma, uygulamanızı yeniden denemeler için bir üstel geri alma İlkesi kullanmalısınız. Üstel geri alma yükü azaltmak ve ani trafik bu bölüme kolaylaştırmak için bölüm sağlar.
> 
> 

Uygulamanızın ihtiyaçlarını tek bir depolama hesabı ölçeklenebilirlik hedefleri aşarsanız, birden çok depolama hesaplarını kullanmak için uygulamanızı oluşturabilirsiniz. Ardından, bu depolama hesabı arasında veri nesnelerinizi bölümleyebilirsiniz. Bkz: [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) birim fiyatlandırma hakkında bilgi için.

## <a name="scalability-targets-for-a-storage-account"></a>Bir depolama hesabı için ölçeklenebilirlik hedefleri
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

### <a name="storage-resource-provider-limits"></a>Depolama kaynak sağlayıcısı sınırları 

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Azure Blob Depolama ölçek hedefleri
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Azure dosyaları ölçek hedefleri
Azure dosyaları ve Azure dosya eşitleme için ölçek ve performans hedefleri hakkında daha fazla bilgi için bkz. [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Azure dosya eşitleme ölçek hedefleri
Bu her zaman mümkün değildir, ancak Azure dosya eşitleme ile sınırsız kullanım için tasarlamak mümkün olduğunca çalıştık. Aşağıdaki tabloda test işlemlerimizi sınırları ve hangi hedeflerin gerçekten sabit limitlerdir gösterir:

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
* [Microsoft Azure Depolama: Yüksek oranda kullanılabilir depolama ile bulut hizmeti güçlü tutarlılık](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

