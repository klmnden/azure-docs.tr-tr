---
title: "include dosyası"
description: "include dosyası"
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 03/01/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 22d37ca30f1319f46a52b96be1c527f6f56719ab
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2018
---
## <a name="supported-distributions-and-drivers"></a>Desteklenen dağıtımlar ve sürücüler

### <a name="nc-ncv2-ncv3-and-nd-series---nvidia-cuda-drivers"></a>NC, NCv2, NCv3 ve ND-serisi - NVIDIA CUDA sürücüleri
| Dağıtım | Sürücü |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3 ya da 7.4<br/><br/> 7.3 ya da 7.4 centOS | NVIDIA CUDA 9.1, sürücüyü dal R390 |

> [!IMPORTANT]
> Yükleme veya dağıtım için en son CUDA sürücüleri yükseltme emin olun. Sürücüleri R390 sürümden daha eski güncelleştirilmiş Linux tekrar ile ilgili sorun yaşayabilir.
>

### <a name="nv-series---nvidia-grid-drivers"></a>NV-serisi - NVIDIA kılavuz sürücüleri

| Dağıtım | Sürücü |
| --- | --- | 
| Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.3<br/><br/>CentOS tabanlı 7.3 | NVIDIA kılavuz 5.2, sürücüyü dal R384|

> [!NOTE]
> Microsoft NV VM'ler için NVIDIA kılavuz sürücü yükleyiciler yeniden dağıtır. Bu kılavuz sürücüleri yalnızca Azure NV Vm'lerinde yükleyin. Bu sürücüleri kılavuz Azure sanal GPU yazılım lisansı içerir.
>

> [!WARNING] 
> Red Hat ürünlerine üçüncü taraf yazılım yüklenmesi Red Hat destek koşullarını etkileyebilir. Bkz. [Red Hat Bilgi Bankası makalesi](https://access.redhat.com/articles/1067).
>
