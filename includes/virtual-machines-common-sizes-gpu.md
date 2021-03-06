---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 06/11/2019
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: b7bc040ae799aec98454fb227bbeeb6027f9615a
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673553"
---
GPU için iyileştirilmiş sanal makine boyutları olan özel sanal makineler tek veya birden çok NVIDIA GPU'ları ile kullanılabilir. Bu boyutları görselleştirme yoğun işlem gücü kullanımlı ve grafik kullanımlı iş yükleri için tasarlanmıştır. Bu makalede sayısı ve gpu'ları, Vcpu, veri diskleri ve NIC türü hakkında bilgi sağlar. Depolama aktarım hızı ve ağ bant genişliği de bu gruplandırmaki her boyut için dahil edilir.

* **NC, NCv2 NCv3** boyutları, yoğun işlem ve ağ kullanımlı uygulamalar ve algoritmalar için iyileştirilmiştir. CUDA ve OpenCL tabanlı uygulama ve simülasyonlar, yapay ZEKA ve derin öğrenme örnek verilebilir. NCv3 serisi, NVIDIA Tesla V100 GPU özelliklerine sahip yüksek performanslı bilgi işlem iş yükleri üzerinde odaklanmıştır. NC serisi kullanan Intel Xeon E5-2690 v3 2.60 GHz v3 (Haswell) işlemciyi ve NCv2 serisi ve NCv3 serisi VM'ler kullanan Intel Xeon E5-2690 v4 (Broadwell) işlemciyi.

* **ND ve NDv2** ND serisi eğitim ve çıkarım senaryoları için ayrıntılı öğrenme odaklanmıştır. NVIDIA Tesla P40 GPU ve Intel Xeon E5-2690 v4 kullanır (Broadwell) işlemciyi. NDv2 serisi, Intel Xeon Platinum 8168 (Skylake) işlemciyi kullanır.

* **NV ve NVv3** boyutları en iyi duruma getirilmiş ve Uzaktan görselleştirme, akışı, oyun, kodlama ve OpenGL ve DirectX gibi çerçeveleri kullanan VDI senaryoları için tasarlanmıştır.  Bu sanal makineler, NVIDIA Tesla M60 GPU tarafından desteklenir.

## <a name="nc-series"></a>NC serisi

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

NC serisi VM'ler ile desteklenen [NVIDIA Tesla K80](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/tesla-product-literature/Tesla-K80-BoardSpec-07317-001-v05.pdf) kartı ve Intel Xeon E5-2690 v3 (Haswell) işlemci. Kullanıcılar verileri daha hızlı enerji keşif uygulamaları CUDA yararlanarak işleyin, benzetimleri kilitlenme, izlenen işleme, derin öğrenme ve diğer ışın. Düşük gecikme süreli, yüksek performanslı ağ arabirimi sıkı bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş NC24r yapılandırması sunar.

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | ---- |
| Standard_NC6 |6 |56 | 340 | 1\. | 12 | 24 | 1\. |
| Standard_NC12 |12 |112 | 680 | 2 | 24 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 48 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 48 | 64 | 4 |

1 GPU = K80 kartın yarısı.

*RDMA özellikli

## <a name="ncv2-series"></a>NCv2 serisi

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

NCv2 serisi VM'ler ile desteklenen [NVIDIA Tesla P100](https://www.nvidia.com/en-us/data-center/tesla-p100/) GPU'ları. Bu GPU'ları NC serisi işlem performansı x 2'den sağlayabilir. Müşteriler, modellemesi, DNA sıralama, protein analizi, Monte Carlo simülasyonları ve diğerleri gibi geleneksel HPC iş yüklerinde bu güncelleştirilmiş GPU avantajlarından yararlanabilirsiniz. GPU'ları yanı sıra, NCv2 serisi VM'ler de Intel Xeon E5-2690 v4 tarafından desteklenen (Broadwell) CPU.

Düşük gecikme süreli, yüksek performanslı ağ arabirimi sıkı bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş NC24rs v2 yapılandırmasını sağlar.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizdeki vCPU (çekirdek) kotasını başlangıçta her bölgede 0 olarak ayarlanır. [VCPU kota artışı isteğinde](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | En fazla NIC |
| --- | --- | --- | --- | --- | --- | ---  | ---| --- |
| Standard_NC6s_v2 | 6 |112 | 736 | 1\. | 16 | 12 | 20000/ 200 | 4 |
| Standard_NC12s_v2 | 12 |224 | 1474 | 2 | 32 | 24 | 40000 / 400 | 8 |
| Standard_NC24s_v2 | 24 |448 | 2948 | 4 | 64 | 32 | 80000 / 800 | 8 |
| Standard_NC24rs_v2* | 24 |448 | 2948 | 4 | 64 | 32 | 80000 / 800 | 8 |

1 GPU = bir P100 kart.

*RDMA özellikli

## <a name="ncv3-series"></a>NCv3 serisi

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

NCv3 serisi VM'ler ile desteklenen [NVIDIA Tesla V100](https://www.nvidia.com/en-us/data-center/tesla-v100/) GPU'ları. Bu GPU'ları 1,5 kat NCv2 serisi işlem performansı sağlayabilir. Müşteriler, modellemesi, DNA sıralama, protein analizi, Monte Carlo simülasyonları ve diğerleri gibi geleneksel HPC iş yüklerinde bu güncelleştirilmiş GPU avantajlarından yararlanabilirsiniz. Düşük gecikme süreli, yüksek performanslı ağ arabirimi sıkı bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş NC24rs v3 yapılandırmasını sağlar. GPU'ları yanı sıra, NCv3 serisi VM'ler de Intel Xeon E5-2690 v4 tarafından desteklenen (Broadwell) CPU.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizdeki vCPU (çekirdek) kotasını başlangıçta her bölgede 0 olarak ayarlanır. [VCPU kota artışı isteğinde](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 | 6 |112 | 736 | 1\. | 16 | 12 | 20000 / 200 | 4 |
| Standard_NC12s_v3 | 12 |224 | 1474 | 2 | 32 | 24 | 40000 / 400 | 8 |
| Standard_NC24s_v3 | 24 |448 | 2948 | 4 | 64 | 32 | 80000 / 800 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 80000 / 800 | 8 |

1 GPU = bir V100 kart.

*RDMA özellikli

## <a name="ndv2-series-preview"></a>NDv2 serisi (Önizleme)

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

Infiniband: Desteklenmiyor

HPC, yapay ZEKA ve makine öğrenimi iş yükleri için tasarlanan GPU ailesine yeni eklenen NDv2 serisi sanal makinesidir. 8 tarafından desteklenir ve gpu'ları ve 40 Intel Xeon Platinum 8168 (Skylake) çekirdek sistem belleği 672 GiB NVIDIA Tesla V100 NVLINK birbirine bağlı. NDv2 örneği Cuda, TensorFlow, Pytorch, Caffe ve diğer çerçeveleri kullanarak HPC ve AI iş yükleri için mükemmel FP32 ve FP64 performansı sağlar.

[Kaydolma ve önizleme süresince bu makineler erişin](https://aka.ms/ndv2signup).
<br>

| Size | Sanal işlemci | GPU | Bellek | NIC'ler (Maks.) | Geçici depolama (SSD) GiB | En çok, Veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | En fazla ağ bant genişliği | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND40s_v2 | 40 | 8 V100 (NVLink) | 672 GiB | 8 | 2948 | 32 | 80000 / 800 | 24000 MB/sn |

## <a name="nd-series"></a>ND serisi

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

ND serisi sanal makineler, GPU ailesine yeni eklenen iş yükleri yapay ZEKA ve derin öğrenme için tasarlanmış olan. Bunlar, eğitim ve çıkarım için mükemmel performans sunar. ND örnekleri ile desteklenen [NVIDIA Tesla P40](https://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU'ları ve Intel Xeon E5-2690 v4 (Broadwell) CPU. Bu örnekler tek duyarlıklı kayan nokta işlemleri, Microsoft Bilişsel araç seti, TensorFlow, Caffe ve diğer çerçeveleri kullanan AI iş yükleri için mükemmel bir performans sağlar. Ayrıca, ND serisi çok daha büyük bir GPU bellek boyutu (24 GB) sunarak daha büyük sinir ağı modellerinin sığdırılmasına imkan tanır. NC serisinde olduğu gibi ND serisi RDMA aracılığıyla ikincil bir düşük gecikme süreli, yüksek performanslı ağ yapılandırmasıyla sunar ve Infiniband bağlantısı birçok GPU'yu kapsayan büyük ölçekli eğitim işleri çalıştırabilirsiniz.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizi bölge başına vCPU (çekirdek) kotasını başlangıçta 0 olarak ayarlanır. [VCPU kota artışı isteğinde](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s | 6 |112 | 736 | 1\. | 24 | 12 | 20000 / 200 | 4 |
| Standard_ND12s | 12 |224 | 1474 | 2 | 48 | 24 | 40000 / 400 | 8 | 
| Standard_ND24s | 24 |448 | 2948 | 4 | 96 | 32 | 80000 / 800 | 8 |
| Standard_ND24rs * | 24 |448 | 2948 | 4 | 96 | 32 | 80000 / 800 | 8 |

1 GPU = bir P40 kart.

*RDMA özellikli

## <a name="nv-series"></a>NV serisi

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleğe alma:  Desteklenmiyor

NV serisi sanal makineler tarafından desteklenen [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU'ları ve NVIDIA GRID teknoloji Masaüstü için hızlandırılmış uygulamaları ve sanal masaüstü müşterilerin verilerini veya simülasyonlarını görselleştirmek mümkün olduğu. Kullanıcılar üst grafik özelliğine sahip olursunuz ve buna ek olarak kodlama ve işleme gibi tek duyarlıklı iş yüklerini çalıştırmak için NV örnekleri üzerinde grafik açısından yoğun kaynak gerektiren iş akışlarını görselleştirin olanağına sahip olursunuz. NV serisi VM'ler de Intel Xeon E5-2690 v3 tarafından desteklenen (Haswell) CPU.

NV örnekleri, her bir GPU kılavuz lisansı ile birlikte gelir. Bu lisans size NV örneği, tek bir kullanıcı için sanal bir iş istasyonu olarak kullanmak için esneklik veya 25 eş zamanlı kullanıcı bir sanal uygulama senaryosu için VM'ye bağlanabilirsiniz.

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | En fazla NIC | Sanal çalışma İstasyonlarınızı | Sanal uygulamalar |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |340 | 1\. | 8 | 24 | 1\. | 1\. | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 16 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = M60 kartın yarısı.

## <a name="nvv3-series-preview-sup1sup"></a>NVv3 serisi (Önizleme) <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleğe alma:  Desteklenen

NVv3 serisi sanal makineler tarafından desteklenen [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU'ları ve NVIDIA GRID teknolojisi ile Intel E5-2690 v4 (Broadwell) CPU. Bu sanal makineler GPU hızlandırılmış grafik uygulamalar ve sanal masaüstlerini müşteriler istediğiniz yere verilerini görselleştirmek için hedeflenen, sonuçları görüntülemek için CAD veya işleme ve akış içeriği üzerinde çalışan benzetimi. Ayrıca bu sanal makineler kodlama ve işleme gibi tek hassas iş yüklerini de çalıştırabilir. NVv3 sanal makine, Premium depolamayı destekler ve kendi öncellerini NV serisi ile karşılaştırıldığında iki kez sistem belleği (RAM) gelir.  

Her GPU NVv3 durumlarda bir kılavuz lisansı ile birlikte gelir. Bu lisans size NV örneği, tek bir kullanıcı için sanal bir iş istasyonu olarak kullanmak için esneklik veya 25 eş zamanlı kullanıcı bir sanal uygulama senaryosu için VM'ye bağlanabilirsiniz.

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS / MB/sn | En fazla NIC | Sanal çalışma İstasyonlarınızı | Sanal uygulamalar | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV12s_v3 |12 |112 |320 | 1\. | 8 | 12 | 20000 / 200 | 4 | 1\. | 25 |
| Standard_NV24s_v3 |24 |224 |640 | 2 | 16 | 24 | 40000 / 400 | 8 | 2 | 50 |
| Standard_NV48s_v3 |48 |448 |1280 | 4 | 32 | 32 | 80000 / 800 | 8 | 4 | 100 |

1 GPU = M60 kartın yarısı.

<sup>1</sup> NVv3 serisi VM'ler, Intel Hyper-Threading teknolojisine özelliği
