---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 82cbffb257d85197848b8bca14231e5363d6d45c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60729860"
---
SQL Server veya Oracle gibi bazı veritabanı iş yükleri, yüksek bellek, depolama ve g/ç bant genişliği, ancak yüksek çekirdek sayısı gerektirir. Birçok veritabanı iş yüklerini CPU yoğunluklu değildir. Azure VM vCPU sayısı aynı bellek, depolama ve g/ç bant genişliği koruyarak yazılım lisans maliyetini azaltmak için nereye kısıtlayabilirsiniz belirli VM boyutları sunar.

VCPU sayısı kısıtlı özgün VM boyutunun yarısı veya bir üç aylık dönem için. Bu yeni VM boyutları, belirlemenizi kolaylaştırmak için etkin Vcpu sayısını belirten bir son eke sahiptir.

Örneğin, mevcut VM boyutlarının Standard_GS5 32 Vcpu, 448 GB RAM ile sunulur, 64 (en fazla 256 TB), diskler ve 80.000 IOPS veya 2 GB/sn g/ç bant genişliği. Standard_GS5-16 ve yeni VM boyutları ve kalan bellek, depolama ve g/ç bant genişliği Standard_GS5 özellikleri korurken sırasıyla Standard_GS5-8 ile 16 ve 8 active Vcpu gelir.

SQL Server veya Oracle için yeni vCPU sayısı sınırlı olduğu ve diğer ürünler yazılacağını lisans Ücretlerle yeni vCPU sayısını temel. VM özellikleri oranını %50 %75 artış için etkin (Faturalanabilir) Vcpu sonuçlanır. Yalnızca Azure’da sunulan bu yeni VM boyutları sayesinde iş yükleri, lisans maliyetlerinin (çekirdek başına) çok daha azıyla CPU kullanımını artırabilir. Şu anda işletim sistemi lisans içerir, işlem maliyeti özgün boyutu olarak aynı kalır. Daha fazla bilgi için [daha uygun maliyetli veritabanı iş yükleri için Azure VM boyutlarını](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).


| Ad                | Sanal işlemci | Özellikler           |
|---------------------|------|-----------------|
| Standard_M8-2ms     | 2    | Aynı M8ms    |
| Standard_M8-4ms     | 4    | Aynı M8ms    |
| Standard_M16-4ms    | 4    | Aynı M16ms   |
| Standard_M16-8ms    | 8    | Aynı M16ms   |
| Standard_M32-8ms    | 8    | Aynı M32ms   |
| Standard_M32-16ms   | 16   | Aynı M32ms   |
| Standard_M64-32ms   | 32   | Aynı M64ms   |
| Standard_M64-16ms   | 16   | Aynı M64ms   |
| Standard_M128-64ms  | 64   | Aynı M128ms  |
| Standard_M128-32ms  | 32   | Aynı M128ms  |
| Standard_E4-2s_v3   | 2    | Same as E4s_v3  |
| Standard_E8-4s_v3   | 4    | Same as E8s_v3  |
| Standard_E8-2s_v3   | 2    | Same as E8s_v3  |
| Standard_E16-8s_v3  | 8    | Same as E16s_v3 |
| Standard_E16-4s_v3  | 4    | Same as E16s_v3 |
| Standard_E32-16_v3  | 16   | Same as E32s_v3 |
| Standard_E32-8s_v3  | 8    | Same as E32s_v3 |
| Standard_E64-32s_v3 | 32   | Same as E64s_v3 |
| Standard_E64-16s_v3 | 16   | Same as E64s_v3 |
| Standard_GS4-8      | 8    | Aynı GS4     |
| Standard_GS4-4      | 4    | Aynı GS4     |
| Standard_GS5-16     | 16   | Aynı GS5     |
| Standard_GS5-8      | 8    | Aynı GS5     |
| Standard_DS11-1_v2  | 1    | DS11_v2 aynı |
| Standard_DS12-2_v2  | 2    | DS12_v2 aynı |
| Standard_DS12-1_v2  | 1    | DS12_v2 aynı |
| Standard_DS13-4_v2  | 4    | DS13_v2 aynı |
| Standard_DS13-2_v2  | 2    | DS13_v2 aynı |
| Standard_DS14-8_v2  | 8    | DS14_v2 aynı |
| Standard_DS14-4_v2  | 4    | DS14_v2 aynı |
