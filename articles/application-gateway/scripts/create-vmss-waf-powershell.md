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
ms.openlocfilehash: d6f8083e0070435e6ad8bb0dc0ff520b58c7eead
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
ms.locfileid: "33202295"
---
# <a name="restrict-web-traffic-using-azure-powershell"></a>Azure PowerShell kullanarak web trafiğini kısıtlama

Bu betik, arka uç sunucuları için bir sanal makine ölçek kümesi kullanan bir web uygulaması güvenlik duvarı ile bir uygulama ağ geçidi oluşturur. Web uygulaması güvenlik duvarı, OWASP kurallarına göre web trafiğini kısıtlar. Betiği çalıştırdıktan sonra, genel IP adresini kullanarak uygulama ağ geçidini test edebilirsiniz.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/application-gateway/create-vmss/create-vmss-waf.ps1 "Create application gateway with WAF")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, uygulama ağ geçidini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroupAG
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Alt ağ yapılandırmasını oluşturur. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Alt ağ yapılandırmalarıyla kullanarak sanal ağı oluşturur. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Uygulama ağ geçidi için genel IP adresini oluşturur. |
| [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) | Bir alt ağı uygulama ağ geçidiyle ilişkilendiren yapılandırmayı oluşturur. |
| [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) | Uygulama ağ geçidine genel bir IP adresi atayan yapılandırmayı oluşturur. |
| [New-AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) | Uygulama ağ geçidine erişmek için kullanılacak bir bağlantı noktası atar. |
| [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) | Uygulama ağ geçidi için bir arka uç havuzu oluşturur. |
| [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings) | Bir arka uç havuzu için ayarları yapılandırır. |
| [New-AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) | Dinleyici oluşturur. |
| [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) | Yönlendirme kuralı oluşturur. |
| [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) | Uygulama ağ geçidi için katman ve kapasiteyi belirtin. |
| [New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewaywebapplicationfirewallconfiguration) | Web uygulaması güvenlik duvarı yapılandırmasını oluşturur. |
| [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway) | Uygulama ağ geçidi oluşturur. |
| [Set-AzureRmVmssStorageProfile](/powershell/module/azurerm.compute/set-azurermvmssstorageprofile) | Ölçek kümesi için bir depolama profili oluşturun. |
| [Set-AzureRmVmssOsProfile](/powershell/module/azurerm.compute/set-azurermvmssosprofile) | Ölçek kümesi için işletim sistemini tanımlayın. |
| [Add-AzureRmVmssNetworkInterfaceConfiguration](/powershell/module/azurerm.compute/add-azurermvmssnetworkinterfaceconfiguration) | Ölçek kümesi için ağ arabirimini tanımlayın. |
| [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm) | Sanal makine ölçek kümesi oluşturun. |
| [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) | Bir depolama hesabı oluşturur. |
| [Set-AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting) | Veri kaydı için tanılamaları yapılandırır. |
| [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) | Uygulama ağ geçidinin genel IP adresini alır. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. | 
## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek uygulama ağ geçidi PowerShell betiği örnekleri, [Azure Application Gateway belgeleri](../powershell-samples.md) içinde bulunabilir.
