---
title: 'Azure VPN ağ geçidinde OpenVPN yapılandırmak nasıl: PowerShell | Microsoft Docs'
description: Azure VPN ağ geçidi için OpenVPN yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: cherylmc
ms.openlocfilehash: 8cc2a6d4ad06bf4a55d4ef078970823df1e0d910
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59281973"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway-preview"></a>OpenVPN Azure noktadan siteye VPN Gateway (Önizleme) için yapılandırma

Bu makalede, ayarladığınız yardımcı **OpenVPN® Protokolü** Azure VPN Gateway'de. Makale, bir çalışma noktadan siteye ortamı zaten sahip olduğunuzu varsayar. Bunu yapmazsanız, noktadan siteye VPN oluşturma için 1. adımda yönergeleri kullanın.

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="register"></a>Bu özelliği kaydedin

Tıklayın **TryIt** kolayca Azure Cloud Shell kullanarak bu özelliği kaydetmek için aşağıdaki adımları.

>[!NOTE]
>Bu özellik kayıt ettirmezseniz, kullanılacağını mümkün olmayacaktır.
>

Azure Cloud Shell'i açmak için **TryIt** ifadesine tıkladıktan sonra aşağıdaki komutları kopyalayıp yapıştırın:

```azurepowershell-interactive
Register-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```
 
```azurepowershell-interactive
Get-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Özellik kayıtlı olarak göründükten sonra aboneliği Microsoft.Network ad alanına yeniden kaydedin.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

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