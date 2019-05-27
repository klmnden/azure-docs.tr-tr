---
title: Azure PowerShell betik örneği - Cihaz yapılandırması şablonu indirme| Microsoft Docs
description: Cihaz yapılandırma şablonunu indirin.
services: vpn-gateway
documentationcenter: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.devlang: powershell
ms.topic: sample
ms.date: 04/17/2018
ms.author: alzam
ms.openlocfilehash: a4cec6661750935cefd3767be7e10e2674ee8473
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66113659"
---
# <a name="download-vpn-device-template-using-powershell"></a>PowerShell kullanarak VPN cihazı şablonunu indirme

Bu betik, bir bağlantı için VPN cihazı şablonunu indirir

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

```azurepowershell-interactive
# Declare variables
$RG          = "TestRG1"
$GWName      = "VNet1GW"
$Connection  = "VNet1toSite1"

# List the available VPN device models and versions
Get-AzVirtualNetworkGatewaySupportedVpnDevice -Name $GWName -ResourceGroupName $RG

# Download the configuration template for the connection
Get-AzVirtualNetworkGatewayConnectionVpnDeviceConfigScript -Name $Connection -ResourceGroupName $RG `
-DeviceVendor Juniper -DeviceFamily Juniper_SRX_GA -FirmwareVersion Juniper_SRX_12.x_GA
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz kaynaklara artık ihtiyacınız olduğunda kullanın [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) kaynak grubunu silmek için komutu. Böylece, kaynak grubu ve içerdiği tüm kaynaklar silinir.

```azurepowershell-interactive
Remove-AzResourceGroup -Name TestRG1
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzVirtualNetworkGatewaySupportedVpnDevice](/powershell/module/az.network/Get-azVirtualNetworkGatewaySupportedVpnDevice) | Kullanılabilir tüm VPN cihazı modelleri ve sürümlerini listeler. |
| [Get-AzVirtualNetworkGatewayConnectionVpnDeviceConfigScript](/powershell/module/az.network/Get-azVirtualNetworkGatewayConnectionVpnDeviceConfigScript) | Bağlantı için yapılandırma şablonunu indirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).
