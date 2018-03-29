---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 03/19/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 5dc0636fa4bfede460b1c8cd8bdab5628a110578
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
## <a name="supported-distributions-and-drivers"></a>Desteklenen dağıtımlar ve sürücüler

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-cuda-drivers"></a>NC, NCv2, NCv3 ve ND-serisi - NVIDIA CUDA sürücüleri

Aşağıdaki tabloda CUDA sürücü bilgileri yayın zamanında geçerli olur. En son CUDA sürücüleri için ziyaret [NVIDIA](https://developer.nvidia.com/cuda-zone) Web sitesi. Yükleme veya dağıtım için en son CUDA sürücüleri yükseltme emin olun. 

> [!TIP]
> Bir Linux VM el ile CUDA sürücü yüklemesinde alternatif olarak, bir Azure dağıtabilirsiniz [veri bilimi sanal makine](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntü. Ubuntu 16.04 LTS veya CentOS 7.4 DSVM sürümleri NVIDIA CUDA sürücüleri, CUDA derin sinir ağ kitaplığı ve başka araçlar önceden yükleyin.

| Dağıtım | Sürücü |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 ya da 7.4<br/><br/> CentOS 7.3 ya da 7.4, CentOS tabanlı 7.4 HPC | NVIDIA CUDA 9.1, sürücüyü dal R390 |

### <a name="nv-series---nvidia-grid-drivers"></a>NV-serisi - NVIDIA kılavuz sürücüleri

Microsoft NV VM'ler için NVIDIA kılavuz sürücü yükleyiciler yeniden dağıtır. Bu kılavuz sürücüleri yalnızca Azure NV Vm'lerinde yükleyin. Bu sürücüleri kılavuz Azure sanal GPU yazılım lisansı içerir.

| Dağıtım | Sürücü |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3<br/><br/>CentOS tabanlı 7.3 | NVIDIA kılavuz 5.2, sürücüyü dal R384|



> [!WARNING] 
> Red Hat ürünlerine üçüncü taraf yazılım yüklenmesi Red Hat destek koşullarını etkileyebilir. Bkz. [Red Hat Bilgi Bankası makalesi](https://access.redhat.com/articles/1067).
>
