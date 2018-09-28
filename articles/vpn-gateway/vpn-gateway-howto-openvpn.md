---
title: 'Azure VPN ağ geçidinde OpenVPN yapılandırma: PowerShell | Microsoft Docs'
description: Azure VPN ağ geçidi için OpenVPN yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: cherylmc
ms.openlocfilehash: 958f4f46ec6ba407df7c739b7c62aa1489458485
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408285"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway-preview"></a>OpenVPN Azure noktadan siteye VPN Gateway (Önizleme) için yapılandırma

Bu makalede Azure VPN Gateway'de OpenVPN ayarlamanıza yardımcı olur. Makale, bir çalışma noktadan siteye ortamı zaten sahip olduğunuzu varsayar. Bunu yapmazsanız, noktadan siteye VPN oluşturma için 1. adımda yönergeleri kullanın.

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="register"></a>Bu özelliği Kaydet

Tıklayın **TryIt** kolayca Azure Cloud Shell kullanarak bu özelliği kaydetmek için aşağıdaki adımları.

>[!NOTE]
>Bu özellik kayıt ettirmezseniz, kullanılacağını mümkün olmayacaktır.
>

' I tıklattıktan sonra **TryIt** Azure Cloud Shell'i açmak için kopyalayın ve aşağıdaki komutları yapıştırın:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```
 
```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Özellik kayıtlı gösterir sonra aboneliği Microsoft.Network ad alanına yeniden kaydettirin.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="vnet"></a>1. Noktadan siteye VPN oluşturma

İşlevsel bir noktadan siteye ortam zaten yoksa, bir oluşturmaya yönelik yönergeyi uygulayın. Bkz: [noktadan siteye VPN oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md) oluşturmak ve yerel Azure sertifika kimlik doğrulaması ile noktadan siteye VPN ağ geçidi yapılandırmak için.

## <a name="cmdlets"></a>2. PowerShell cmdlet'lerini yükleme

Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyin. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview). Bu önemlidir, çünkü cmdlet’lerin daha önceki sürümleri bu alıştırma için gereken geçerli değerleri içermez.

## <a name="enable"></a>3. Ağ geçidi üzerinde OpenVPN etkinleştir

Ağ geçidinizde OpenVPN etkinleştirin. Ağ geçidi zaten noktadan siteye için (Ikev2 veya SSTP) aşağıdaki komutları çalıştırmadan önce yapılandırıldığından emin olun:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Sonraki adımlar

OpenVPN ait istemcileri yapılandırmak için bkz: [yapılandırma OpenVPN istemciler](vpn-gateway-howto-openvpn-clients.md).