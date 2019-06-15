---
title: Yerel ağ geçidi IP adresi ön eklerini ve VPN ağ geçidi IP adresini değiştirme | Azure | PowerShell | Microsoft Docs
description: Bu makalede, PowerShell kullanarak yerel ağ geçidinizin IP adresi ön eklerini değiştirme aracılığıyla gösterilmektedir.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 8c7db48f-d09a-44e7-836f-1fb6930389df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: 923c478909e0393f36da06802023b3f40ee63a3d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64723888"
---
# <a name="modify-local-network-gateway-settings-using-powershell"></a>PowerShell kullanarak yerel ağ geçidi ayarlarını değiştirme

Bazen yerel ağ geçidinizin AddressPrefix veya Gatewayıpaddress ayarlarını değiştirin. Bu makalede, yerel ağ geçidi ayarlarını değiştirme gösterilmektedir. Ayrıca aşağıdaki listeden farklı bir seçenek belirleyerek, farklı bir yöntem kullanarak bu ayarları değiştirebilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before"></a>Başlamadan önce

Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azureps-cmdlets-docs).

## <a name="ipaddprefix"></a>IP adresi ön eklerini değiştirme

[!INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="gwip"></a>Ağ geçidi IP adresini değiştirme

[!INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi bağlantınızı doğrulayabilirsiniz. Bkz: [bir ağ geçidi bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md).