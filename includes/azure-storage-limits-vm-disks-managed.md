---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 12/12/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3ad66d235e00d9866794ca90be9934e174bf4102
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554135"
---
**HDD standart yönetilen diskler**

| Standart disk türü  | S4               | S6               | S10             | S15 | S20              | S30              | S40              | S50              | S60 *             | S70 *             | S80*             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Disk boyutu gib biriminde          | 32             | 64             | 128            | 256  | 512            | 1,024    | 2,048     | 4,095    | 8,192     | 16,384     | 32,767     |
| Disk başına IOPS       | En fazla 500              | En fazla 500              | En fazla 500              | En fazla 500 | En fazla 500              | En fazla 500              | En fazla 500             | En fazla 500              | En fazla 1.300              | En fazla 2.000              | En fazla 2.000              |
| Disk başına aktarım hızı | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn | En fazla 60 MiB/sn| En fazla 300 MiB/sn | En fazla 500 MiB/sn | En fazla 500 MiB/sn |

**Standart SSD yönetilen diskler**

| Standart SSD disk türü | E4                | E6                | E10               | E15               | E20             | E30              | E40              | E50              | E60*             | E70 *             | E80*             |
|------------------------|-------------------|-------------------|-------------------|-------------------|-----------------|------------------|------------------|------------------|-------------------|-------------------|-------------------|
| Disk boyutu gib biriminde       | 32                | 64                | 128               | 256               | 512             | 1,024            | 2,048            | 4,095            | 8,192             | 16,384            | 32,767            |
| Disk başına IOPS          | En fazla 120         | En fazla 240         | En fazla 500         | En fazla 500         | En fazla 500       | En fazla 500        | En fazla 500        | En fazla 500        | En fazla 1.300       | En fazla 2.000       | En fazla 2.000       |
| Disk başına aktarım hızı    | En fazla 25 MB/sn   | En fazla 50 MB/sn   | En fazla 60 MB/sn   | En fazla 60 MB/sn   | En fazla 60 MB/sn | En fazla 60 MB/sn  | En fazla 60 MB/sn  | En fazla 60 MB/sn  | En fazla 300 MiB/sn | En fazla 500 MiB/sn | En fazla 500 MiB/sn |

**Premium SSD yönetilen diskler: Disk başına limitler**

| Premium disk türü  | P4               | P6               | P10             | P15 | P20              | P30              | P40              | P50              | P60 *             | P70 *             | P80 *             |
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|
| Disk boyutu gib biriminde           | 32             | 64             | 128            | 256  | 512            | 1,024    | 2,048     | 4,095    | 8,192     | 16,384     | 32,767     |
| Disk başına IOPS       | En fazla 120 | En fazla 240              | En fazla 500              | En fazla 1.100 | En fazla 2,300              | En fazla 5000              | En fazla 7.500             | En fazla 7.500              | En fazla 12.500              | En fazla 15000              | 20000              |
| Disk başına aktarım hızı | En fazla 25 MiB/sn | En çok 50 MiB/sn | En fazla 100 MiB/sn | En fazla 125 MiB/sn | En fazla 150 MiB/sn | En fazla 200 MiB/sn | En fazla 250 MiB/sn | En fazla 250 MiB/sn| En fazla 480 MiB/sn | En fazla 750 MiB/sn | En fazla 750 MiB/sn |

**Premium SSD yönetilen diskler: VM başına limitler**

| Kaynak | Varsayılan limit |
| --- | --- |
| VM başına maksimum IOPS |GS5 VM ile 80.000 IOPS |
| VM başına en fazla aktarım hızı |GS5 VM ile 2.000 MB/sn |
