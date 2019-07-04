---
title: Azure NetApp dosyaları performans değerlendirmeleri | Microsoft Docs
description: Azure NetApp dosyaları için performans konuları açıklanmaktadır.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: b-juche
ms.openlocfilehash: 97e3c6212edd2ade4eabb96db3543e9b3b68e2ae
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454145"
---
# <a name="performance-considerations-for-azure-netapp-files"></a>Azure NetApp Files için performansla ilgili önemli noktalar

[Aktarım hızı sınırı](azure-netapp-files-service-levels.md) birim birime atanan kota ve hizmet düzeyi seçili bir birleşimi tarafından belirlenir. Azure NetApp dosyalarla ilgili performans planları yaptığınızda, bazı önemli noktalar anlamanız gerekir. 

## <a name="quota-and-throughput"></a>Kota ve aktarım hızı  

Aktarım hızı, yalnızca bir etken gerçekleşmiş gerçek performans sınırdır.  

Tipik depolama performansla ilgili önemli noktalar dahil olmak üzere okuyup karışımı, teslim eksiksiz bir performans, Aktarım boyutu, rastgele veya sıralı desenleri ve diğer birçok faktörü katkıda bulunur.  

Test gözlemlenen en yüksek görgül aktarım 4,500 MiB/sn ' dir.  Premium depolama katmanında 70.31 TiB birimi kotası, bu performans düzeyine ulaşmak için yüksek bir aktarım hızı sınırı sağlanır.  

Kota miktarları 70.31 TiB aştığında birim atama düşünüyorsanız, ek verileri depolamak için bir birime ek kota atanabilir. Ancak, ek kota gerçek aktarım hızı başka bir artış açmayacaktır.  

Bkz: [Azure NetApp dosyaları için performans kıyaslamalarını](azure-netapp-files-performance-benchmarks.md) ek bilgi için.

## <a name="overprovisioning-the-volume-quota"></a>Açıdan hacmi kotası aşıldı

Bir iş yükünün performansı, aktarım hızı sınırı bağlı ise, yüksek aktarım hızı ve daha iyi performans elde hacmi kotası fazladan mümkündür.  

Premium depolama katmanı bir birimde verilerin yalnızca 500 GiB ancak 128 MiB/sn aktarım hızı gerektiren yoksa üretilen iş düzeyi buna uygun olarak ayarlanır, böylece Örneğin, kota 2 TiB olarak ayarlayabilirsiniz (TB başına 64 MiB/sn * 2 TiB = 128 MiB/sn).  

Tutarlı bir şekilde daha yüksek bir performans elde etmek için bir birim fazladan, daha yüksek bir hizmet düzeyi kullanmayı düşünün.  Yukarıdaki örnekte, Ultra yüksek depolama katmanını kullanarak aynı yarı hacmi kotası aktarım hızı sınırına ulaşabilir (TiB başına 128 MiB/sn * 1 TiB = 128 MiB/sn).

## <a name="dynamically-increasing-or-decreasing-volume-quota"></a>Dinamik olarak artan veya azalan hacmi kotası aşıldı

Performans gereksinimlerinizi doğası gereği geçici veya bir sabit bir süre boyunca performans gereksinimlerine artırmıştır, dinamik olarak artırır veya azaltırsanız aktarım hızı sınırına eşzamanlı olarak ayarlamak için hacmi kotası aşıldı.  Aşağıdaki konuları göz önünde bulundurun: 

* Birim kota artırabilir veya azaltılabilir GÇ duraklatmak için gerek ve birime erişimi kesildi veya etkilenen değil.  

    Bir birim karşı etkin bir g/ç işlem sırasında bir kota ayarlayabilirsiniz.  Bu birimi kotası hiçbir zaman birimi içinde depolanan mantıksal veri miktarı aşağıdaki azaltılabilir unutmayın.

* Kota toplu değiştirildiğinde, aktarım hızı sınırı karşılık gelen değişiklik neredeyse anında. 

    Değişikliği kesme veya birim erişimi veya g/ç desteklemez.  

* Ayarlama birim kota kapasitesi havuzu boyutundaki bir değişikliği gerektirir.  

    Dinamik olarak ve birim kullanılabilirliğine veya g/ç etkilemeden kapasitesi havuzu boyutu ayarlanabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp dosyaları için hizmet düzeyleri](azure-netapp-files-service-levels.md)
- [Azure NetApp dosyaları için performans kıyaslamalarını](azure-netapp-files-performance-benchmarks.md)