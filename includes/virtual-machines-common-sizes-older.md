---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, virtual-machines-linux
author: laurenhughes
ms.service: multiple
ms.topic: include
ms.date: 04/11/2019
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 7196a2ea794c1d17a2c55c05accb447d83929972
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64744677"
---
Bu bölümde, eski nesil sanal makine boyutları hakkında bilgi sağlar. Bu boyutları hala desteklenmektedir, ancak ek kapasite almaz. Genel kullanıma sunulmuş olan yeni veya farklı boyutları vardır. Lütfen [boyutları için Windows azure'da sanal makineler](../articles/virtual-machines/windows/sizes.md) veya [azure'da Linux sanal makine boyutları](../articles/virtual-machines/linux/sizes.md) VM seçmek için en iyi boyutlar gereksinimlerinize uyacak.  

Bir Linux VM yeniden boyutlandırma ile ilgili daha fazla bilgi için bkz: [Azure CLI kullanarak bir Linux sanal makinesi yeniden boyutlandırma](../articles/virtual-machines/linux/change-vm-size.md). Windows Vm'leri kullanıyorsanız ve PowerShell kullanmayı tercih ediyorsanız, bkz. [Windows VM'yi yeniden boyutlandırma](../articles/virtual-machines/windows/resize-vm.md).  

<br>

### <a name="basic-a"></a>Temel A  

**Yeni boyut önerisi**: [Av2 serisi](../articles/virtual-machines/windows/sizes-general.md#av2-series)

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

Temel katman boyutları genelde geliştirme iş yükleri ve yük dengeleme, otomatik ölçeklendirme veya bellek kullanımı yoğun sanal makineler gerektirmeyen diğer uygulamalar içindir.

|Boyut – Boyut\Ad | Sanal işlemci |Bellek|NIC'ler (Maks.)|En fazla geçici disk boyutu |En çok, Veri diskleri (her biri 1.023 GB)|En çok, IOPS (disk başına 300)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 MB|2| 20 GB|1|1x300|
|A1\Basic_A1|1|1,75 GB|2| 40 GB |2|2x300|
|A2\Basic_A2|2|3,5 GB|2| 60 GB|4|4x300|
|A3\Basic_A3|4|7 GB|2| 120 GB |8|8x300|
|A4\Basic_A4|8|14 GB|2| 240 GB |16|16x300|

<br>

### <a name="a-series"></a>A Serisi  

**Yeni boyut önerisi**: [Av2 serisi](../articles/virtual-machines/windows/sizes-general.md#av2-series)

ACU: 50-100

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (HDD): GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | Maks NIC / beklenen ağ bant genişliği (MB/sn)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0&nbsp;<sup>1</sup> |1 |0,768 |20 |1 |1x500 |2 / 100 |
| Standard_A1 |1 |1,75 |70 |2 |2x500 |2 / 500  |
| Standard_A2 |2 |3,5 |135 |4 |4x500 |2 / 500 |
| Standard_A3 |4 |7 |285 |8 |8x500 |2 / 1000 |
| Standard_A4 |8 |14 |605 |16 |16x500 |4 / 2000 |
| Standard_A5 |2 |14 |135 |4 |4x500 |2 / 500 |
| Standard_A6 |4 |28 |285 |8 |8x500 |2 / 1000 |
| Standard_A7 |8 |56 |605 |16 |16x500 |4 / 2000 |

<sup>1</sup> A0 boyutunun fiziksel donanım üzerindeki abone sayısı planlanandan fazladır. Yalnızca bu boyutta diğer müşteri dağıtımları, çalışan iş yükünüzün performansını etkileyebilir. Göreli performans aşağıda beklenen temel düzey olarak belirtilmiştir ve %15 oranında değişiklik gösterebilir.

<br>

### <a name="a-series---compute-intensive-instances"></a>A Serisi - Yoğun işlem gücü kullanımlı örnekler  

**Yeni boyut önerisi**: [Av2 serisi](../articles/virtual-machines/windows/sizes-general.md#av2-series)

ACU: 225

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

A8-A11 ve H Serisi boyutlar *yoğun işlem gücü kullanımlı örnekler* olarak da bilinir. Bu boyutları çalıştıran donanım; yüksek performanslı bilgi işlem (HPC) kümesi uygulamaları, modellemeler ve simülasyonlar gibi yoğun işlem ve ağ kullanımlı uygulamalar için tasarlanmış ve iyileştirilmiştir. A8-A11 Serisinde, Intel Xeon E5-2670 @ 2,6 GHZ, H Serisinde ise Intel Xeon E5-2667 v3 @ 3,2 GHz işlemciler kullanılmaktadır.  

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (HDD): GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | En fazla NIC|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8&nbsp;<sup>1</sup> |8 |56 |382 |32 |32x500 |2 |
| Standard_A9&nbsp;<sup>1</sup> |16 |112 |382 |64 |64 x 500 |4 |
| Standard_A10 |8 |56 |382 |32 |32x500 |2  |
| Standard_A11 |16 |112 |382 |64 |64 x 500 |4 |

<sup>1</sup>MPI uygulamaları için adanmış RDMA arka uç ağı ultra düşük gecikmeli ve yüksek bant genişliği sağlayan FDR InfiniBand ağı tarafından etkinleştirilir.  

<br>

### <a name="d-series"></a>D Serisi  

**Yeni boyut önerisi**: [Dv3 serisi](../articles/virtual-machines/windows/sizes-general.md#dv3-series-1)

ACU: 160-250 <sup>1</sup>

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

| Boyut         | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / okuma MB/sn / yazma MB/sn | Maksimum veri diski / aktarım hızı: IOPS | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3,5         | 50             | 3000/46/23                                           | 4/4x500                         | 2 / 500                 |
| Standard_D2  | 2         | 7           | 100            | 6000/93/46                                           | 8/8x500                         | 2 / 1000                     |
| Standard_D3  | 4         | 14          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 2000                     |
| Standard_D4  | 8         | 28          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 4000                     |

<sup>1</sup> VM ailesi aşağıdaki CPU'lar birinde çalıştırabilirsiniz: 2.2 GHz Intel Xeon® E5 2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) veya XEON® 2,3 GHz Intel E5-2673 v4 (Broadwell)  

<br>

### <a name="d-series---memory-optimized"></a>D serisi - bellek için iyileştirilmiş  

**Yeni boyut önerisi**: [Dv3 serisi](../articles/virtual-machines/windows/sizes-general.md#dv3-series-1)

ACU: 160-250 <sup>1</sup>

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

| Boyut         | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / okuma MB/sn / yazma MB/sn | Maksimum veri diski / aktarım hızı: IOPS | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6000/93/46                                           | 8/8x500                         | 2 / 1000                     |
| Standard_D12 | 4         | 28          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 2000                     |
| Standard_D13 | 8         | 56          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 4000                     |
| Standard_D14 | 16        | 112         | 800            | 48000/750/375                                        | 64 / 64 x 500                       | 8 / 8000                |

<sup>1</sup> VM ailesi aşağıdaki CPU'lar birinde çalıştırabilirsiniz: 2.2 GHz Intel Xeon® E5 2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) veya XEON® 2,3 GHz Intel E5-2673 v4 (Broadwell)  

<br>

### <a name="ds-series"></a>DS serisi  

**Yeni boyut önerisi**: [DSv3 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv3-series-1)

ACU: 160-250 <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3,5 |7 |4 |4000/32 (43) |3200/32 |2 / 500 |
| Standard_DS2 |2 |7 |14 |8 |8000/64 (86) |6400/64 |2 / 1000 |
| Standard_DS3 |4 |14 |28 |16 |16.000/128 (172) |12.800/128 |4 / 2000 |
| Standard_DS4 |8 |28 |56 |32 |32.000/256 (344) |25.600/256 |8 / 4000 |

<sup>1</sup> VM ailesi aşağıdaki CPU'lar birinde çalıştırabilirsiniz: 2.2 GHz Intel Xeon® E5 2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) veya XEON® 2,3 GHz Intel E5-2673 v4 (Broadwell)  

<br>

### <a name="ds-series---memory-optimized"></a>DS serisi - bellek için iyileştirilmiş  

**Yeni boyut önerisi**: [DSv3 serisi](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general#dsv3-series-1)

ACU: 160-250 <sup>1,2</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |8 |8000/64 (72) |6400/64 |2 / 1000 |
| Standard_DS12 |4 |28 |56 |16 |16.000/128 (144) |12.800/128 |4 / 2000 |
| Standard_DS13 |8 |56 |112 |32 |32.000/256 (288) |25.600/256 |8 / 4000 |
| Standard_DS14 |16 |112 |224 |64 |64.000/512 (576) |51.200/512 |8 / 8000 |

<sup>1</sup> maksimum disk aktarım hızı (IOPS veya MB/sn) DS serisi VM ile sınırlı olabilir sayısı, boyutu ve bölümleme türüyle ekli disklerin.  Ayrıntılar için bkz [yüksek performans için tasarlama](../articles/virtual-machines/windows/premium-storage-performance.md).   
<sup>2</sup> VM ailesi aşağıdaki CPU'lar birinde çalıştırabilirsiniz: 2.2 GHz Intel Xeon® E5 2660 v2, 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) veya XEON® 2,3 GHz Intel E5-2673 v4 (Broadwell)  

<br>
