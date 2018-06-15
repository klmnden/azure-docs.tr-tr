---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: b7bbf1fb8ffc1f57a53682d3baf288077b144572
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664692"
---
### <a name="virtual-networks"></a>Sanal Ağlar

|  |  |
|---------|---------|
| [İzin verilen Application Gateway SKU’ları](../articles/azure-policy/scripts/allowed-app-gate-sku.md) | Uygulama ağ geçitlerinin onaylı bir SKU kullanmasını gerektirir. Onaylı bir SKU dizisi belirtirsiniz. |
| [ER ağı ile ağ eşlemesi yok](../articles/azure-policy/scripts/no-peering-er-net.md) | Bir ağ eşlemesinin belirli bir kaynak grubundaki bir ağ ile ilişkilendirilmesini önler. Merkezi yönetilen ağ altyapısı ile bağlantıyı engellemek için kullanın. İlişkilendirmeyi önlemek için kaynak grubunun adını belirtirsiniz. |
| [Kullanıcı Tanımlı Yol Tablosu Yok](../articles/azure-policy/scripts/no-user-def-route-table.md)  |Sanal ağların kullanıcı tanımlı bir yönlendirme tablosu ile dağıtılmasını engeller. |
| [Her alt ağ üzerinde NSG X](../articles/azure-policy/scripts/nsg-on-subnet.md) | Her sanal alt ağ ile belirli bir ağ güvenlik grubunun kullanılmasını gerektirir. Kullanılacak ağ güvenlik grubunun kimliğini belirtirsiniz. |
| [VM ağ arabirimlerinde onaylanan alt ağı kullan](../articles/azure-policy/scripts/use-approved-subnet-vm-nics.md) | Ağ arabirimlerinin onaylı bir alt ağ kullanmasını gerektirir. Onaylanan alt ağın kimliğini belirtirsiniz. |
| [VM ağ arabirimlerinde onaylanan sanal ağı kullan](../articles/azure-policy/scripts/use-approved-vnet-vm-nics.md) | Ağ arabirimlerinin onaylı bir sanal ağ kullanmasını gerektirir. Onaylanan sanal ağın kimliğini belirtirsiniz. |