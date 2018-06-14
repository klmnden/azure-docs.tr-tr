---
title: Kıyaslama puanları Azure Windows VM'ler için işlem | Microsoft Docs
description: SPECint işlem Kıyaslama puanları Azure Windows Server çalıştıran VM'ler için karşılaştırın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 69ae72ec-e8be-4e46-a8f0-e744aebb5cc2
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/09/2018
ms.author: cynthn;davberg
ms.openlocfilehash: 2831f177bbcd02be02124812de144bd7a6867aa4
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31424713"
---
# <a name="compute-benchmark-scores-for-windows-vms"></a>Windows VM'ler için Kıyaslama puanları işlem
Aşağıdaki SPECInt Kıyaslama puanlar Windows Server çalıştıran Azure'nın yüksek performans VM serisi için işlem performansı gösterir. İşlem Kıyaslama puanları için kullanılabilir de [Linux VM'ler](../linux/compute-benchmark-scores.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE]
> Linux numaraları en son güncelleştirilen ve VM'ler daha kapsamlı bir kümesini içerir.

## <a name="a-series---compute-intensive"></a>A-series-işlem yoğunluklu
| Boyut | vCPU sayısı | NUMA düğümleri | CPU | Çalıştırmalar | Ortalama temel oranı | StdDev |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8 |8 |1 |Intel Xeon CPU E5-2670 0 @ 2.6 GHz |10 |236.1 |1.1 |
| Standard_A9 |16 |2 |Intel Xeon CPU E5-2670 0 @ 2.6 GHz |10 |450.3 |7.0 |
| Standard_A10 |8 |1 |Intel Xeon CPU E5-2670 0 @ 2.6 GHz |5 |235.6 |0.9 |
| Standard_A11 |16 |2 |Intel Xeon CPU E5-2670 0 @ 2.6 GHz |7 |454.7 |4.8 |

## <a name="dv2-series"></a>Dv2 Serisi
| Boyut | vCPU sayısı | NUMA düğümleri | CPU | Çalıştırmalar | Ortalama temel oranı | StdDev |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D1_v2 |1 |1 |2.4 GHz @ Intel Xeon E5-2673 v3 |83 |36.6 |2.6 |
| Standard_D2_v2 |2 |1 |2.4 GHz @ Intel Xeon E5-2673 v3 |27 |70.0 |3.7 |
| Standard_D3_v2 |4 |1 |2.4 GHz @ Intel Xeon E5-2673 v3 |19 |130.5 |4.4 |
| Standard_D4_v2 |8 |1 |2.4 GHz @ Intel Xeon E5-2673 v3 |19 |238.1 |5.2 |
| Standard_D5_v2 |16 |2 |2.4 GHz @ Intel Xeon E5-2673 v3 |14 |460.9 |15.4 |
| Standard_D11_v2 |2 |1 |2.4 GHz @ Intel Xeon E5-2673 v3 |19 |70.1 |3.7 |
| Standard_D12_v2 |4 |1 |2.4 GHz @ Intel Xeon E5-2673 v3 |2 |132.0 |1.4 |
| Standard_D13_v2 |8 |1 |2.4 GHz @ Intel Xeon E5-2673 v3 |17 |235.8 |3.8 |
| Standard_D14_v2 |16 |2 |2.4 GHz @ Intel Xeon E5-2673 v3 |15 |460.8 |6.5 |

## <a name="g-series-gs-series"></a>G-serisi, GS serisi
| Boyut | vCPU sayısı | NUMA düğümleri | CPU | Çalıştırmalar | Ortalama temel oranı | StdDev |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_G1, Standard_GS1 |2 |1 |Intel Xeon E5-2698B v3 @ 2 GHz |31 |71.8 |6.5 |
| Standard_G2, Standard_GS2 |4 |1 |Intel Xeon E5-2698B v3 @ 2 GHz |5 |133.4 |13.0 |
| Standart G3, Standard_GS3 |8 |1 |Intel Xeon E5-2698B v3 @ 2 GHz |6 |242.3 |6.0 |
| Standard_G4, Standard_GS4 |16 |1 |Intel Xeon E5-2698B v3 @ 2 GHz |15 |398.9 |6.0 |
| Standard_G5, Standard_GS5 |32 |2 |Intel Xeon E5-2698B v3 @ 2 GHz |22 |762.8 |3.7 |

## <a name="h-series"></a>H Serisi
| Boyut | vCPU sayısı | NUMA düğümleri | CPU | Çalıştırmalar | Ortalama temel oranı  | StdDev |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |1 |Intel Xeon E5-2667 v3 3,2 GHz @ |5 |297.4 |0.9 |
| Standard_H16 |16 |2 |Intel Xeon E5-2667 v3 3,2 GHz @ |5 |575.8 |6.8 |
| Standard_H8m |8 |1 |Intel Xeon E5-2667 v3 3,2 GHz @ |5 |297.0 |1.2 |
| Standard_H16m |16 |2 |Intel Xeon E5-2667 v3 3,2 GHz @ |5 |572.2 |3.9 |
| Standart_h16r |16 |2 |Intel Xeon E5-2667 v3 3,2 GHz @ |5 |573.2 |2.9 |
| Standart_h16mr |16 |2 |Intel Xeon E5-2667 v3 3,2 GHz @ |7 |569.6 |2.8 |

## <a name="about-specint"></a>SPECint hakkında
Windows numaraları hesaplanan çalıştırarak [SPECint 2006](https://www.spec.org/cpu2006/results/rint2006.html) Windows Server'da. SPECint vCPU başına bir kopya ile temel oranı seçeneği (yayımlanan SPECint_rate2006) kullanılarak çalıştırıldı. SPECint 12 ayrı testler her üç kez çalıştırın, her test Medyan değerini almak ve bunları bileşik bir puan oluşturmak üzere ağırlığı oluşur. Bu testlerin sonra gösterilen ortalama puanları sağlamak için birden çok VM çalıştırılmadı.

## <a name="next-steps"></a>Sonraki adımlar
* Depolama kapasitesi, disk ayrıntıları ve VM boyutları arasında seçmeye yönelik ek hususlar için bkz: [sanal makineler için Boyutlar](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

