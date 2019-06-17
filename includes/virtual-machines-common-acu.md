---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 12/21/2018
ms.author: cynthn;davberg
ms.custom: include file
ms.openlocfilehash: ca0c45b5d74140e0398595070df71da669caab53
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66160081"
---
Kavram, Azure işlem birimi (ACU) açısından, bilgi işlem (CPU) performansını Azure SKU'ları arasında karşılaştırma bir yol sağlar. Bu birim, performans ihtiyaçlarınızı karşılayabilecek SKU'yu kolayca belirlemenize yardımcı olacak.  ACU şu anda Küçük (Standard_A1) VM'de 100 olarak standart haline getirilmiş ve diğer tüm SKU'lar, standart bir karşılaştırmalı testte sunabilecekleri yaklaşık hıza göre derecelendirilmiştir. 

> [!IMPORTANT]
> ACU yalnızca rehberlik etme amacı taşımaktadır.  İş yükünüzle aldığınız sonuçlar farklılık gösterebilir. 
> 
> 

<br>

| SKU Ailesi | ACU \ vCPU | vCPU: Çekirdek |
| --- | --- |---|
| [A0](../articles/virtual-machines/windows/sizes-general.md) |50 | 1:1 |
| [A1 - A4](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A5 - A7](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A1_v2 - A8_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A2m_v2 - A8m_v2](../articles/virtual-machines/windows/sizes-general.md) |100 | 1:1 |
| [A8 - A11](../articles/virtual-machines/windows/sizes-hpc.md) |225* | 1:1 |
| [D1 - D14](../articles/virtual-machines/windows/sizes-general.md) |160 - 250 | 1:1 |
| [D1_v2 - D15_v2](../articles/virtual-machines/windows/sizes-general.md) |210-250* | 1:1 |
| [DS1 - DS14](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 - 250 | 1:1 |
| [DS1_v2 - DS15_v2](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |210-250* | 1:1 |
| [D_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160 - 190* | 2:1\*\*\* |
| [Ds_v3](../articles/virtual-machines/virtual-machines-windows-sizes-general.md) |160 - 190* | 2:1\*\*\* |
| [E_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 - 190* | 2:1\*\*\*|
| [Es_v3](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |160 - 190* | 2:1\*\*\* |
| [F2s_v2 - F72s_v2](../articles/virtual-machines/windows/sizes-compute.md) |195 - 210* | 2:1\*\*\* |
| [F1 - F16](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 |
| [F1s - F16s](../articles/virtual-machines/windows/sizes-compute.md) |210-250* | 1:1 |
| [G1 - G5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180-240* | 1:1 |
| [GS1 - GS5](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) |180-240* | 1:1 |
| [H](../articles/virtual-machines/windows/sizes-hpc.md) |290-300* | 1:1 |
| [HB](../articles/virtual-machines/windows/sizes-hpc.md) |199 - 216** | 1:1 |
| [HC](../articles/virtual-machines/windows/sizes-hpc.md) |297 - 315* | 1:1 |
| [L4s - L32s](../articles/virtual-machines/windows/sizes-storage.md) |180-240* | 1:1 |
| [L8s_v2 - L80s_v2](../articles/virtual-machines/windows/sizes-storage.md) |150 - 175** | 2:1 |
| [M](../articles/virtual-machines/virtual-machines-windows-sizes-memory.md) | 160 - 180 | 2:1\*\*\* |

\* Acu'lar, CPU frekansını artırmak ve performans artışı sağlamak için Intel® Turbo teknolojisini kullanır.  Performans artışı miktarını VM boyutuna, iş yükü ve aynı ana bilgisayarda çalışan iş yüklerine göre değişebilir.

** Acu'lar, CPU frekansını artırmak ve performans artışı sağlamak için AMD® Boost teknolojisini kullanır.  Performans artışı miktarını VM boyutuna, iş yükü ve aynı ana bilgisayarda çalışan iş yüklerine göre değişebilir.

Hyper-Threading Teknolojisi ve iç içe sanallaştırmayı çalıştıran özellikli
