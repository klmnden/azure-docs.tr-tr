---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 10/23/2018
ms.author: danlep;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 4fde34338d5606a1f431ff4b7f7074d9cd472e90
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "50035179"
---
GPU için iyileştirilmiş sanal makine boyutları olan özel sanal makineler tek veya birden çok NVIDIA GPU'ları ile kullanılabilir. Bu boyutları görselleştirme yoğun işlem gücü kullanımlı ve grafik kullanımlı iş yükleri için tasarlanmıştır. Bu makalede sayısı ve gpu'ları, Vcpu, veri diskleri ve NIC türü hakkında bilgi sağlar. Depolama aktarım hızı ve ağ bant genişliği de bu gruplandırmaki her boyut için dahil edilir. 

* **NC, NCv2 NCv3 ve ND** boyutları, yoğun işlem ve ağ kullanımlı uygulamalar ve algoritmalar için iyileştirilmiştir. CUDA ve OpenCL tabanlı uygulama ve simülasyonlar, yapay ZEKA ve derin öğrenme örnek verilebilir. NCv3 serisi, NVIDIA Tesla V100 GPU özelliklerine sahip yüksek performanslı bilgi işlem iş yükleri üzerinde odaklanmıştır.  ND serisi, derin öğrenme için eğitim ve çıkarım senaryolarına odaklanır. Bu seri, NVIDIA Tesla P40 GPU’yu kullanır.
* **NV ve NVv2** boyutları en iyi duruma getirilmiş ve Uzaktan görselleştirme, akışı, oyun, kodlama ve OpenGL ve DirectX gibi çerçeveleri kullanan VDI senaryoları için tasarlanmıştır.  Bu sanal makineler, NVIDIA Tesla M60 GPU tarafından desteklenir.


## <a name="nc-series"></a>NC serisi

Premium Depolama: Desteklenmez

Premium depolama önbelleğe alma: Desteklenmez

NC serisi VM'ler ile desteklenen [NVIDIA Tesla K80](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) kart. Kullanıcılar verileri daha hızlı enerji keşif uygulamaları CUDA yararlanarak işleyin, benzetimleri kilitlenme, izlenen işleme, derin öğrenme ve daha fazlasını ışın. Düşük gecikme süreli, yüksek performanslı ağ arabirimi sıkı bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş NC24r yapılandırması sunar.


| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | ---- |
| Standard_NC6 |6 |56 | 340 | 1 | 8 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 16 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 32 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 32 | 64 | 4 |

1 GPU = K80 kartın yarısı.

*RDMA özellikli

## <a name="ncv2-series"></a>NCv2 serisi

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: desteklenir

NCv2 serisi VM'ler ile desteklenen [NVIDIA Tesla P100](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) GPU'ları. Bu GPU'ları NC serisi işlem performansı x 2'den sağlayabilir. Müşteriler, modellemesi, DNA sıralama, protein analizi, Monte Carlo simülasyonları ve diğerleri gibi geleneksel HPC iş yüklerinde bu güncelleştirilmiş GPU avantajlarından yararlanabilirsiniz. Düşük gecikme süreli, yüksek performanslı ağ arabirimi sıkı bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş NC24rs v2 yapılandırmasını sağlar.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizdeki vCPU (çekirdek) kotasını başlangıçta her bölgede 0 olarak ayarlanır. [VCPU kota artışı isteğinde](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | ---  | --- |
| Standard_NC6s_v2 |6 |112 | 736 | 1 | 16 | 12 | 4 |
| Standard_NC12s_v2 |12 |224 | 1474 | 2 | 32 | 24 | 8 |
| Standard_NC24s_v2 |24 |448 | 2948 | 4 | 64 | 32 | 8 |
| Standard_NC24rs_v2* |24 |448 | 2948 | 4 | 64 | 32 | 8 |

1 GPU = bir P100 kart.

*RDMA özellikli

## <a name="ncv3-series"></a>NCv3 serisi

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: desteklenir

NCv3 serisi VM'ler ile desteklenen [NVIDIA Tesla V100](http://www.nvidia.com/content/PDF/Volta-Datasheet.pdf) GPU'ları. Bu GPU'ları 1,5 kat NCv2 serisi işlem performansı sağlayabilir. Müşteriler, modellemesi, DNA sıralama, protein analizi, Monte Carlo simülasyonları ve diğerleri gibi geleneksel HPC iş yüklerinde bu güncelleştirilmiş GPU avantajlarından yararlanabilirsiniz. Düşük gecikme süreli, yüksek performanslı ağ arabirimi sıkı bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş NC24rs v3 yapılandırmasını sağlar.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizdeki vCPU (çekirdek) kotasını başlangıçta her bölgede 0 olarak ayarlanır. [VCPU kota artışı isteğinde](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 |6 |112 | 736 | 1 | 16 | 12 | 4 |
| Standard_NC12s_v3 |12 |224 | 1474 | 2 | 32 | 24 | 8 |
| Standard_NC24s_v3 |24 |448 | 2948 | 4 | 64 | 32 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 8 |

1 GPU = bir V100 kart.

*RDMA özellikli

## <a name="nd-series"></a>ND serisi

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: desteklenir

ND serisi sanal makineler, yapay ZEKA ve derin öğrenme iş yükleri için tasarlandı GPU ailesine yeni eklenen olur. Bunlar, eğitim ve çıkarım için mükemmel performans sunar. ND örnekleri ile desteklenen [NVIDIA Tesla P40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU'ları. Bu örnekler tek duyarlıklı kayan nokta işlemleri, Microsoft Bilişsel araç seti, TensorFlow, Caffe ve diğer çerçeveleri kullanan AI iş yükleri için mükemmel bir performans sağlar. Ayrıca, ND serisi çok daha büyük bir GPU bellek boyutu (24 GB) sunarak daha büyük sinir ağı modellerinin sığdırılmasına imkan tanır. NC serisinde olduğu gibi ND serisi RDMA aracılığıyla ikincil bir düşük gecikme süreli, yüksek performanslı ağ yapılandırmasıyla sunar ve Infiniband bağlantısı birçok GPU'yu kapsayan büyük ölçekli eğitim işleri çalıştırabilirsiniz.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizi bölge başına vCPU (çekirdek) kotasını başlangıçta 0 olarak ayarlanır. [VCPU kota artışı isteğinde](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s |6 |112 | 736 | 1 | 24 | 12 | 4 |
| Standard_ND12s |12 |224 | 1474 | 2 | 48 | 24 | 8 | 
| Standard_ND24s |24 |448 | 2948 | 4 | 96 | 32 | 8 |
| Standard_ND24rs * |24 |448 | 2948 | 4 | 96 | 32 | 8 |

1 GPU = bir P40 kart.

*RDMA özellikli

## <a name="nv-series"></a>NV serisi

Premium Depolama: Desteklenmez

Premium depolama önbelleğe alma: Desteklenmez

NV serisi sanal makineler tarafından desteklenen [NVIDIA Tesla M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU'ları ve NVIDIA GRID teknoloji Masaüstü için hızlandırılmış uygulamaları ve sanal masaüstü müşterilerin verilerini veya simülasyonlarını görselleştirmek mümkün olduğu. Kullanıcılar üst grafik özelliğine sahip olursunuz ve buna ek olarak kodlama ve işleme gibi tek duyarlıklı iş yüklerini çalıştırmak için NV örnekleri üzerinde grafik açısından yoğun kaynak gerektiren iş akışlarını görselleştirin olanağına sahip olursunuz. 

NV örnekleri, her bir GPU kılavuz lisansı ile birlikte gelir. Bu lisans size NV örneği, tek bir kullanıcı için sanal bir iş istasyonu olarak kullanmak için esneklik veya 25 eş zamanlı kullanıcı bir sanal uygulama senaryosu için VM'ye bağlanabilirsiniz.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | En fazla NIC | Sanal çalışma İstasyonlarınızı | Sanal uygulamalar | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Standard_NV6 |6 |56 |340 | 1 | 8 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 16 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = M60 kartın yarısı.

## <a name="nvv2-series-preview"></a>NVv2 serisi (Önizleme)

Premium Depolama: desteklenir

Premium depolama önbelleğe alma: desteklenir

NVv2 serisi sanal makineler tarafından desteklenen [NVIDIA Tesla M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU'ları ve NVIDIA GRID Intel Broadwell CPU teknolojisi. Bu sanal makineler GPU hızlandırılmış grafik uygulamalar ve sanal masaüstlerini müşteriler istediğiniz yere verilerini görselleştirmek için hedeflenen, sonuçları görüntülemek için CAD veya işleme ve akış içeriği üzerinde çalışan benzetimi. Ayrıca bu sanal makineler kodlama ve işleme gibi tek hassas iş yüklerini de çalıştırabilir. NVv2 sanal makine, Premium depolamayı destekler ve kendi öncellerini NV serisi ile karşılaştırıldığında iki kez sistem belleği (RAM) gelir.  

Her GPU NVv2 durumlarda bir kılavuz lisansı ile birlikte gelir. Bu lisans size NV örneği, tek bir kullanıcı için sanal bir iş istasyonu olarak kullanmak için esneklik veya 25 eş zamanlı kullanıcı bir sanal uygulama senaryosu için VM'ye bağlanabilirsiniz.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | En fazla NIC | Sanal çalışma İstasyonlarınızı | Sanal uygulamalar | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6s_v2 |6 |112 |320 | 1 | 8 | 12 | 4 | 1 | 25 |
| Standard_NV12s_v2 |12 |224 |640 | 2 | 16 | 24 | 8 | 2 | 50 |
| Standard_NV24s_v2 |24 |448 |1280 | 4 | 32 | 32 | 8 | 4 | 100 |

1 GPU = M60 kartın yarısı.

 
