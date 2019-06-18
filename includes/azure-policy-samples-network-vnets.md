---
title: include dosyası
description: include dosyası
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: f93e22012a4855257f5372c1fc1dbc05ad29a6cd
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188518"
---
### <a name="virtual-networks"></a>Sanal Ağlar

|  |  |
|---------|---------|
| [İzin verilen Application Gateway SKU’ları](../articles/governance/policy/samples/allowed-app-gateway-sku.md) | Uygulama ağ geçitlerinin onaylı bir SKU kullanmasını gerektirir. Onaylı bir SKU dizisi belirtirsiniz. |
| [İzin verilen vNet Ağ Geçidi SKU'ları](../articles/governance/policy/samples/allowed-vnet-gateway-sku.md) | Sanal ağ geçitlerinin onaylı bir SKU kullanmasını gerektirir. Onaylı bir SKU dizisi belirtirsiniz. |
| [İzin verilen Load Balancer SKU'ları](../articles/governance/policy/samples/allowed-load-balancer-skus.md) | Sanal ağ yük dengeleyicilerin onaylı bir SKU kullanmasını gerektirir. Onaylı bir SKU dizisi belirtirsiniz. |
| [Express Route ağına hiç ağ eşlemesi yok](../articles/governance/policy/samples/no-peering-express-route-network.md) | Bir ağ eşlemesinin belirli bir kaynak grubundaki bir ağ ile ilişkilendirilmesini önler. Merkezi yönetilen ağ altyapısı ile bağlantıyı engellemek için kullanın. İlişkilendirmeyi önlemek için kaynak grubunun adını belirtirsiniz. |
| [Kullanıcı Tanımlı Yol Tablosu Yok](../articles/governance/policy/samples/no-user-defined-route-table.md)  | Sanal ağların kullanıcı tanımlı bir yönlendirme tablosu ile dağıtılmasını engeller. |
| [Her alt ağ üzerinde NSG X](../articles/governance/policy/samples/nsg-on-subnet.md) | Her sanal alt ağ ile belirli bir ağ güvenlik grubunun kullanılmasını gerektirir. Kullanılacak ağ güvenlik grubunun kimliğini belirtirsiniz. |
| [VM ağ arabirimlerinde onaylanan alt ağı kullan](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) | Ağ arabirimlerinin onaylı bir alt ağ kullanmasını gerektirir. Onaylanan alt ağın kimliğini belirtirsiniz. |
| [VM ağ arabirimlerinde onaylanan sanal ağı kullan](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) | Ağ arabirimlerinin onaylı bir sanal ağ kullanmasını gerektirir. Onaylanan sanal ağın kimliğini belirtirsiniz. |