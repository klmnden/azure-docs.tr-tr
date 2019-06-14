---
title: Yerel ağ geçidi IP adresi ön eklerini ve VPN ağ geçidi IP adresini değiştirme | Azure | CLI | Microsoft Docs
description: Bu makalede, Azure CLI kullanarak yerel ağ geçidinizin IP adresi ön eklerini değiştirme aracılığıyla gösterilmektedir.
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
ms.date: 11/29/2017
ms.author: cherylmc
ms.openlocfilehash: 20e8269f0ee7463dcff9a5f66e46ebd81cb3df84
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60652879"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a>Azure CLI kullanarak yerel ağ geçidi ayarlarını değiştirme

Bazen yerel ağ geçidi adres ön eki veya ağ geçidi IP adresi ayarlarını değiştirin. Bu makalede, yerel ağ geçidi ayarlarını değiştirme gösterilmektedir. Ayrıca aşağıdaki listeden farklı bir seçenek belirleyerek, farklı bir yöntem kullanarak bu ayarları değiştirebilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before"></a>Başlamadan önce

(2.0 veya üzeri) CLI komutlarının en son sürümünü yükleyin. CLI komutlarını yükleme hakkında bilgi için bkz. [Azure CLI’yi yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="ipaddprefix"></a>IP adresi ön eklerini değiştirme

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <a name="gwip"></a>Ağ geçidi IP adresini değiştirme

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi bağlantınızı doğrulayabilirsiniz. Bkz: [bir ağ geçidi bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md).

