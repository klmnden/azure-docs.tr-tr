---
title: Azure'da Linux VM boyutları | Microsoft Docs
description: Azure'da Linux sanal makineleri için kullanılabilen farklı boyutlarını listeler.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/01/2018
ms.author: jonbeck
ms.openlocfilehash: a779f29dd0acf633a8bd9390c68992342b4cee53
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394774"
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Azure'da Linux sanal makine boyutları
Bu makale, sunulan boyutlar ve Seçenekler Linux uygulamaları ve iş yüklerini çalıştırmak için kullanabileceğiniz bir Azure sanal makineleri için açıklar. Ayrıca, ne zaman, bu kaynakları kullanmayı planlıyorsanız dikkat edilecekler sağlar. Bu makale için de kullanılabilir olan [Windows sanal makineleri](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


| Tür                     | Boyutlar           |    Açıklama       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Genel amaçlı](sizes-general.md)          | B, Dsv3, Dv3, DSv2, Dv2, Av2, DC  | Dengeli CPU/bellek oranı. Test ve geliştirme, küçük - orta boyutlu veritabanları, düşük - orta yoğunluklu trafiğe sahip web sunucuları için idealdir. |
| [İşlem için iyileştirilmiş](sizes-compute.md)        | Fsv2, Fs, F             | Yüksek CPU/bellek oranı. Orta yoğunlukta trafiğe sahip web sunucuları, ağ gereçleri, toplu işlemler ve uygulama sunucuları için uygundur.        |
| [Bellek için iyileştirilmiş](sizes-memory.md)         | Esv3, Ev3, M, GS, G, DSv2, Dv2  | Yüksek bellek CPU oranı. İlişkisel veritabanı sunucuları, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir.                 |
| [Depolama için iyileştirilmiş](sizes-storage.md)        | Ls                | Yüksek disk aktarım hızı ve GÇ. Büyük Veri, SQL ve NoSQL veritabanları için ideal.                                                         |
| [GPU](sizes-gpu.md)            | NV, NVv2, NC, NCv2, NCv3, ND            | Özelleştirilmiş sanal makineler ağır grafik işlemleri ile video düzenleme için hedeflenmiş, hem de eğitim ve çıkarım (ND) ile ayrıntılı öğrenme modeli. Tek veya birden çok GPU ile kullanılabilir.       |
| [Yüksek performanslı işlem](sizes-hpc.md) | H       | İşleme düzeyi yüksek olan isteğe bağlı ağ arabirimleri (RDMA) içeren sanal makineler, şimdiye kadarki en hızlı ve en güçlü CPU ile sunuluyor. |


<br>

- Çeşitli boyutlardaki fiyatlandırması hakkında daha fazla bilgi için bkz: [sanal makineler fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- Azure bölgeleri VM boyutları kullanılabilirliği için bkz: [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).
- Azure Vm'lerinde genel sınırları görmek için bkz [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md).
- Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları arasında işlem performansını karşılaştırmanıza yardımcı olabilir.


## <a name="rest-api"></a>REST API

VM boyutları için sorgu için REST API kullanma hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Yeniden boyutlandırma için kullanılabilir sanal makine boyutlarını Listele](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines_listavailablesizes)
- [Bir abonelik için kullanılabilir sanal makine boyutlarını Listele](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines_listall)
- [Bir kullanılabilirlik kümesinde kullanılabilir sanal makine boyutlarını Listele](https://docs.microsoft.com/rest/api/compute/availabilitysets/availabilitysets_listavailablesizes)

## <a name="acu"></a>ACU

Hakkında daha fazla bilgi [Azure işlem birimleri (ACU)](acu.md) Azure SKU'ları arasında işlem performansını karşılaştırmanıza yardımcı olabilir.

## <a name="benchmark-scores"></a>Kıyaslama puanları

Linux Vm'leri kullanarak performans işlem hakkında daha fazla bilgi edinin [CoreMark Kıyaslama puanlarını](compute-benchmark-scores.md).

## <a name="next-steps"></a>Sonraki adımlar

Mevcut olan farklı VM boyutları hakkında daha fazla bilgi edinin:
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- Denetleme [önceki nesil](sizes-previous-gen.md) sayfası A standart, Dv1 (D1-4 ve D11-14 v1) ve A8-A11 serileri



