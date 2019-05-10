---
title: Azure PowerShell Betiği Örneği - Web trafiğini kısıtlama | Microsoft Docs
description: Azure PowerShell Betik Örneği - Bir web uygulaması güvenlik duvarı ve trafiği kısıtlamak için OWASP kurallarını kullanan bir sanal makine ölçek kümesi ile bir uygulama ağ geçidi oluşturun.
services: application-gateway
documentationcenter: networking
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: d3dfb9708e22a86af8fb9854e424f7da7d1f410a
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65202866"
---
# <a name="restrict-web-traffic-using-azure-powershell"></a>Azure PowerShell kullanarak web trafiğini kısıtlama

Bu betik, arka uç sunucuları için bir sanal makine ölçek kümesi kullanan bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturur. Web uygulaması güvenlik duvarı, OWASP kurallarına göre web trafiğini kısıtlar. Betiği çalıştırdıktan sonra, genel IP adresini kullanarak uygulama ağ geçidini test edebilirsiniz.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/application-gateway/create-vmss/create-vmss-waf.ps1 "Create application gateway with WAF")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, uygulama ağ geçidini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroupAG
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
| [Yeni AzApplicationGatewayWebApplicationFirewallConfiguration](/powershell/module/az.network/new-azapplicationgatewaywebapplicationfirewallconfiguration) | Web uygulaması güvenlik duvarı yapılandırmasını oluşturur. |
| [Yeni AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway) | Uygulama ağ geçidi oluşturur. |
| [Set-AzVmssStorageProfile](/powershell/module/az.compute/set-azvmssstorageprofile) | Ölçek kümesi için bir depolama profili oluşturun. |
| [Set-AzVmssOsProfile](/powershell/module/az.compute/set-azvmssosprofile) | Ölçek kümesi için işletim sistemini tanımlayın. |
| [Add-AzVmssNetworkInterfaceConfiguration](/powershell/module/az.compute/add-azvmssnetworkinterfaceconfiguration) | Ölçek kümesi için ağ arabirimini tanımlayın. |
| [Yeni AzVmss](/powershell/module/az.compute/new-azvm) | Sanal makine ölçek kümesi oluşturun. |
| [Yeni AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Bir depolama hesabı oluşturur. |
| [Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting) | Veri kaydı için tanılamaları yapılandırır. |
| [Get-AzPublicIPAddress](/powershell/module/az.network/get-azpublicipaddress) | Uygulama ağ geçidinin genel IP adresini alır. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. | 
## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek uygulama ağ geçidi PowerShell betiği örnekleri, [Azure Application Gateway belgeleri](../powershell-samples.md) içinde bulunabilir.
