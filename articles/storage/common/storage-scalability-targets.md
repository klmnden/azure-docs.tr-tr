---
title: "Azure Storage ölçeklenebilirlik ve performans hedefleri | Microsoft Docs"
description: "Azure depolama kapasitesi, istek hızı ve her iki standart ve premium depolama hesapları için gelen ve giden bant genişliği de dahil olmak üzere, ölçeklenebilirlik ve performans hedefleri hakkında bilgi edinin. Bölümler içinde Azure Storage hizmetlerinin her biri için performans hedefleri anlayın."
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: be721bd3-159f-40a1-88c1-96418537fe75
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 07/12/2017
ms.author: tamram
ms.openlocfilehash: 805b0eee46846345ee1f33faf0c28393c3e8ebb1
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="azure-storage-scalability-and-performance-targets"></a>Azure Depolama Ölçeklenebilirlik ve Performans Hedefleri
## <a name="overview"></a>Genel Bakış
Bu konuda, Microsoft Azure Storage ölçeklenebilirlik ve performans konuları açıklanmaktadır. Diğer Azure sınırları özeti için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../../azure-subscription-service-limits.md).

> [!NOTE]
> Tüm depolama hesaplarının yeni düz ağ topolojisine çalıştırın ve ne zaman oluşturulduğu bağımsız olarak, aşağıda açıklanan ölçeklenebilirlik ve performans hedefleri destekler. Ölçeklenebilirlik ve Azure depolama düz ağ mimarisi üzerinde daha fazla bilgi için bkz: [Microsoft Azure Storage: yüksek oranda kullanılabilir bulut depolama hizmet güçlü tutarlılık](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).
> 
> [!IMPORTANT]
> Burada listelenen ölçeklenebilirlik ve performans hedefleri Gelişmiş hedefleri, ancak ulaşılabilir. Uygulamanızı iş yükü türünü gerçekleştirir ve tüm durumlarda, istek hızı ve bant genişliği, depolama birimi tarafından elde saklanan nesneleri kullanılan, erişim desenlerini boyutuna bağlı hesap bağlıdır. Hizmetinizin performansını gereksinimlerinizi karşılayıp karşılamadığını belirlemek için test emin olun. Mümkünse, trafiğinin oranını içindeki ani ani önlemek ve bölümler trafiği iyi dağıtılmış olduğundan emin olun.
> 
> Uygulamanızın ne bir bölüm için iş yükünü işleyebilir, sınırına ulaştığında, Azure depolama hata kodu 503 (Sunucu meşgul) veya hata kodu 500 (işlemi zaman aşımı) yanıtları döndürülecek başlar. Bu durumda, uygulama için yeniden deneme üstel geri alma İlkesi kullanmanız gerekir. Üstel geri alma yükü azaltmak ve o bölümün trafiğinin ani çıkışı kolaylaştırmak için bölüm olanak tanır.
> 
> 

Uygulamanızın gereksinimlerine tek bir depolama hesabı ölçeklenebilirlik hedeflerini aşarsa, birden çok depolama hesabı kullanmak için uygulamanızı oluşturmak ve bu depolama hesapları arasında veri nesnelerinizi bölüm. Bkz: [Azure Storage fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) birim fiyatlandırma hakkında bilgi için.

## <a name="scalability-targets-for-a-storage-account"></a>Bir depolama hesabı için ölçeklenebilirlik hedefleri
[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Azure Blob Depolama ölçek hedefleri
[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Azure dosyaları ölçek hedefleri
Azure dosyaları ve Azure dosya eşitleme için ölçek ve performans hedefleri hakkında daha fazla bilgi için lütfen bkz [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](../files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Azure dosya eşitleme ölçek hedefleri
[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Azure kuyruk depolama ölçek hedefleri
[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Azure tablo depolama ölçek hedefleri
[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
## <a name="scalability-targets-for-virtual-machine-disks"></a>Sanal makine disklerini için ölçeklenebilirlik hedefleri
[!INCLUDE [azure-storage-limits-vm-disks](../../../includes/azure-storage-limits-vm-disks.md)]

Bkz: [Windows VM boyutları](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [Linux VM boyutları](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ek ayrıntılar için.

## <a name="managed-virtual-machine-disks"></a>Yönetilen sanal makine disklerini

[!INCLUDE [azure-storage-limits-vm-disks-managed](../../../includes/azure-storage-limits-vm-disks-managed.md)]

## <a name="unmanaged-virtual-machine-disks"></a>Yönetilmeyen sanal makine disklerini
[!INCLUDE [azure-storage-limits-vm-disks-standard](../../../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="see-also"></a>Ayrıca Bkz.
* [Depolama fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/storage/)
* [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../../azure-subscription-service-limits.md)
* [Premium Depolama: Azure Sanal Makine İş Yükleri için Yüksek Performanslı Depolama](../../virtual-machines/windows/premium-storage.md)
* [Azure Storage çoğaltma](../storage-redundancy.md)
* [Microsoft Azure depolama performans ve ölçeklenebilirlik Yapılacaklar listesi](../storage-performance-checklist.md)
* [Microsoft Azure Storage: Yüksek oranda kullanılabilir depolama sahip bulut hizmeti güçlü tutarlılık](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

