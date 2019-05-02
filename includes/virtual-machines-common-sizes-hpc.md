---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 04/26/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 4b7cda593bd4dd39a7220aa282529535c6a63bea
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64914562"
---
Azure H serisi sanal makineler (VM'ler) liderlik düzeyde performans ve ölçeklenebilirlik MPI, teslim etmek ve maliyet verimliliği çeşitli gerçek HPC iş yükleri için tasarlanmıştır.

HB serisi VM’ler sıvı dinamiği, belirtik sonlu öğe analizi ve hava durumu modelleme gibi bellek bant genişliğine dayalı uygulamalar için iyileştirilmiştir. HB VM’ler 60 AMD EPYC 7551 işlemci çekirdeği ve CPU çekirdeği başına 4 GB RAM içerir, hyperthreading yoktur. AMD EPYC platformu, 260 GB/sn’den fazla bellek bant genişliği sağlar.

HC serisi VM'ler, örtük sınırlı öğe analizi, moleküler dynamics ve hesaplama Kimya gibi yoğun bir hesaplama tarafından yönetilen uygulamalar için iyileştirilmiştir. HC VM’ler, 44 Intel Xeon Platinum 8168 işlemci çekirdeği ve CPU çekirdeği başına 8 GB RAM içerir ve hyperthreading yoktur. Intel'in zengin ekosistemi Intel matematik çekirdek kitaplığı gibi yazılım araçları, Intel Xeon Platinum platformu destekler.

100 Gb/sn HB hem HC Vm'leri özelliği, engelleyici olmayan bir fat Mellanox EDR InfiniBand RDMA tutarlı bir performans için yapılandırma ağaç. Tüm MPI türleri ve sürümleri, aynı zamanda RDMA fiiller de destek şekilde HB ve HC Vm'leri standart Mellanox/OFED sürücüleri destekler.

H serisi VM'ler, yüksek CPU frekansları ya da temel gereksinimleri başına büyük bellek tarafından yönetilen uygulamalar için iyileştirilmiştir. H serisi VM'ler özelliği 8 veya 16 Intel Xeon E5-2667 v3 işlemci çekirdeği, 7 veya 14 GB RAM, CPU çekirdeği başına ve hiçbir hiper iş parçacığı. H serisi, 56 Gb/sn özellikleri Mellanox FDR InfiniBand engelleyici olmayan bir fat, ağaç tutarlı RDMA performansı için yapılandırma. H serisi VM'ler, Intel MPI destek 5.x ve MS-MPI.

## <a name="hb-series"></a>HB serisi

Premium Depolama: Premium depolama desteklenen önbelleğe alma: Desteklenen

| Boyut | Sanal işlemci | İşlemci | Bellek (GB) | Bellek, GB/sn bant genişliği | Temel CPU frekansını (GHz) | Tüm çekirdek sıklığı (GHz, en yüksek) | Tek çekirdekli sıklığı (GHz, en yüksek) | RDMA performans (GB/sn) | MPI desteği | Geçici depolama (GB) | Maksimum veri diskleri | En fazla Ethernet NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB60rs | 60 | AMD EPYC 7551 | 240 | 263 | 2.0 | 2.55 | 2.55 | 100 | Tümü | 700 | 4 | 1 |

<br>

## <a name="hc-series"></a>HC serisi

Premium Depolama: Premium depolama desteklenen önbelleğe alma: Desteklenen


| Boyut | Sanal işlemci | İşlemci | Bellek (GB) | Bellek, GB/sn bant genişliği | Temel CPU frekansını (GHz) | Tüm çekirdek sıklığı (GHz, en yüksek) | Tek çekirdekli sıklığı (GHz, en yüksek) | RDMA performans (GB/sn) | MPI desteği | Geçici depolama (GB) | Maksimum veri diskleri | En fazla Ethernet NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HC44rs | 44 | Intel Xeon Platinum 8168 | 352 | 191 | 2.7 | 3.4 | 3.7 | 100 | Tümü | 700 | 4 | 1 |


<br>

## <a name="h-series"></a>H Serisi

ACU: 290-300

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

| Boyut | Sanal işlemci | İşlemci | Bellek (GB) | Bellek, GB/sn bant genişliği | Temel CPU frekansını (GHz) | Tüm çekirdek sıklığı (GHz, en yüksek) | Tek çekirdekli sıklığı (GHz, en yüksek) | RDMA performans (GB/sn) | MPI desteği | Geçici depolama (GB) | Maksimum veri diskleri | En fazla Ethernet NIC |
| --- | --- |--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 | 8 | Intel Xeon E5 2667 v3 | 56 | 40 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 1000 | 32 | 2 |
| Standard_H16 | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 |  56 | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standard_H8m | 8 | Intel Xeon E5 2667 v3 | 112 | 40 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 1000 | 32 | 2 |
| Standard_H16m | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | 56 | Intel 5.x, MS-MPI | 2000 | 64 | 4 |
| Standart_h16r <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3.6 | 56 | 2000 | Intel 5.x, MS-MPI | 64 | 4 |
| Standart_h16mr <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3.6 | 56 | 2000 | Intel 5.x, MS-MPI | 64 | 4 |

<sup>1</sup> MPI uygulamaları için adanmış RDMA arka uç ağı ultra düşük gecikmeli ve yüksek bant genişliği sağlayan FDR InfiniBand ağı tarafından etkinleştirilir.

<br>