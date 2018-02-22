---
title: "Azure’da Windows VM özelleştirme | Microsoft Docs"
description: "Azure’da Windows VM’leri üzerinde uygulama yüklemelerini otomatikleştirmek için özel betik uzantısını kullanmayı öğrenin"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 63858da0a4a47d67ec659e922ab10f9f7bc97938
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="how-to-customize-a-windows-virtual-machine-in-azure"></a>Azure'da Windows sanal makinesini özelleştirme
Sanal makineleri (VM’ler) hızlı ve tutarlı bir şekilde yapılandırmak için genellikle bir otomasyon biçimi istenir. [Windows için Özel Betik Uzantısı](extensions-customscript.md) kullanarak Windows VM özelleştirmek yaygın bir yaklaşımdır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * IIS yüklemek için Özel Betik Uzantısı kullanma
> * Özel Betik Uzantısı kullanan bir VM oluşturma
> * Uzantı uygulandıktan sonra çalışan bir IIS sitesi görüntüleme

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.3 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir. 


## <a name="custom-script-extension-overview"></a>Özel betik uzantısına genel bakış
Özel Betik Uzantısı, Azure VM’lerinde betik indirir ve yürütür. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir.

Özel Betik uzantısı, Azure Resource Manager şablonları ile tümleşir ve Azure CLI, PowerShell, Azure portalı veya Azure Sanal Makine REST API'si kullanılarak da çalıştırılabilir.

Özel Betik Uzantısı’nı, Windows ve Linux VM'ler ile kullanabilirsiniz.


## <a name="create-virtual-machine"></a>Sanal makine oluşturma
İlk olarak, VM için [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) ile bir yönetici kullanıcı adı ve parola ayarlayın:

```azurepowershell-interactive
$cred = Get-Credential
```

Artık [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) ile VM’yi oluşturabilirsiniz. Aşağıdaki örnekte *EastUS* konumunda *myVM* adlı bir VM oluşturulur. Zaten mevcut değilse, *myResourceGroupAutomate* kaynak grubu ve destekleyici ağ kaynakları oluşturulur. Web trafiğine izin vermek için, cmdlet ayrıca *80* numaralı bağlantı noktasını açar.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupAutomate" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80 `
    -Credential $cred
```

Kaynakların ve sanal makinenin oluşturulması birkaç dakika sürer.


## <a name="automate-iis-install"></a>IIS yüklemeyi otomatikleştirme
Özel Betik Uzantısı’nı yüklemek için [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) komutunu kullanın. Uzantı, IIS web sunucusunu yüklemek için `powershell Add-WindowsFeature Web-Server` komutunu çalıştırır ve ardından VM’nin ana bilgisayar adını göstermek için *Default.htm* sayfasını güncelleştirir:

```azurepowershell-interactive
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroupAutomate" `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location "EastUS" `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}'
```


## <a name="test-web-site"></a>Web sitesini test etme
[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) ile yük dengeleyicinizin genel IP adresini alın. Aşağıdaki örnek, daha önce oluşturulan *myPublicIPAddress* için IP adresini alır:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
    -ResourceGroupName "myResourceGroupAutomate" `
    -Name "myPublicIPAddress" | select IpAddress
```

Sonra da genel IP adresini bir web tarayıcısına girebilirsiniz. Aşağıdaki örnekteki gibi yük dengeleyicinin trafiği dağıttığı VM’nin ana bilgisayar adının dahil olduğu web sitesi görüntülenir:

![Çalışan IIS web sitesi](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir VM’de IIS yüklemesini otomatikleştirdiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * IIS yüklemek için Özel Betik Uzantısı kullanma
> * Özel Betik Uzantısı kullanan bir VM oluşturma
> * Uzantı uygulandıktan sonra çalışan bir IIS sitesi görüntüleme

Özel VM görüntülerinin nasıl oluşturulacağını öğrenmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Özel VM görüntüleri oluşturma](./tutorial-custom-images.md)
