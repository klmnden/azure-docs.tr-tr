---
title: Windows Azure Cloud Shell'de New-AzureRMVM cmdlet'i Basitleştirilmiş kullanarak VM oluşturma | Microsoft Docs
description: Azure Cloud shell'de Basitleştirilmiş New-AzureRMVM cmdlet'i ile Windows sanal makineler oluşturmak hızlı bir şekilde öğrenin.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: d078a9d319ea914c0cab9113ee71283213471261
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54428674"
---
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azurermvm-cmdlet-in-cloud-shell"></a>Cloud shell'de Basitleştirilmiş New-AzureRMVM cmdlet'i ile Windows sanal makine oluşturma 

[New-AzureRMVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm?view=azurermps-6.8.1) cmdlet'i, PowerShell kullanarak yeni bir VM oluşturmak için parametreler basitleştirilmiş bir dizi ekledi. Bu konuda, PowerShell'in Azure Cloud Shell'de önceden yeni bir VM oluşturmak için New-AzureVM cmdlet'ini en son sürümü ile nasıl kullanılacağı gösterilmektedir. Otomatik olarak akıllı varsayılanları kullanarak tüm gerekli kaynakları oluşturan Basitleştirilmiş parametre kümesi kullanacağız. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.1.1 veya sonraki bir sürümü gerektirir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzureRmAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-the-vm"></a>Sanal makine oluşturma

Kullanabileceğiniz [New-AzureRMVM](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvm?view=azurermps-6.8.1) dahil Azure Market'te Windows Server 2016 Datacenter görüntüsü kullanarak akıllı varsayılanlar ile bir VM oluşturmak için cmdlet'i. New-AzureRMVM ile kullanabileceğiniz yalnızca **-adı** parametresi ve kullanacağınız değeri tüm kaynak adları için. Bu örnekte, **-Name** parametresini *myVM* olarak ayarlayacağız. 

Cloud Shell’de **PowerShell**’in seçili olduğundan emin olun ve aşağıdakileri yazın:

```azurepowershell-interactive
New-AzureRMVm -Name myVM
```

VM için bir kullanıcı adı ve parola oluşturmanız istenir. Bu bilgiler bu konunun ilerleyen bölümlerinde VM’ye bağlanırken kullanılır. Parola 12-123 karakter uzunluğunda olmalıdır ve şu dört karmaşıklık gereksiniminden en az üçünü karşılamalıdır: Bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter.

VM’yi ve ilişkili kaynakları oluşturmak bir dakika sürer. İşiniz bittiğinde, kullanılarak oluşturulan tüm kaynakları görebilirsiniz [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) cmdlet'i.

```azurepowershell-interactive
Get-AzureRmResource `
    -ResourceGroupName myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

Dağıtım tamamlandıktan sonra sanal makine ile bir uzak masaüstü bağlantısı oluşturun.

Sanal makinenin genel IP adresini döndürmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) komutunu kullanın. Bu IP Adresini not alın.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Yerel makinenizde bir komut istemi açın ve kullanmak **mstsc** yeni VM'NİZLE bir Uzak Masaüstü oturumu başlatmak için komut. &lt;publicIPAddress&gt; ifadesini, sanal makinenizin IP adresiyle değiştirin. İstendiğinde, VM’niz oluşturulurken belirlediğiniz kullanıcı adını ve parolayı girin.

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>Farklı kaynak adlarını belirtin

Kaynakların daha açıklayıcı adlar da sağlar ve bunları otomatik olarak oluşturulan çözümlenmedi. Size yeni bir kaynak grubu da dahil olmak üzere yeni VM için birden fazla kaynak yeri Adlandırdığınız bir örnek aşağıdadır.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 3389
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVM
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu konu başlığında, New-AzVM kullanarak basit bir sanal makine dağıttınız ve RDP üzerinden bu makineye bağlandınız. Azure sanal makineleri hakkında daha fazla bilgi için Windows VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Windows sanal makine öğreticileri](./tutorial-manage-vm.md)
