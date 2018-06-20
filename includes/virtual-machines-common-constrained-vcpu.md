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
ms.openlocfilehash: eaf490979167ea5cbfe5846e62444526310452f8
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36269553"
---
SQL Server veya Oracle gibi bazı veritabanı iş yükleri, yüksek bellek, depolama ve g/ç bant genişliği, ancak yüksek çekirdek sayısı gerektirir. Çok sayıda veritabanı iş yükü CPU-yoğun değildir. Azure VM vCPU sayısı aynı bellek, depolama ve g/ç bant genişliği koruyarak Yazılım Lisanslama maliyetini azaltmak için burada kısıtlayabilirsiniz belirli VM boyutları sunar.

VCPU sayısı kısıtlı özgün VM boyutunun yarısı ya da bir üç aylık dönem için. Bu yeni VM boyutları belirlemenizi kolaylaştırmak için etkin Vcpu'lar sayısını belirten bir son eke sahip.

Örneğin, geçerli VM boyutu Standard_GS5 32 Vcpu, 448 GB RAM ile gelir, 64 (en fazla 256 TB), diskleri ve 80.000 IOPS ya da 2 GB/sn, g/ç bant genişliği. Standard_GS5-16 yeni VM boyutları ve bellek, depolama ve g/ç bant genişliği Standard_GS5 belirtimlerin kalan korurken sırasıyla Standard_GS5-8 ile 16 ve 8 active Vcpu'lar sunar.

SQL Server veya Oracle yeni vCPU sayısı kısıtlı ve diğer ürünler yazılacağını hesaplanan lisans ücretlerin yeni vCPU sayısına göre. Bu VM belirtimlerin oranını 50-% 75 artış için etkin (Faturalanabilir) Vcpu'lar sonuçlanır. Yalnızca Azure’da sunulan bu yeni VM boyutları sayesinde iş yükleri, lisans maliyetlerinin (çekirdek başına) çok daha azıyla CPU kullanımını artırabilir. Şu anda işletim sistemi lisans içerir, işlem maliyet özgün boyutu aynı kalır. Daha fazla bilgi için bkz: [daha fazla uygun maliyetli veritabanı iş yükleri için Azure VM boyutlarını](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).


| Ad                | Sanal işlemci | Özellikler           |
|---------------------|------|-----------------|
| Standard_M8 2ms'kadar     | 2    | M8ms aynı    |
| Standard_M8 4ms     | 4    | M8ms aynı    |
| Standard_M16 4ms    | 4    | M16ms aynı   |
| Standard_M16 8ms    | 8    | M16ms aynı   |
| Standard_M32 8ms    | 8    | M32ms aynı   |
| Standard_M32 16ms   | 16   | M32ms aynı   |
| Standard_M64 32ms   | 32   | M64ms aynı   |
| Standard_M64 16ms   | 16   | M64ms aynı   |
| Standard_M128 64ms  | 64   | M128ms aynı  |
| Standard_M128 32ms  | 32   | M128ms aynı  |
| Standard_E4 2s_v3   | 2    | E4s_v3 aynı  |
| Standard_E8 4s_v3   | 4    | E8s_v3 aynı  |
| Standard_E8 2s_v3   | 2    | E8s_v3 aynı  |
| Standard_E16 8s_v3  | 8    | E16s_v3 aynı |
| Standard_E16 4s_v3  | 4    | E16s_v3 aynı |
| Standard_E32 16_v3  | 16   | E32s_v3 aynı |
| Standard_E32 8s_v3  | 8    | E32s_v3 aynı |
| Standard_E64-32s_v3 | 32   | E64s_v3 aynı |
| Standard_E64 16s_v3 | 16   | E64s_v3 aynı |
| Standard_GS4-8      | 8    | GS4 aynı     |
| Standard_GS4-4      | 4    | GS4 aynı     |
| Standard_GS5-16     | 16   | GS5 aynı     |
| Standard_GS5-8      | 8    | GS5 aynı     |
| Standard_DS11-1_v2  | 1    | DS11_v2 aynı |
| Standard_DS12-2_v2  | 2    | DS12_v2 aynı |
| Standard_DS12-1_v2  | 1    | DS12_v2 aynı |
| Standard_DS13-4_v2  | 4    | DS13_v2 aynı |
| Standard_DS13-2_v2  | 2    | DS13_v2 aynı |
| Standard_DS14-8_v2  | 8    | DS14_v2 aynı |
| Standard_DS14-4_v2  | 4    | DS14_v2 aynı |
