---
title: Yerel ağ geçidi IP adresi ön eklerini ve VPN ağ geçidi IP adresini değiştirme | Azure | Portal | Microsoft Docs
description: Bu makalede Azure portalını kullanarak yerel ağ geçidinizin IP adresi ön eklerini değiştirme aracılığıyla gösterilmektedir.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: bdd6f90fe97408bd45a72adf58bfdc190207de46
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38651945"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Azure portalını kullanarak yerel ağ geçidi ayarlarını değiştirme

Bazen yerel ağ geçidinizin AddressPrefix veya Gatewayıpaddress ayarlarını değiştirin. Bu makalede, yerel ağ geçidi ayarlarını değiştirme gösterilmektedir. Ayrıca aşağıdaki listeden farklı bir seçenek belirleyerek, farklı bir yöntem kullanarak bu ayarları değiştirebilirsiniz:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure CLI](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="ipaddprefix"></a>IP adresi ön eklerini değiştirme

IP adresi öneklerini değiştirirken, izlediğiniz adımlar, yerel ağ geçidinizin bağlantı olup bağlıdır.

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="gwip"></a>Ağ geçidi IP adresini değiştirme

Bağlanmak istediğiniz VPN cihazının genel IP adresi değiştiyse, yerel ağ geçidini bu değişikliği yansıtacak şekilde değiştirmeniz gerekir. Genel IP adresini değiştirdiğinizde, izlediğiniz adımlar, yerel ağ geçidinizin bağlantı olup bağlıdır.

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi bağlantınızı doğrulayabilirsiniz. Bkz: [bir ağ geçidi bağlantısını doğrulama](vpn-gateway-verify-connection-resource-manager.md).