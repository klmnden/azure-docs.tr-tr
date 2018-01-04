---
title: "Linux VM boyutları Azure'da | Microsoft Docs"
description: "Azure'daki Linux sanal makineler için kullanılabilir farklı boyutlarını listeler."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 11/08/2017
ms.author: jonbeck
<<<<<<< HEAD
ms.openlocfilehash: 79ca33c7b1cf55782911cbe9a27fd47547c41c0e
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
=======
ms.openlocfilehash: 568048c6117d81ec351d5057505dafeec58d7b5a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Azure'daki Linux sanal makineler için Boyutlar
Bu makalede, kullanılabilir boyutları ve Linux uygulamaları ve iş yüklerini çalıştırmak için kullanabileceğiniz Azure sanal makineleri için seçenekleri açıklar. Ayrıca, ne zaman, bu kaynakları kullanmayı planlıyorsanız bulundurmanız dağıtımında dikkat edilecek noktalar sağlar. Bu makalede ayrıca kullanılabilir [Windows sanal makineleri](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


| Tür                     | Boyutlar           |    Açıklama       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Genel amaçlı](sizes-general.md)          | B, Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7  | Dengeli CPU/bellek oranı. Test ve geliştirme, küçük - orta boyutlu veritabanları, düşük - orta yoğunluklu trafiğe sahip web sunucuları için idealdir. |
| [İşlem için iyileştirilmiş](sizes-compute.md)        | Fsv2, Fs, F             | Yüksek CPU/bellek oranı. Orta yoğunlukta trafiğe sahip web sunucuları, ağ gereçleri, toplu işlemler ve uygulama sunucuları için uygundur.        |
| [Bellek için iyileştirilmiş](sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Yüksek bellek CPU oranı. İlişkisel veritabanı sunucuları, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir.                 |
| [Depolama için iyileştirilmiş](sizes-storage.md)        | Ls                | Yüksek disk aktarım hızı ve GÇ. Büyük Veri, SQL ve NoSQL veritabanları için ideal.                                                         |
| [GPU](sizes-gpu.md)            | NV, NC, NCv2, ND            | Yoğun Grafik işleme ve video düzenleme için hedeflenen özelleştirilmiş sanal makineler. Tek veya birden çok GPU ile kullanılabilir.       |
| [Yüksek performanslı işlem](sizes-hpc.md) | H, A8-11          | İşleme düzeyi yüksek olan isteğe bağlı ağ arabirimleri (RDMA) içeren sanal makineler, şimdiye kadarki en hızlı ve en güçlü CPU ile sunuluyor. 

<br>

- Çeşitli boyutlarda fiyatlandırma hakkında daha fazla bilgi için bkz: [sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- VM boyutları Azure bölgelerindeki kullanılabilirlik için bkz: [bölgeye göre ürünleri](https://azure.microsoft.com/regions/services/).
- Azure Vm'lerinde genel sınırları görmek için bkz [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../../azure-subscription-service-limits.md).
- Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları üzerinde işlem performans karşılaştırmanıza yardımcı olur.


## <a name="rest-api"></a>REST API'si

VM boyutları için sorgu için REST API kullanma hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Yeniden boyutlandırma için kullanılabilir sanal makine boyutlarını Listele](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [Bir abonelik için kullanılabilir sanal makine boyutlarını Listele](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [Bir kullanılabilirlik kümesinde kullanılabilir sanal makine boyutlarını Listele](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a>ACU

Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları üzerinde işlem performans karşılaştırmanıza yardımcı olur.

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir farklı VM boyutları hakkında daha fazla bilgi edinin:
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)



