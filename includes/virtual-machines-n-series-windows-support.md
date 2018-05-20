---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/16/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 9b36a8e90c20c8f0d08ca241e48e9306fb5b10f8
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-tesla-cuda-drivers"></a>NC, NCv2, NCv3 ve ND-serisi - NVIDIA Tesla (CUDA) sürücüleri

Aşağıdaki tabloda sürücü indirme bağlantıları yayın zamanında geçerli. En son sürücüler için [NVIDIA](http://www.nvidia.com/) web sitesini ziyaret edin.

> [!TIP]
> Windows Server VM üzerinde el ile CUDA sürücü yükleme alternatif olarak, bir Azure dağıtabilirsiniz [veri bilimi sanal makine](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntü. Windows Server 2016 için DSVM sürümleri NVIDIA CUDA sürücüleri, CUDA derin sinir ağ kitaplığı ve başka araçlar önceden yükleyin.


| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows Server 2016 | [391.29](http://us.download.nvidia.com/Windows/Quadro_Certified/391.29/391.29-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [391.29](http://us.download.nvidia.com/Windows/Quadro_Certified/391.29/391.29-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nv-series---nvidia-grid-drivers"></a>NV-serisi - NVIDIA kılavuz sürücüleri

Microsoft NV VM'ler için NVIDIA kılavuz sürücü yükleyiciler yeniden dağıtır. Bu kılavuz sürücüleri yalnızca Azure NV Vm'lerinde yükleyin. Bu sürücüleri kılavuz Azure sanal GPU yazılım lisansı içerir.

| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows Server 2016<br/><br/>Windows 10 | [Kılavuz 6.0 (391.03)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [Kılavuz 6.0 (391.03)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |