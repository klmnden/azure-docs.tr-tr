---
title: Azure PowerShell betik örneği - WAF özel kurallar oluşturma
description: Azure PowerShell betik örneği - özel Web uygulaması güvenlik duvarı kuralları oluşturma
author: vhorne
ms.service: application-gateway
ms.topic: sample
ms.date: 6/7/2019
ms.author: victorh
ms.openlocfilehash: ffdde80598322222e2a8f000eee8be269becdd11
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743426"
---
# <a name="create-waf-custom-rules-with-azure-powershell"></a>Azure PowerShell ile WAF özel kurallar oluşturma

Bu betik, bir Application Gateway Web uygulaması özel kurallar kullanan güvenlik duvarı oluşturur. User-Agent istek üst bilgisi içeriyorsa, özel kural bloklarını trafiği *evilbot*.

## <a name="prerequisites"></a>Önkoşullar

### <a name="azure-powershell-module"></a>Azure PowerShell modülü

Azure PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu betik Azure PowerShell modülü sürüm 2.1.0 gerektirir. veya üzeri.

1. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps).
2. Azure ile bağlantı oluşturmak için çalıştırın `Connect-AzAccount`.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/application-gateway/waf-rules/waf-custom-rules.ps1 "Custom WAF rules")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, uygulama ağ geçidini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name CustomRulesTest
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Alt ağ yapılandırmasını oluşturur. |
| [Yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Alt ağ yapılandırmalarıyla kullanarak sanal ağı oluşturur. |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Uygulama ağ geçidi için genel IP adresini oluşturur. |
| [Yeni AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration) | Bir alt ağı uygulama ağ geçidiyle ilişkilendiren yapılandırmayı oluşturur. |
| [Yeni AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig) | Uygulama ağ geçidine genel bir IP adresi atayan yapılandırmayı oluşturur. |
| [Yeni AzApplicationGatewayFrontendPort](/powershell/module/az.network/new-azapplicationgatewayfrontendport) | Uygulama ağ geçidine erişmek için kullanılacak bir bağlantı noktası atar. |
| [Yeni AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool) | Uygulama ağ geçidi için bir arka uç havuzu oluşturur. |
| [Yeni AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsetting) | Arka uç havuzu için ayarları yapılandırır. |
| [Yeni AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener) | Dinleyici oluşturur. |
| [Yeni AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule) | Yönlendirme kuralı oluşturur. |
| [Yeni AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku) | Uygulama ağ geçidi için katman ve kapasiteyi belirtin. |
| [Yeni AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway) | Uygulama ağ geçidi oluşturur. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |
|[Yeni AzApplicationGatewayAutoscaleConfiguration](/powershell/module/az.network/New-AzApplicationGatewayAutoscaleConfiguration)|Application Gateway için bir otomatik ölçeklendirme yapılandırması oluşturur.|
|[Yeni AzApplicationGatewayFirewallMatchVariable](/powershell/module/az.network/New-AzApplicationGatewayFirewallMatchVariable)|Güvenlik Duvarı koşul için bir eşleşme değişkeni oluşturur.|
|[Yeni AzApplicationGatewayFirewallCondition](/powershell/module/az.network/New-AzApplicationGatewayFirewallCondition)|Özel kural için bir eşleşme koşulu oluşturur.|
|[Yeni AzApplicationGatewayFirewallCustomRule](/powershell/module/az.network/New-AzApplicationGatewayFirewallCustomRule)|Application gateway güvenlik duvarı ilkesi için yeni bir özel kural oluşturur.|
|[Yeni AzApplicationGatewayFirewallPolicy](/powershell/module/az.network/New-AzApplicationGatewayFirewallPolicy)|Bir application gateway güvenlik duvarı ilkesi oluşturur.|
|[Yeni AzApplicationGatewayWebApplicationFirewallConfiguration](/powershell/module/az.network/New-AzApplicationGatewayWebApplicationFirewallConfiguration)|Bir application Gateway WAF yapılandırması oluşturur.|

## <a name="next-steps"></a>Sonraki adımlar

- WAF özel kurallar hakkında daha fazla bilgi için bkz: [Web uygulaması güvenlik duvarı için özel kurallar](../custom-waf-rules-overview.md)
- Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).
- Ek uygulama ağ geçidi PowerShell betiği örnekleri, [Azure Application Gateway belgeleri](../powershell-samples.md) içinde bulunabilir.
