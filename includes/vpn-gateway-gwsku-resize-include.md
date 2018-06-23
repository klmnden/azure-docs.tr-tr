---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c06e69dd9d1997500589659e936dc25ee01ed145
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "30196824"
---
Geçerli için SKU'ları (VpnGw1, VpnGw2 ve VPNGW3), ağ geçidi daha güçlü bir yükseltmek için SKU yeniden boyutlandırmak istediğiniz kullanabileceğiniz `Resize-AzureRmVirtualNetworkGateway` PowerShell cmdlet'i. Ayrıca ağ geçidi Bu cmdlet'i kullanarak SKU boyutunu düşürmek. Temel ağ geçidi SKU'su, kullanıyorsanız [bu yönergeleri kullanmanız](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md#resize) ağ geçidiniz yeniden boyutlandırmak için.

Aşağıdaki PowerShell örnek bir ağ geçidi için VpnGw2 yeniden boyutlandırılan SKU gösterir.

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku VpnGw2
```

Azure portalında bir ağ geçidi giderek boyutlandırabilirsiniz **yapılandırma** sayfasında sanal ağ geçidiniz için ve farklı bir SKU aşağı açılır listeden seçerek.