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
ms.openlocfilehash: 00889717e0c22477b9933725bccc7de05c82bc4f
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47454515"
---
| **Satıcı** | **Cihaz ailesi** | **Üretici yazılımı sürümü** |
| --- | --- | --- |
|Cisco | ISR| IOS 15.1 (Önizleme)|
|Cisco | ASA | ASA 9.8 aşağıda için ASA (*) RouteBased (BGP Ikev2 yok) |
|Cisco | ASA | ASA 9.8 + için ASA RouteBased (Ikev2 - BGP yok) |
|Juniper | SRX_GA | 12.x|
|Juniper | SSG_GA | ScreenOS 6.2.x|
|Juniper | JSeries_GA | JunOS 12.x|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased VTI|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased BGP|

> [!NOTE]
> (*) Gerekli: NarrowAzureTrafficSelectors (UsePolicyBasedTrafficSelectors seçeneğini etkinleştirin) ve CustomAzurePolicies (IPSec/IKE)
>
