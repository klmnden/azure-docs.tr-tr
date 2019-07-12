---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 05/16/2019
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: bb945695e0525876e044117e26c239e21d66473f
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673552"
---
Bu bölümde, önceki nesil sanal makine boyutları hakkında bilgi sağlar. Bu boyutları hala kullanılabilir, ancak yeni nesli vardır. 

## <a name="f-series"></a>F Serisi

F Serisi, Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan saat hızlarına ulaşabilen 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemciyi temel alır. Bu değer Dv2 Serisi VM'lerdeki CPU performansıyla aynıdır.  

F Serisi VM'ler, daha hızlı CPU'ya ihtiyaç duyan ancak çok fazla belleğe veya vCPU başına geçici depolamaya ihtiyaç duymayan iş yükleri için harika bir seçenektir.  Analitikler, oyun sunucuları, web sunucuları ve toplu işleme gibi iş yükleri, F Serisinin özelliklerinden en fazla yarar sağlayacak iş yükleridir.

ACU: 210 - 250

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

| Size         | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum geçici depolama aktarım hızı: IOPS / okuma MB/sn / yazma MB/sn | Maksimum veri diski / aktarım hızı: IOPS | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1\.         | 2           | 16             | 3000/46/23                                           | 4/4x500                         | 2 / 750                 |
| Standard_F2  | 2         | 4           | 32             | 6000/93/46                                           | 8/8x500                         | 2 / 1500                     |
| Standard_F4  | 4         | 8           | 64             | 12000/187/93                                         | 16/16x500                         | 4 / 3000                     |
| Standard_F8  | 8         | 16          | 128            | 24000/375/187                                        | 32/32x500                       | 8 / 6000                     |
| Standard_F16 | 16        | 32          | 256            | 48000/750/375                                        | 64 / 64 x 500                       | 8 / 12000           |

## <a name="fs-series-sup1sup"></a>FS serisi <sup>1</sup>

Fs serisi, Premium depolamaya ek olarak F serisinin tüm avantajlarını sağlar.

ACU: 210 - 250

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS / sn (önbellek boyutu gib biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | Maks NIC / beklenen ağ bant genişliği (MB/sn) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1\. |2 |4 |4 |4000 / 32 (12) |3200 / 48 |2 / 750 |
| Standard_F2s |2 |4 |8 |8 |8000 / 64 (24) |6400 / 96 |2 / 1500 |
| Standard_F4s |4 |8 |16 |16 |16000 / 128 (48) |12800 / 192 |4 / 3000 |
| Standard_F8s |8 |16 |32 |32 |32000 / 256 (96) |25600 / 384 |8 / 6000 |
| Standard_F16s |16 |32 |64 |64 |64000 / 512 (192) |51200 / 768 |8 / 12000 |

MB/sn = 10^6 bayt/saniye ve GiB = 1024^3 bayt.

<sup>1</sup> maksimum disk aktarım hızı (IOPS veya MB/sn) Fs serisi VM ile sınırlı olabilir sayısı, boyutu ve bölümleme türüyle ekli disklerin.  Ayrıntılar için bkz [yüksek performans için tasarlama](../articles/virtual-machines/windows/premium-storage-performance.md).  

## <a name="ls-series"></a>Ls serisi

Ls serisi, [Intel® Xeon İşlemci E5 v3 ailesi](https://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html) ile 32’ye kadar vCPU kullanım olanağı sunar. Ls serisi, G/GS serisi ile aynı CPU performansı sunar ve her vCPU başına 8 GiB bellek içerir.

Ls serisi, kalıcı veri diskleri tarafından ulaşılabilir IOPS artırmak için yerel bir önbellek oluşturulmasını desteklemiyor. Yüksek aktarım hızı ve yerel disk IOPS sağlar Ls serisi VM'ler Apache Cassandra ve MongoDB gibi tek bir VM bir arıza olması durumunda kalıcılığı sağlamak için birden çok VM arasında veri çoğaltmak NoSQL depoları için idealdir.

ACU: 180-240

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenmiyor
 
| Size          | Sanal işlemci | Bellek (GiB) | Geçici depolama alanı (GiB) | Maksimum veri diskleri | Maksimum geçici depolama aktarım hızı (IOPS / MB/sn) | Maksimum önbelleğe alınmamış disk aktarım hızı (IOPS / MB/sn) | Maks NIC / beklenen ağ bant genişliği (MB/sn) | 
|----------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standart_L4s   | 4  | 32  | 678   | 16 | 20000 / 200 | 5000 / 125  | 2 / 4000  | 
| Standart_L8s   | 8  | 64  | 1388 | 32 | 40000 / 400 | 10000 / 250 | 4 / 8000  | 
| Standart_L16s  | 16 | 128 | 2807 | 64 | 80000 / 800 | 20000 / 500 | 8 / 16000 | 
| Standard_L32s&nbsp;<sup>1</sup> | 32   | 256  | 5630 | 64   | 160000 / 1600   | 40000 / 1000     | 8 / 20000 | 

Ls serisi VM ile maksimum disk aktarım hızı olabilir bağlı diskleri sayısı, boyutu ve bölümleme türüyle herhangi sınırlıdır. Ayrıntılar için bkz [yüksek performans için tasarlama](../articles/virtual-machines/windows/premium-storage-performance.md).

<sup>1</sup> örneği, tek bir müşteriye özel donanımla yalıtılır.

## <a name="nvv2-series-preview"></a>NVv2 serisi (Önizleme)

**Yeni boyut önerisi**: [NVv3 serisi (Önizleme)](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu#nvv3-series-preview-1)

NVv2 serisi sanal makineler tarafından desteklenen [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU'ları ve NVIDIA GRID Intel Broadwell CPU teknolojisi. Bu sanal makineler GPU hızlandırılmış grafik uygulamalar ve sanal masaüstlerini müşteriler istediğiniz yere verilerini görselleştirmek için hedeflenen, sonuçları görüntülemek için CAD veya işleme ve akış içeriği üzerinde çalışan benzetimi. Ayrıca bu sanal makineler kodlama ve işleme gibi tek hassas iş yüklerini de çalıştırabilir. NVv2 sanal makine, Premium depolamayı destekler ve kendi öncellerini NV serisi ile karşılaştırıldığında iki kez sistem belleği (RAM) gelir.  

Her GPU NVv2 durumlarda bir kılavuz lisansı ile birlikte gelir. Bu lisans size NV örneği, tek bir kullanıcı için sanal bir iş istasyonu olarak kullanmak için esneklik veya 25 eş zamanlı kullanıcı bir sanal uygulama senaryosu için VM'ye bağlanabilirsiniz.

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | En fazla NIC | Sanal çalışma İstasyonlarınızı | Sanal uygulamalar | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6s_v2 |6 |112 |320 | 1 | 8 | 12 | 4 | 1\. | 25 |
| Standard_NV12s_v2 |12 |224 |640 | 2 | 16 | 24 | 8 | 2 | 50 |
| Standard_NV24s_v2 |24 |448 |1280 | 4 | 32 | 32 | 8 | 4 | 100 |

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard A0 - A4, CLI ve PowerShell kullanarak

Klasik dağıtım modelinde bazı VM boyutu adları CLI ve PowerShell'dekilerden biraz farklıdır:

* Standard_A0 = Çok Küçük
* Standard_A1 = Küçük
* Standard_A2 = Orta
* Standard_A3 = Büyük
* Standard_A4 = Çok Büyük
