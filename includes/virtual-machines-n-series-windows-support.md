---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 03/20/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 426340a5b452124dcba4f51f2ec2c1e2ec0f54b2
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-tesla-cuda-drivers"></a>NC, NCv2, NCv3 ve ND-serisi - NVIDIA Tesla (CUDA) sürücüleri

Aşağıdaki tabloda sürücü indirme bağlantıları yayın zamanında geçerli. En son sürücüler için [NVIDIA](http://www.nvidia.com/) web sitesini ziyaret edin.

> [!TIP]
> Windows Server VM üzerinde el ile CUDA sürücü yükleme alternatif olarak, bir Azure dağıtabilirsiniz [veri bilimi sanal makine](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntü. Windows Server 2016 için DSVM sürümleri NVIDIA CUDA sürücüleri, CUDA derin sinir ağ kitaplığı ve başka araçlar önceden yükleyin.


| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows Server 2016 | [390.85](http://us.download.nvidia.com/Windows/Quadro_Certified/390.85/390.85-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [390.85](http://us.download.nvidia.com/Windows/Quadro_Certified/390.85/390.85-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nv-series---nvidia-grid-drivers"></a>NV-serisi - NVIDIA kılavuz sürücüleri

Microsoft NV VM'ler için NVIDIA kılavuz sürücü yükleyiciler yeniden dağıtır. Bu kılavuz sürücüleri yalnızca Azure NV Vm'lerinde yükleyin. Bu sürücüleri kılavuz Azure sanal GPU yazılım lisansı içerir.

| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows Server 2016 | [GRID 5.2 (386.09)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [GRID 5.2 (386.09)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |