---
title: "include dosyası"
description: "include dosyası"
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: cynthn;davberg
ms.custom: include file
ms.openlocfilehash: fde43e40a7a5bb87b9e63af47ae795616fac8b3f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
Azure işlem birim'nı (ACU) kavramı Azure SKU'ları üzerinde işlem (CPU) performans karşılaştırma bir yolunu sunar. Bu birim, performans ihtiyaçlarınızı karşılayabilecek SKU'yu kolayca belirlemenize yardımcı olacak.  ACU şu anda Küçük (Standard_A1) VM'de 100 olarak standart haline getirilmiş ve diğer tüm SKU'lar, standart bir karşılaştırmalı testte sunabilecekleri yaklaşık hıza göre derecelendirilmiştir. 

> [!IMPORTANT]
> ACU yalnızca rehberlik etme amacı taşımaktadır.  İş yükünüzle aldığınız sonuçlar farklılık gösterebilir. 
> 
> 

<br>

| SKU Ailesi | ACU \ vCPU | vCPU: Core |
| --- | --- |---|
| [A0](../articles/virtual-machines/windows/sizes-general.md) |50 | 1:1 |
| [A1-A4](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A5-A7](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A1_v2-A8_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A2m_v2-A8m_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A8-A11](../articles/virtual-machines/windows/sizes-hpc.md) |225* | 1:1 |
| [D1-D14](../articles/virtual-machines/windows/sizes-general.md) |160 | 1:1 |
| [D1_v2-D15_v2](../articles/virtual-machines/windows/sizes-general.md) |210-250* | 1:1 |
| [DS1-DS14](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 | 1:1 |
| [DS1_v2-DS15_v2](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |210-250* | 1:1 |
| [D_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160-190* | 2:1** |
| [Ds_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160-190* | 2:1** |
| [E_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160-190* | 2:1** |
| [Es_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160-190* | 2:1** |
| [F2s_v2-F72s_v2](../articles/virtual-machines/windows/sizes-compute.md) |195-210* | 2:1** |
| [F1-F16](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 |
| [F1s-F16s](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 |
| [G1-G5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180-240* | 1:1 |
| [GS1-GS5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180-240* | 1:1 |
| [H](../articles/virtual-machines/windows/sizes-hpc.md) |290-300* | 1:1 |
| [L4s L32s](../articles/virtual-machines/windows/sizes-storage.md) |180-240* | 1:1 |
| [M](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) | 160-180 | 2:1** |

* işaretli ACU'lar, CPU frekansını artırmak ve performans artışı sağlamak için Intel® Turbo Boost teknolojisinden faydalanır.  Performans artışının oranı VM boyutuna, iş yüküne ve aynı ana bilgisayarda çalışan iş yüklerine göre değişiklik gösterebilir.

** Hiper iş parçacığı. 
