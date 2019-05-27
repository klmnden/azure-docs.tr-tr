---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 04/02/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: f1f02f8b5488609a0c69a6d335c96d3cc9266c71
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170356"
---
<!-- F-series, Fs-series* -->

İşlem VM boyutları en iyi duruma getirilmiş bir yüksek CPU / bellek oranına sahip olması ve orta düzeyde trafiğe sahip web sunucuları, ağ Gereçleri, toplu işlemleri ve uygulama sunucuları için uygundur. Bu makalede, Vcpu, veri diskleri ve NIC yanı sıra depolama aktarım hızı ve ağ bant genişliği için bu gruplandırma her boyutundaki sayısı hakkında bilgi sağlar.

Fsv2 serisi Intel® Xeon® Platinum 8168 işlemciye bağlı olduğu için sürekli bir özelliğe sahip olan tüm 3.4 GHz Turbo saat hızı ve 3.7 GHz en fazla tek çekirdekli turbo sıklığını çekirdek. Intel ölçeklenebilir işlemciler üzerinde yenidir Intel® AVX-512 yönergeler, hem tek hem de çift duyarlıklı kayan nokta işlemleri üzerinde vektör işleme iş yükleri için yüksek performans X 2 kadar sağlar. Diğer bir deyişle, bunlar için herhangi bir hesaplama iş yükünü hızlı. 

Daha düşük bir saatlik liste fiyatına Fsv2 serisi fiyat-performans alanında temel Azure işlem birimi (ACU) açısından vCPU başına Azure Portföyündeki en iyi değerdir.

## <a name="fsv2-series-sup1sup"></a>Fsv2 serisi <sup>1</sup>

ACU: 195 - 210

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

| Boyutlandır             | vCPU's | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|------------------|--------|-------------|----------------|----------------|--------------------------|--------------------------|-------------------------|
| Standard_F2s_v2  | 2      | 4           | 16             | 4              | 4000 / 31 (32)           | 3200 / 47                | 2 / 875                 |
| Standard_F4s_v2  | 4      | 8           | 32             | 8              | 8000 / 63 (64)           | 6400 / 95                | 2 / 1,750               |
| Standard_F8s_v2  | 8      | 16          | 64             | 16             | 16000 / 127 (128)        | 12800 / 190              | 4 / 3,500               |
| Standard_F16s_v2 | 16     | 32          | 128            | 32             | 32000 / 255 (256)        | 25600 / 380              | 4 / 7,000               |
| Standard_F32s_v2 | 32     | 64          | 256            | 32             | 64000 / 512 (512)        | 51200 / 750              | 8 / 14,000              |
| Standard_F64s_v2 | 64     | 128         | 512            | 32             | 128000 / 1024 (1024)     | 80000 / 1100             | 8 / 28,000              |
| Standard_F72s_v2<sup>2, 3</sup> | 72 | 144 | 576         | 32             | 144000 / 1152 (1520)     | 80000 / 1100             | 8 / 30,000              |


<sup>1</sup> Fsv2 serisi sanal makineler Intel® Hyper-Threading teknolojisine özelliği

<sup>2</sup> 64'ten fazla şu desteklenen konuk işletim sistemlerinden birini gerektirir: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 ve Red Hat Enterprise Linux, CentOS 7.3 veya LIS 4.2.1 ile Oracle Linux 7.3

<sup>3</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.