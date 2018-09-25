---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 09/24/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 2fefe23a57668ce20ed1a6afaf3514750e3c72da
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47043889"
---
## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

### <a name="nvidia-tesla-cuda-drivers"></a>NVIDIA Tesla (CUDA) sürücüleri

NC, NCv2 NCv3 ve ND serisi VM'ler (NV serisi için isteğe bağlı) için NVIDIA Tesla (CUDA) sürücüleri yalnızca aşağıdaki tabloda listelenen işletim sistemlerinden desteklenir. Geçerli yayın zamanında sürücüsü indirme bağlantıları. En son sürücüler için [NVIDIA](http://www.nvidia.com/) web sitesini ziyaret edin.

> [!TIP]
> Bir Windows Server VM'de el ile CUDA sürücü yüklemesi için alternatif olarak, bir Azure dağıtabilirsiniz [veri bilimi sanal makinesi](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntü. Windows Server 2016 için DSVM sürümleri önceden NVIDIA CUDA sürücüleri, CUDA derin sinir ağı kitaplığı ve diğer araçları yükleyin.


| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows Server 2016 | [398.75](http://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [398.75](http://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |


### <a name="nvidia-grid-drivers"></a>NVIDIA GRID sürücüleri

Microsoft, sanal uygulamaları veya NV amd sanal çalışma İstasyonlarınızı kullanılan NVv2 serisi VM'ler için NVIDIA GRID sürücü yükleyiciler yeniden dağıtır. Bu kılavuz sürücüleri yalnızca Azure NV Vm'lerinde yalnızca aşağıdaki tabloda listelenen işletim sistemlerinden yükleyin. Bu sürücüleri kılavuz Azure'da sanal GPU yazılımı için lisans içerir.

| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows Server 2016<br/><br/>Windows 10 | [Kılavuz 6.2 (391.81)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [Kılavuz 6.2 (391.81)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |