---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9cdb608505e594e0020eb33abc869c6bf4b6b263
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
| **Satıcı** | **Cihaz ailesi** | **Bellenim sürümü** |
| --- | --- | --- |
|Cisco | ISR| IOS 15.1 (Önizleme)|
|Cisco | ASA | ASA 9.8 aşağıda için ASA (*) RouteBased (Ikev2 Hayır BGP) |
|Cisco | ASA | ASA 9.8 + için ASA RouteBased (Ikev2 - hiçbir BGP) |
|Juniper | SRX_GA | 12.x|
|Juniper | SSG_GA | ScreenOS 6.2.x|
|Juniper | JSeries_GA | JunOS 12.x|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased VTI|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased BGP|

> [!NOTE]
> (*) Gerekli: NarrowAzureTrafficSelectors ve CustomAzurePolicies (IKE/IPSec)
>