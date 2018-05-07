---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 05/01/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 7139ec67536a1c0e41c991db6d867b956f995c11
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
## <a name="supported-distributions-and-drivers"></a>Desteklenen dağıtımlar ve sürücüler

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-cuda-drivers"></a>NC, NCv2, NCv3 ve ND-serisi - NVIDIA CUDA sürücüleri

Aşağıdaki tabloda CUDA sürücü bilgileri yayın zamanında geçerli olur. En son CUDA sürücüleri için ziyaret [NVIDIA](https://developer.nvidia.com/cuda-zone) Web sitesi. Yükleme veya dağıtım için en son CUDA sürücüleri yükseltme emin olun. 

> [!TIP]
> Bir Linux VM el ile CUDA sürücü yüklemesinde alternatif olarak, bir Azure dağıtabilirsiniz [veri bilimi sanal makine](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntü. Ubuntu 16.04 LTS veya CentOS 7.4 DSVM sürümleri NVIDIA CUDA sürücüleri, CUDA derin sinir ağ kitaplığı ve başka araçlar önceden yükleyin.

| Dağıtım | Sürücü |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 ya da 7.4<br/><br/> CentOS tabanlı 7.3 ya da 7.4, CentOS tabanlı 7.4 HPC | NVIDIA CUDA 9.1, sürücüyü dal R390 |

### <a name="nv-series---nvidia-grid-drivers"></a>NV-serisi - NVIDIA kılavuz sürücüleri

Microsoft NV VM'ler için NVIDIA kılavuz sürücü yükleyiciler yeniden dağıtır. Bu kılavuz sürücüleri yalnızca Azure NV Vm'lerinde yükleyin. Bu sürücüleri kılavuz Azure sanal GPU yazılım lisansı içerir.

| Dağıtım | Sürücü |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3 ya da 7.4<br/><br/>CentOS tabanlı 7.3 veya 7.4 | NVIDIA kılavuz 6.0, sürücüyü dal R390|



> [!WARNING] 
> Red Hat ürünlerine üçüncü taraf yazılım yüklenmesi Red Hat destek koşullarını etkileyebilir. Bkz. [Red Hat Bilgi Bankası makalesi](https://access.redhat.com/articles/1067).
>
