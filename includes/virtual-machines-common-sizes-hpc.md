---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 07/06/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: ac8686d0ea5704492bfc2e08972a2f70c9b34c43
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37906828"
---
Azure H serisi sanal makineler en son Vm'leri molekül modellemeleri ve hesaplama sıvı dinamiği gibi yüksek son işlem hesaplama gereksinimlerine yönelik yüksek performanslı bilgi işlem olur. Bu 8 ve 16 vCPU VM'ler Intel Haswell E5-2667 V3 işlemci teknolojisini DDR4 bellek ve SSD tabanlı geçici depolama üzerinde oluşturulur. 

H Serisi önemli miktarda CPU gücünün yanı sıra, FDR InfiniBand ile düşük gecikmeli RDMA ağ iletişimi için farklı seçeneklere ek olarak yoğun bellek kullanımlı işlem gereksinimlerini için çok sayıda bellek yapılandırması sunar.



## <a name="h-series"></a>H Serisi

ACU: 290-300

Premium Depolama: Desteklenmez

Premium depolama önbelleğe alma: Desteklenmez

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum diski aktarım hızı: IOPS | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |32 |32x500 |2  |
| Standard_H16 |16 |112 |2000 |64 |64x500 |4 |
| Standard_H8m |8 |112 |1000 |32 |32x500 |2  |
| Standard_H16m |16 |224 |2000 |64 |64x500 |4  |
| Standart_h16r <sup>1</sup> |16 |112 |2000 |64 |64x500 |4  |
| Standart_h16mr <sup>1</sup> |16 |224 |2000 |64 |64x500 |4 |

<sup>1</sup> MPI uygulamaları için adanmış RDMA arka uç ağı ultra düşük gecikmeli ve yüksek bant genişliği sağlayan FDR InfiniBand ağı tarafından etkinleştirilir.

<br>






