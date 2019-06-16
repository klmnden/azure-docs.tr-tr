---
title: 'Azure VPN ağ geçidinde OpenVPN yapılandırmak nasıl: PowerShell | Microsoft Docs'
description: Azure VPN ağ geçidi için OpenVPN yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: cherylmc
ms.openlocfilehash: 609c2ef91fafe0ae955252a594292d861e772f87
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002955"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway"></a>OpenVPN için Azure noktadan siteye VPN ağ geçidi yapılandırma

Bu makalede, ayarladığınız yardımcı **OpenVPN® Protokolü** Azure VPN Gateway'de. Makale, bir çalışma noktadan siteye ortamı zaten sahip olduğunuzu varsayar. Bunu yapmazsanız, noktadan siteye VPN oluşturma için 1. adımda yönergeleri kullanın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="vnet"></a>1. Noktadan siteye VPN oluşturma

İşlevsel bir noktadan siteye ortam zaten yoksa, bir oluşturmaya yönelik yönergeyi uygulayın. Bkz: [noktadan siteye VPN oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md) oluşturmak ve yerel Azure sertifika kimlik doğrulaması ile noktadan siteye VPN ağ geçidi yapılandırmak için. 

> [!IMPORTANT]
> Temel SKU OpenVPN için desteklenmiyor.

## <a name="enable"></a>2. Ağ geçidi üzerinde OpenVPN etkinleştir

Ağ geçidinizde OpenVPN etkinleştirin. Ağ geçidi zaten noktadan siteye için (Ikev2 veya SSTP) aşağıdaki komutları çalıştırmadan önce yapılandırıldığından emin olun:

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Sonraki adımlar

OpenVPN ait istemcileri yapılandırmak için bkz: [yapılandırma OpenVPN istemciler](vpn-gateway-howto-openvpn-clients.md).

**"OpenVPN" OpenVPN Inc.'in ticari markasıdır.**
