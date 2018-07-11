---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: dlepow
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 05/29/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: a07f1622f2d41d105c79fa5354d97aa079567e62
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935768"
---
## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

### <a name="nvidia-tesla-cuda-drivers"></a>NVIDIA Tesla (CUDA) sürücüleri

NC, NCv2 NCv3 ve ND serisi VM'ler (NV serisi için isteğe bağlı) için NVIDIA Tesla (CUDA) sürücüleri yalnızca aşağıdaki tabloda listelenen işletim sistemlerinden desteklenir. Geçerli yayın zamanında sürücüsü indirme bağlantıları. En son sürücüler için [NVIDIA](http://www.nvidia.com/) web sitesini ziyaret edin.

> [!TIP]
> Bir Windows Server VM'de el ile CUDA sürücü yüklemesi için alternatif olarak, bir Azure dağıtabilirsiniz [veri bilimi sanal makinesi](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntü. Windows Server 2016 için DSVM sürümleri önceden NVIDIA CUDA sürücüleri, CUDA derin sinir ağı kitaplığı ve diğer araçları yükleyin.


| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows Server 2016 | [397.44](http://us.download.nvidia.com/Windows/Quadro_Certified/397.44/397.44-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [397.44](http://us.download.nvidia.com/Windows/Quadro_Certified/397.44/397.44-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |


### <a name="nvidia-grid-drivers"></a>NVIDIA GRID sürücüleri

Microsoft, sanal uygulamaları veya sanal çalışma İstasyonlarınızı kullanılan NV serisi VM'ler için NVIDIA GRID sürücü yükleyiciler yeniden dağıtır. Bu kılavuz sürücüleri yalnızca Azure NV Vm'lerinde yalnızca aşağıdaki tabloda listelenen işletim sistemlerinden yükleyin. Bu sürücüleri kılavuz Azure'da sanal GPU yazılımı için lisans içerir.

| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows Server 2016<br/><br/>Windows 10 | [Kılavuz 6.2 (391.81)](https://go.microsoft.com/fwlink/?linkid=836843) (.exe) |
| Windows Server 2012 R2 | [Kılavuz 6.2 (391.81)](https://go.microsoft.com/fwlink/?linkid=836844) (.exe)  |