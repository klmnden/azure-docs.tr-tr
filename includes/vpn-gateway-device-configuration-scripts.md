---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 01/09/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 78dfd57fba6365f9c8937b30b5cf96b840749c68
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188241"
---
| **Satıcı** | **Cihaz ailesi** | **Üretici yazılımı sürümü** |
| --- | --- | --- |
|Cisco | ISR| IOS 15.1 (Önizleme)|
|Cisco | ASA | ASA 9.8 aşağıda için ASA (*) RouteBased (BGP Ikev2 yok) |
|Cisco | ASA | ASA 9.8 + için ASA RouteBased (Ikev2 - BGP yok) |
|Juniper | SRX_GA | 12.x|
|Juniper | SSG_GA | ScreenOS 6.2.x|
|Juniper | JSeries_GA | JunOS 12.x|
|Juniper | SRX | JunOS 12.x RouteBased BGP |
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased VTI|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased BGP|

> [!NOTE]
> ( * ) Gerekli: (UsePolicyBasedTrafficSelectors seçeneğini etkinleştirin) NarrowAzureTrafficSelectors ve CustomAzurePolicies (IPSec/IKE)
>
