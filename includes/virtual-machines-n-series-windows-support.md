---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 0f87b122f81fbd04767354e623dcb808466ee1be
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673562"
---
## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

### <a name="nvidia-tesla-cuda-drivers"></a>NVIDIA Tesla (CUDA) sürücüleri

NC, NCv2 NCv3, ND ve NDv2 serisi VM'ler (NV serisi için isteğe bağlı) için NVIDIA Tesla (CUDA) sürücüleri yalnızca aşağıdaki tabloda listelenen işletim sistemlerinden desteklenir. Geçerli yayın zamanında sürücüsü indirme bağlantıları. En son sürücüler için [NVIDIA](https://www.nvidia.com/) web sitesini ziyaret edin.

> [!TIP]
> Bir Windows Server VM'de el ile CUDA sürücü yüklemesi için alternatif olarak, bir Azure dağıtabilirsiniz [veri bilimi sanal makinesi](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntü. Windows Server 2016 için DSVM sürümleri önceden NVIDIA CUDA sürücüleri, CUDA derin sinir ağı kitaplığı ve diğer araçları yükleyin.


| OS | Sürücü |
| -------- |------------- |
| Windows Server 2016 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID sürücüleri

Microsoft, sanal uygulamaları veya NV ve sanal çalışma İstasyonlarınızı kullanılan NVv2 serisi VM'ler için NVIDIA GRID sürücü yükleyiciler yeniden dağıtır. Bu kılavuz sürücüleri yalnızca Azure NV Vm'lerinde yalnızca aşağıdaki tabloda listelenen işletim sistemlerinden yükleyin. Bu sürücüleri kılavuz Azure'da sanal GPU yazılımı için lisans içerir. NVIDIA vGPU yazılım lisans sunucusu ayarlamak gerekmez.

| OS | Sürücü |
| -------- |------------- |
| Windows Server 2019<br/><br/>Windows Server 2016<br/><br/>Windows 10 (en fazla 1809 sürümü. Düzeltme yüklenmemiş/düzeltme eki düzeyi 0 1809 desteklenmiyor.) | [Kılavuz 8.0 (425.31)](https://go.microsoft.com/fwlink/?linkid=874181) (.exe) |
| Windows Server 2012 R2 | [Kılavuz 8.0 (425.31)](https://go.microsoft.com/fwlink/?linkid=874184) (.exe)  |
