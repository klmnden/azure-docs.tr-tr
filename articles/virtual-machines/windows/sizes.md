---
title: Azure'da Windows VM boyutları | Microsoft Docs
description: Azure'da Windows sanal makineler için farklı boyutlarda listeler.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/14/2018
ms.author: jonbeck
ms.openlocfilehash: b9a81fb451164aa58ca4ba943ed000c14d9f87cc
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873578"
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Azure'da Windows sanal makine boyutları

Bu makalede, sunulan boyutlar ve Windows uygulamaları ve iş yüklerini çalıştırmak için kullanabileceğiniz bir Azure sanal makineleri için seçenekleri açıklar. Ayrıca, ne zaman, bu kaynakları kullanmayı planlıyorsanız dikkat edilecekler sağlar.  Bu makale için de kullanılabilir olan [Linux sanal makineleri](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


| Tür                     | Boyutlar           |    Açıklama       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Genel amaçlı](sizes-general.md)          | B, Dsv3, Dv3, DSv2, Dv2, Av2, DC | Dengeli CPU/bellek oranı. Test ve geliştirme, küçük - orta boyutlu veritabanları, düşük - orta yoğunluklu trafiğe sahip web sunucuları için idealdir. |
| [İşlem için iyileştirilmiş](sizes-compute.md)        | Fsv2          | Yüksek CPU/bellek oranı. Orta yoğunlukta trafiğe sahip web sunucuları, ağ gereçleri, toplu işlemler ve uygulama sunucuları için uygundur.        |
| [Bellek için iyileştirilmiş](../virtual-machines-windows-sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2, Dv2  | Yüksek bellek CPU oranı. İlişkisel veritabanı sunucuları, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir.                 |
| [Depolama için iyileştirilmiş](../virtual-machines-windows-sizes-storage.md)        | Lsv2, Ls                | Yüksek disk aktarım hızı ve GÇ büyük veri, SQL, NoSQL veritabanları, veri ambarı ve büyük işlem veritabanları için ideal.  |
| [GPU](sizes-gpu.md)            | NV, NVv2, NC, NCv2, NCv3, ND, NDv2 (Önizleme)            | Özelleştirilmiş sanal makineler ağır grafik işlemleri ile video düzenleme için hedeflenmiş, hem de eğitim ve çıkarım (ND) ile ayrıntılı öğrenme modeli. Tek veya birden çok GPU ile kullanılabilir.       |
| [Yüksek performanslı işlem](sizes-hpc.md) | H       | İşleme düzeyi yüksek olan isteğe bağlı ağ arabirimleri (RDMA) içeren sanal makineler, şimdiye kadarki en hızlı ve en güçlü CPU ile sunuluyor. |


<br> 

- Çeşitli boyutlardaki fiyatlandırması hakkında daha fazla bilgi için bkz: [sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows). 
- Azure Vm'lerinde genel sınırları görmek için bkz [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md).
- Depolama maliyetleri, depolama hesabında kullanılan sayfa sayısına göre ayrıca hesaplanır. Ayrıntılar için [Azure depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).
- Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları arasında işlem performansını karşılaştırmanıza yardımcı olabilir.


## <a name="rest-api"></a>REST API

VM boyutları için sorgu için REST API kullanma hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Yeniden boyutlandırma için kullanılabilir sanal makine boyutlarını Listele](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [Bir abonelik için kullanılabilir sanal makine boyutlarını Listele](https://docs.microsoft.com/rest/api/compute/resourceskus/list)
- [Bir kullanılabilirlik kümesinde kullanılabilir sanal makine boyutlarını Listele](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları arasında işlem performansını karşılaştırmanıza yardımcı olabilir.

## <a name="benchmark-scores"></a>Kıyaslama puanları

Windows sanal makineleri kullanarak performans işlem hakkında daha fazla bilgi edinin [CoreMark Kıyaslama puanlarını](compute-benchmark-scores.md).

## <a name="next-steps"></a>Sonraki adımlar

Mevcut olan farklı VM boyutları hakkında daha fazla bilgi edinin:
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](../virtual-machines-windows-sizes-memory.md)
- [Depolama için iyileştirilmiş](../virtual-machines-windows-sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- Denetleme [önceki nesil](sizes-previous-gen.md) sayfası A standart, Dv1 (D1-4 ve D11-14 v1) ve A8-A11 serileri




