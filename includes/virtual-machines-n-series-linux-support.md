---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 11/27/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: df7d8815eeb588e4e99041844642b6721e25dad7
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52585857"
---
## <a name="supported-distributions-and-drivers"></a>Desteklenen dağıtımlar ve sürücüler

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA sürücüleri

NVIDIA CUDA sürücüleri NC, NCv2 NCv3, ND ve NDv2 serisi VM'ler (NV serisi için isteğe bağlı) için aşağıdaki tabloda listelenen Linux dağıtımlarında desteklenir. CUDA sürücü bilgileri yayın zamanında güncel değil. En son CUDA sürücüleri için ziyaret [NVIDIA](https://developer.nvidia.com/cuda-zone) Web sitesi. Yükleme veya dağıtımınız için en son CUDA sürücüleri için yükseltme emin olun. 

> [!TIP]
> Bir Azure Linux VM'de el ile CUDA sürücü yüklemesi için alternatif olarak, dağıtabilirsiniz [veri bilimi sanal makinesi](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntü. Ubuntu 16.04 LTS veya 7.4 CentOS DSVM sürümleri önceden NVIDIA CUDA sürücüleri, CUDA derin sinir ağı kitaplığı ve diğer araçları yükleyin.

| Dağıtım | Sürücü |
| --- | -- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.4 ya da 7.3<br/><br/> CentOS tabanlı 7.3 veya 7.4, CentOS tabanlı 7.4 HPC | NVIDIA CUDA 10.0, sürücü dalı R410 |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID sürücüleri

Microsoft, sanal uygulamaları veya NV ve sanal çalışma İstasyonlarınızı kullanılan NVv2 serisi VM'ler için NVIDIA GRID sürücü yükleyiciler yeniden dağıtır. Bu kılavuz sürücüleri yalnızca Azure NV Vm'lerinde yalnızca aşağıdaki tabloda listelenen işletim sistemlerinden yükleyin. Bu sürücüleri kılavuz Azure'da sanal GPU yazılımı için lisans içerir. NVIDIA vGPU yazılım lisans sunucusu ayarlamak gerekmez.

| Dağıtım | Sürücü |
| --- | -- |
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.4 ya da 7.3<br/><br/>CentOS tabanlı 7.3 veya 7.4 | NVIDIA Kılavuz 7, sürücü dalı R410|

> [!WARNING] 
> Red Hat ürünlerine üçüncü taraf yazılım yüklenmesi Red Hat destek koşullarını etkileyebilir. Bkz. [Red Hat Bilgi Bankası makalesi](https://access.redhat.com/articles/1067).
>
