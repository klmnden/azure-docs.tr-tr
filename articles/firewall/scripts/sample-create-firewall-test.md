---
title: Azure PowerShell betiği örneği - Azure Güvenlik Duvarı test ortamı oluşturma
description: Azure PowerShell betiği örneği - Azure Güvenlik Duvarı test ortamı oluşturun.
services: virtual-network
author: vhorne
ms.service: firewall
ms.devlang: powershell
ms.topic: sample
ms.date: 8/13/2018
ms.author: victorh
ms.openlocfilehash: 7f1986a9a59087d084577e980233ff87360a17e0
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49390117"
---
# <a name="create-an-azure-firewall-test-environment"></a>Azure Güvenlik Duvarı test ortamı oluşturma

Bu betik örneği, bir güvenlik duvarı ve bir test ağ ortamı oluşturur. Ağın bir Sanal Ağı ve üç alt ağı vardır: *AzureFirewallSubnet*, *ServersSubnet* ve *JumpboxSubnet*. ServersSubnet ve JumpboxSubnet'ten her birinin içinde bir tane 2 çekirdekli Windows Server bulunur.

Güvenlik duvarı AzureFirewallSubnet'in içindedir ve www.microsoft.com'a erişim izni veren tek kurallı bir Uygulama Kuralı Koleksiyonu ile yapılandırılmıştır.

Güvenlik duvarı kurallarının uygulandığı güvenlik duvarı üzerinden ServersSubnet'ten gelen ağ trafiğine işaret eden, kullanıcı tanımlı bir yol oluşturulur.

Azure [Cloud Shell](https://shell.azure.com/powershell)’den veya yerel bir PowerShell yüklemesinden betiği çalıştırabilirsiniz. 

PowerShell’i yerel olarak çalıştırıyorsanız, bu betik için güncel AzureRM PowerShell modülü sürümü (6.9.0 veya üstü) gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. 

Yükseltmeniz gerekiyorsa, Windows 10 ve Windows Server 2016'da yerleşik olarak sağlanan `PowerShellGet` komutunu kullanabilirsiniz.

> [!NOTE]
>Diğer Windows sürümleri, kullanabilmek için `PowerShellGet` yüklemenizi gerektirir. Sisteminizde yüklü olup olmadığını saptamak için `Get-Module -Name PowerShellGet -ListAvailable | Select-Object -Property Name,Version,Path` komutunu çalıştırabilirsiniz. Çıkış boşsa, en son [Windows Management Framework](https://www.microsoft.com/download/details.aspx?id=54616)'ü yüklemeniz gerekir.

Daha fazla bilgi için bkz. [PowerShellGet ile Windows'a Azure PowerShell yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.4.0)

Web Platformu yükleyicisiyle yapılmış mevcut Azure PowerShell yüklemeleri PowerShellGet yüklemesiyle çakışır ve kaldırılması gerekir.

PowerShell'i yerel olarak çalıştırırsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerektiğini unutmayın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik


[!code-azurepowershell-interactive[main](../../../powershell_scripts/firewall/create-fw-test.ps1  "Create a firewall test environment")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, sanal makineyi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzureRmResourceGroup -Name AzfwSampleScriptEastUS -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal ağ ve ağ güvenliği grupları oluşturmak için aşağıdaki komutları kullanır. Aşağıdaki tabloda yer alan her komut, komuta özgü belgelere yönlendirir:

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Bir alt ağ yapılandırma nesnesi oluşturur |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Bir Azure sanal ağı ve ön uç alt ağı oluşturur. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Bir ağ güvenlik grubuna atanacak güvenlik kuralları oluşturur. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |Belirli alt ağlara yönelik belirli bağlantı noktalarına izin veren veya engelleyen NSG kuralları oluşturur. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | NSG’leri alt ağlarla ilişkilendirir. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | İnternet’ten sanal makineye erişmek için genel IP adresi oluşturur. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Sanal ağ arabirimleri oluşturur ve bunları sanal ağın ön uç ve arka uç alt ağlarına ekler. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Sanal makine yapılandırması oluşturur. Bu yapılandırma; sanal makine adı, işletim sistemi ve yönetici kimlik bilgileri gibi bilgileri içerir. Yapılandırma, sanal makine oluşturulurken kullanılır. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Sanal makine oluşturur. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |
|[New-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermfirewall?view=azurermps-6.9.0)| Yeni bir Azure Güvenlik Duvarı oluşturur.|
|[Get-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermfirewall?view=azurermps-6.9.0)|Azure Güvenlik Duvarı nesnesini alır.|
|[New-AzureRmFirewallApplicationRule](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermfirewallapplicationrule?view=azurermps-6.9.0)|Yeni bir Azure Güvenlik Duvarı uygulama kuralı oluşturur.|
|[Set-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermfirewall?view=azurermps-6.9.0)|Değişiklikleri Azure Güvenlik Duvarı nesnesine işler.|


## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

