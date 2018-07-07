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
ms.openlocfilehash: 033ae1de25fbaada0c2bce715e6bdd71818b341a
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37906811"
---
<!-- F-series, Fs-series* -->

İşlem VM boyutları en iyi duruma getirilmiş bir yüksek CPU / bellek oranına sahip olması ve orta düzeyde trafiğe sahip web sunucuları, ağ Gereçleri, toplu işlemleri ve uygulama sunucuları için uygundur. Bu makalede, Vcpu, veri diskleri ve NIC yanı sıra depolama aktarım hızı ve ağ bant genişliği için bu gruplandırma her boyutundaki sayısı hakkında bilgi sağlar.

Fsv2 serisi 2,7 GHz temel çekirdek sıklığını ve 3.7 GHz en fazla tek çekirdekli turbo sıklığını Intel® Xeon® Platinum 8168 işlemci üzerinde temel alır. Intel ölçeklenebilir işlemciler üzerinde yenidir Intel® AVX-512 yönergeler, hem tek hem de çift duyarlıklı kayan nokta işlemleri üzerinde vektör işleme iş yükleri için yüksek performans X 2 kadar sağlar. Diğer bir deyişle, bunlar için herhangi bir hesaplama iş yükünü hızlı. 

Daha düşük bir saatlik liste fiyatına Fsv2 serisi fiyat-performans alanında temel Azure işlem birimi (ACU) açısından vCPU başına Azure Portföyündeki en iyi değerdir. 

F Serisi, Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan saat hızlarına ulaşabilen 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemciyi temel alır. Bu değer Dv2 Serisi VM'lerdeki CPU performansıyla aynıdır.  

F Serisi VM'ler, daha hızlı CPU'ya ihtiyaç duyan ancak çok fazla belleğe veya vCPU başına geçici depolamaya ihtiyaç duymayan iş yükleri için harika bir seçenektir.  Analitikler, oyun sunucuları, web sunucuları ve toplu işleme gibi iş yükleri, F Serisinin özelliklerinden en fazla yarar sağlayacak iş yükleridir.

Fs serisi, Premium depolamaya ek olarak F serisinin tüm avantajlarını sağlar.

## <a name="fsv2-series-sup1sup"></a>Fsv2 serisi <sup>1</sup>

ACU: 195-210

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: desteklenir

| Boyut             | vCPU'ın | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|------------------------------------------------|
| Standard_F2s_v2  | 2      | 4           | 16             | 4              | 4000 (32)                                                             | 2 / 875                                        |
| Standard_F4s_v2  | 4      | 8           | 32             | 8              | 8000 (64)                                                             | 2 / 1,750                                      |
| Standard_F8s_v2   | 8      | 16          | 64             | 16             | 16000 (128)                                                           | 4 / 3.500                                      |
| Standard_F16s_v2 | 16     | 32          | 128            | 32             | 32000 (256)                                                           | 4 / 7000                                      |
| Standard_F32s_v2 | 32     | 64          | 256            | 32             | 64000 (512)                                                           | 8 / 14000                                     |
| Standard_F64s_v2 | 64     | 128         | 512            | 32             | 128000 (1024)                                                         | 8 / 28,000                                     |
| Standard_F72s_v2<sup>2, 3</sup> | 72     | 144         | 576            | 32             | 144000 (1520)                                                         | 8 / 30,000                                     |

<sup>1</sup> Fsv2 serisi sanal makineler Intel® Hyper-Threading teknolojisine özelliği

<sup>2</sup> 64'ten fazla şu desteklenen konuk işletim sistemlerinden birini gerektirir: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 ve Red Hat Enterprise Linux, CentOS 7.3 veya LIS 4.2.1 ile Oracle Linux 7.3

<sup>3</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.

## <a name="fs-series-sup1sup"></a>FS serisi <sup>1</sup>

ACU: 210 - 250

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |4 |4000/32 (12) |3200/48 |2 / 750 |
| Standard_F2s |2 |4 |8 |8 |8000/64 (24) |6400/96 |2 / 1500 |
| Standard_F4s |4 |8 |16 |16 |16.000/128 (48) |12.800/192 |4 / 3000 |
| Standard_F8s |8 |16 |32 |32 |32.000/256 (96) |25.600/384 |8 / 6000 |
| Standard_F16s |16 |32 |64 |64 |64.000/512 (192) |51.200/768 |8 / 12000 |

MB/sn = 10^6 bayt/saniye ve GiB = 1024^3 bayt.

<sup>1</sup> maksimum disk aktarım hızı (IOPS veya MB/sn) Fs serisi VM ile sınırlı olabilir sayısı, boyutu ve bölümleme türüyle ekli disklerin.  Ayrıntılar için bkz. [Premium Depolama: Azure sanal makine iş yükleri için yüksek performanslı depolama](../articles/virtual-machines/windows/premium-storage.md).


<br>

## <a name="f-series"></a>F Serisi

ACU: 210 - 250

Premium Depolama: Desteklenmez

Premium depolama önbelleğe alma: Desteklenmez

| Boyut         | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / Okuma MB/sn / Yazma MB/sn | Maksimum veri diski/aktarım hızı: IOPS | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3000/46/23                                           | 4/4x500                         | 2 / 750                 |
| Standard_F2  | 2         | 4           | 32             | 6000/93/46                                           | 8/8x500                         | 2 / 1500                     |
| Standard_F4  | 4         | 8           | 64             | 12000/187/93                                         | 16/16x500                         | 4 / 3000                     |
| Standard_F8  | 8         | 16          | 128            | 24000/375/187                                        | 32/32x500                       | 8 / 6000                     |
| Standard_F16 | 16        | 32          | 256            | 48000/750/375                                        | 64 / 64 x 500                       | 8 / 12000           |


<br>


