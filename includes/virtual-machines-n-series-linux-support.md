---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 05/29/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0264f92fa10bd503a2811ce40ee0b8d4edd5f3b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34669841"
---
## <a name="supported-distributions-and-drivers"></a>Desteklenen dağıtımlar ve sürücüler

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA sürücüleri

NVIDIA CUDA sürücüleri NC, NCv2, NCv3 ve ND-serisi VM'ler (NV-seri için isteğe bağlı) için aşağıdaki tabloda listelenen Linux dağıtımları üzerinde desteklenir. CUDA sürücü bilgileri yayın zamanında geçerli olur. En son CUDA sürücüleri için ziyaret [NVIDIA](https://developer.nvidia.com/cuda-zone) Web sitesi. Yükleme veya dağıtım için en son CUDA sürücüleri yükseltme emin olun. 

> [!TIP]
> Bir Linux VM el ile CUDA sürücü yüklemesinde alternatif olarak, bir Azure dağıtabilirsiniz [veri bilimi sanal makine](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntü. Ubuntu 16.04 LTS veya CentOS 7.4 DSVM sürümleri NVIDIA CUDA sürücüleri, CUDA derin sinir ağ kitaplığı ve başka araçlar önceden yükleyin.

| Dağıtım | Sürücü |
| --- | -- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 ya da 7.4<br/><br/> CentOS tabanlı 7.3 ya da 7.4, CentOS tabanlı 7.4 HPC | NVIDIA CUDA 9.1, sürücüyü dal R390 |

### <a name="nvidia-grid-drivers"></a>NVIDIA kılavuz sürücüleri

Microsoft sanal iş istasyonları kullanılan NV-serisi VM'ler için veya sanal uygulamalar için NVIDIA kılavuz sürücü yükleyiciler yeniden dağıtır. Yalnızca bu kılavuz sürücüleri yalnızca aşağıdaki tabloda listelenen dağıtımları Azure NV Vm'lerinde yükleyin. Bu sürücüleri kılavuz Azure sanal GPU yazılım lisansı içerir.

| Dağıtım | Sürücü |
| --- | -- |
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 ya da 7.4<br/><br/>CentOS tabanlı 7.3 veya 7.4 | NVIDIA kılavuz 6.0, sürücüyü dal R390|



> [!WARNING] 
> Red Hat ürünlerine üçüncü taraf yazılım yüklenmesi Red Hat destek koşullarını etkileyebilir. Bkz. [Red Hat Bilgi Bankası makalesi](https://access.redhat.com/articles/1067).
>
