---
title: "include dosyası"
description: "include dosyası"
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: ee145e5784ae6da6cce22f1b21f9a5d45335ea8b
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
A8-A11 ve H Serisi boyutlar *yoğun işlem gücü kullanımlı örnekler* olarak da bilinir. Bu boyutları çalıştıran donanım; yüksek performanslı bilgi işlem (HPC) kümesi uygulamaları, modellemeler ve simülasyonlar gibi yoğun işlem ve ağ kullanımlı uygulamalar için tasarlanmış ve iyileştirilmiştir. A8-A11 Serisinde, Intel Xeon E5-2670 @ 2,6 GHZ, H Serisinde ise Intel Xeon E5-2667 v3 @ 3,2 GHz işlemciler kullanılmaktadır.  Bu makale Vcpu, veri diskleri ve NIC yanı sıra bu gruplandırmadaki her boyutu için depolama üretilen iş ve ağ bant sayısı hakkında bilgi sağlar. 

Yüksek performanslı bilgi işlem VM'ler molecular modelleme ve hesaplama sıvı dinamiği gibi yüksek son hesaplama gereksinimlerine yönelik en son Azure H-serisi sanal makine yok. Bu 8 ve 16 vCPU VM'ler Intel Haswell E5-2667 V3 işlemci teknolojisi ekranlarını DDR4 bellek ve SSD tabanlı geçici depolama üzerinde oluşturulmuştur. 

H Serisi önemli miktarda CPU gücünün yanı sıra, FDR InfiniBand ile düşük gecikmeli RDMA ağ iletişimi için farklı seçeneklere ek olarak yoğun bellek kullanımlı işlem gereksinimlerini için çok sayıda bellek yapılandırması sunar.



## <a name="h-series"></a>H Serisi

ACU: 290-300

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum diski aktarım hızı: IOPS | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |32 |32x500 |2  |
| Standard_H16 |16 |112 |2000 |64 |64x500 |4 |
| Standard_H8m |8 |112 |1000 |32 |32x500 |2  |
| Standard_H16m |16 |224 |2000 |64 |64x500 |4  |
| Standard_H16r <sup>1</sup> |16 |112 |2000 |64 |64x500 |4  |
| Standart_h16mr <sup>1</sup> |16 |224 |2000 |64 |64x500 |4 |

<sup>1</sup> MPI uygulamaları için ayrılmış RDMA arka uç ağ çok düşük gecikme ve yüksek bant genişliği sunar FDR InfiniBand ağ tarafından etkinleştirilir.

<br>



## <a name="a-series---compute-intensive-instances"></a>A Serisi - Yoğun işlem gücü kullanımlı örnekler

ACU: 225

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (HDD): GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | En fazla NIC|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8 <sup>1</sup> |8 |56 |382 |32 |32x500 |2 |
| Standard_A9 <sup>1</sup> |16 |112 |382 |64 |64x500 |4 |
| Standard_A10 |8 |56 |382 |32 |32x500 |2  |
| Standard_A11 |16 |112 |382 |64 |64x500 |4 |

<sup>1</sup>MPI uygulamaları için ayrılmış RDMA arka uç ağ çok düşük gecikme ve yüksek bant genişliği sunar FDR InfiniBand ağ tarafından etkinleştirilir.

<br>



