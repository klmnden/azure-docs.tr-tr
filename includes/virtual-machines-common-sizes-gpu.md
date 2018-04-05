---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 03/30/2018
ms.author: danlep;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 5457ac8bd229889ed2b96354c44066959c00c64f
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
GPU en iyi duruma getirilmiş VM ile tek veya birden çok NVIDIA GPU kullanılabilir özelleştirilmiş sanal makineler boyutlarıdır. Bu boyutlar işlem yoğunluklu, grafik yoğun ve görselleştirme iş yükleri için tasarlanmıştır. Bu makale numarası ve gpu, Vcpu, veri diskleri ve NIC yanı sıra depolama üretilen iş ve ağ bant genişliğini her boyutu Bu gruplandırma türü hakkında bilgi sağlar. 

* **NC, NCv2, NCv3 ve ND** boyutları işlem yoğunluklu ve ağ yoğunluklu uygulamalar ve CUDA ve OpenCL tabanlı uygulamalar ve benzetimleri, AI ve derin öğrenme gibi algoritmaları için iyileştirilmiştir. 
* **NV** boyutları en iyi duruma getirilmiş ve uzak görselleştirme, akış, oyun, kodlama ve OpenGL ve DirectX gibi çerçeveler kullanılarak VDI senaryoları için tasarlanmıştır.  


## <a name="nc-series"></a>NC serisi

NC-serisi VM'ler tarafından desteklenir [NVIDIA Tesla K80](http://images.nvidia.com/content/pdf/kepler/Tesla-K80-BoardSpec-07317-001-v05.pdf) kart. Kullanıcılar enerji araştırması uygulamalar için CUDA yararlanarak daha hızlı verilerine uğraşmaları, benzetimleri kilitlenme, izlenen işleme, derin öğrenme ve daha fazlasını ışın. NC24r yapılandırma, düşük gecikme, sıkı şekilde bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş yüksek verimlilik ağ arabirimi sağlar.


| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6 |6 |56 | 340 | 1 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 64 | 4 |

1 GPU = K80 kartın yarısı.

*RDMA özellikli

## <a name="ncv2-series"></a>NCv2 serisi

NCv2-serisi VM'ler tarafından desteklenir [NVIDIA Tesla P100](http://images.nvidia.com/content/tesla/pdf/nvidia-tesla-p100-datasheet.pdf) GPU. Bu GPU hesaplama performans NC serinin x 2'den fazla sağlayabilir. Müşteriler bu güncelleştirilmiş GPU rezervuarı modelleme, DNA sıralama, protein analiz, Monte Carlo benzetimleri ve diğerleri gibi geleneksel HPC iş yükleri için yararlanabilir. NC24rs v2 yapılandırma, düşük gecikme, sıkı şekilde bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş yüksek verimlilik ağ arabirimi sağlar.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizde vCPU (Temel) kota başlangıçta her bölgede 0 olarak ayarlanır. [VCPU kota artışı isteği](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | ---  |
| Standard_NC6s_v2 |6 |112 | 736 | 1 | 12 | 4 |
| Standard_NC12s_v2 |12 |224 | 1474 | 2 | 24 | 8 |
| Standard_NC24s_v2 |24 |448 | 2948 | 4 | 32 | 8 |
| Standard_NC24rs_v2* |24 |448 | 2948 | 4 | 32 | 8 |

1 GPU = bir P100 kart.

*RDMA özellikli

## <a name="ncv3-series"></a>NCv3 serisi

NCv3-serisi VM'ler tarafından desteklenir [NVIDIA Tesla nı V100](http://www.nvidia.com/content/PDF/Volta-Datasheet.pdf) GPU. Bu GPU hesaplama performans NCv2 serinin x 1,5 sağlayabilir. Müşteriler bu güncelleştirilmiş GPU rezervuarı modelleme, DNA sıralama, protein analiz, Monte Carlo benzetimleri ve diğerleri gibi geleneksel HPC iş yükleri için yararlanabilir. NC24rs v3 yapılandırma, düşük gecikme, sıkı şekilde bağlı paralel bilgi işlem iş yükleri için en iyi duruma getirilmiş yüksek verimlilik ağ arabirimi sağlar.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizde vCPU (Temel) kota başlangıçta her bölgede 0 olarak ayarlanır. [VCPU kota artışı isteği](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 |6 |112 | 736 | 1 | 12 | 4 |
| Standard_NC12s_v3 |12 |224 | 1474 | 2 | 24 | 8 |
| Standard_NC24s_v3 |24 |448 | 2948 | 4 | 32 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 32 | 8 |

1 GPU = bir nı V100 kart.

*RDMA özellikli

## <a name="nd-series"></a>ND serisi

ND-serisi sanal makine AI ve derin öğrenme iş yükleri için tasarlanmış GPU ailesi için yeni bir toplama yok. Bunlar, eğitim ve çıkarım için mükemmel performans sunar. Tarafından desteklenir ve örnekleri [NVIDIA Tesla P40](http://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU. Bu örnekler tek duyarlıklı kayan nokta işlemleri için Microsoft Bilişsel araç seti, TensorFlow, Caffe ve diğer çerçeveler kullanılarak AI iş yükleri için mükemmel performans sağlar. Ayrıca, ND serisi çok daha büyük bir GPU bellek boyutu (24 GB) sunarak daha büyük sinir ağı modellerinin sığdırılmasına imkan tanır. NC serisi gibi ND-ikincil düşük gecikmeli, yüksek verimlilik ağ RDMA üzerinden bir yapılandırmayla sunar ve büyük ölçekli eğitim işleri birçok GPU kapsayıcı çalıştırabilmeniz için InfiniBand bağlantı.

> [!IMPORTANT]
> Bu boyut ailesi için aboneliğinizi bölgede başına vCPU (Temel) kota başlangıçta 0 olarak ayarlanır. [VCPU kota artışı isteği](../articles/azure-supportability/resource-manager-core-quotas-request.md) bu ailesi için bir [kullanılabilir bölge](https://azure.microsoft.com/regions/services/).
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s |6 |112 | 736 | 1 | 12 | 4 |
| Standard_ND12s |12 |224 | 1474 | 2 | 24 | 8 | 
| Standard_ND24s |24 |448 | 2948 | 4 | 32 | 8 |
| Standard_ND24rs * |24 |448 | 2948 | 4 | 32 | 8 |

1 GPU = bir P40 kart.

*RDMA özellikli

## <a name="nv-series"></a>NV serisi

NV-serisi sanal makineler tarafından sağlanmıştır [NVIDIA Tesla M60](http://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU ve NVIDIA kılavuz Masaüstü teknolojinin hızlandırılmış uygulamalar ve sanal masaüstlerini müşteriler, veri veya benzetimleri görselleştirmek mümkün olduğu. Kullanıcılar kendi grafik yoğun iş akışları üstün grafik yeteneği almak ve ayrıca kodlama ve işleme gibi tek duyarlıklı iş yüklerini çalıştırmak için NV örneklerinde görselleştirmek kullanabilirsiniz. 

Her GPU NV durumlarda bir kılavuz lisansı ile birlikte gelir. Bu lisans NV örneği tek bir kullanıcı için sanal bir iş istasyonu olarak kullanmak için esneklik sunar veya 25 eşzamanlı kullanıcı VM için bir sanal uygulama senaryo bağlanabilir.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | Maksimum veri diskleri | En fazla NIC | Sanal iş istasyonları | Sanal uygulamalar | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |340 | 1 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 64 | 4 | 4 | 100 |

1 GPU = M60 kartın yarısı.

 
