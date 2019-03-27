---
title: Azure Cloud Shell'de Basitleştirilmiş New-AzVM cmdlet'ini kullanarak Windows VM oluşturma | Microsoft Docs
description: Azure Cloud shell'de Basitleştirilmiş New-AzVM cmdlet'i ile Windows sanal makineler oluşturmak hızlı bir şekilde öğrenin.
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
ms.openlocfilehash: 02f2bd78ca5656534b106c6f7f18c05165b4b9ff
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58444492"
---
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azvm-cmdlet-in-cloud-shell"></a>Cloud shell'de Basitleştirilmiş New-AzVM cmdlet'i ile Windows sanal makine oluşturma 

[New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet'i, PowerShell kullanarak yeni bir VM oluşturmak için parametreler basitleştirilmiş bir dizi ekledi. Bu konuda, PowerShell'in Azure Cloud Shell'de önceden yeni bir VM oluşturmak için New-AzureVM cmdlet'ini en son sürümü ile nasıl kullanılacağı gösterilmektedir. Otomatik olarak akıllı varsayılanları kullanarak tüm gerekli kaynakları oluşturan Basitleştirilmiş parametre kümesi kullanacağız. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]


## <a name="create-the-vm"></a>Sanal makine oluşturma

Azure Market’ten Windows Server 2016 Veri Merkezi görüntüsünü kullanma dahil olmak üzere akıllı varsayılan ayarlara sahip bir VM oluşturmak için [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet’ini kullanabilirsiniz. New-AzVM ile kullanabileceğiniz yalnızca **-adı** parametresi ve kullanacağınız değeri tüm kaynak adları için. Bu örnekte, **-Name** parametresini *myVM* olarak ayarlayacağız. 

Cloud Shell’de **PowerShell**’in seçili olduğundan emin olun ve aşağıdakileri yazın:

```azurepowershell-interactive
New-AzVm -Name myVM
```

VM için bir kullanıcı adı ve parola oluşturmanız istenir. Bu bilgiler bu konunun ilerleyen bölümlerinde VM’ye bağlanırken kullanılır. Parola 12-123 karakter uzunluğunda olmalıdır ve şu dört karmaşıklık gereksiniminden en az üçünü karşılamalıdır: Bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter.

VM’yi ve ilişkili kaynakları oluşturmak bir dakika sürer. İşiniz bittiğinde, kullanılarak oluşturulan tüm kaynakları görebilirsiniz [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) cmdlet'i.

```azurepowershell-interactive
Get-AzResource `
    -ResourceGroupName myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

Dağıtım tamamlandıktan sonra sanal makine ile bir uzak masaüstü bağlantısı oluşturun.

Kullanım [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) sanal makinenin genel IP adresini döndürmek için komutu. Bu IP Adresini not alın.

```azurepowershell-interactive
Get-AzPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Yerel makinenizde bir komut istemi açın ve kullanmak **mstsc** yeni VM'NİZLE bir Uzak Masaüstü oturumu başlatmak için komut. &lt;publicIPAddress&gt; ifadesini, sanal makinenizin IP adresiyle değiştirin. İstendiğinde, VM’niz oluşturulurken belirlediğiniz kullanıcı adını ve parolayı girin.

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>Farklı kaynak adlarını belirtin

Kaynakların daha açıklayıcı adlar da sağlar ve bunları otomatik olarak oluşturulan çözümlenmedi. Size yeni bir kaynak grubu da dahil olmak üzere yeni VM için birden fazla kaynak yeri Adlandırdığınız bir örnek aşağıdadır.

```azurepowershell-interactive
New-AzVm `
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

Artık gerekli değilse [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) komutunu kaynak grubunu, VM'yi ve tüm ilgili kaynakları.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myVM
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu konu başlığında, New-AzVM kullanarak basit bir sanal makine dağıttınız ve RDP üzerinden bu makineye bağlandınız. Azure sanal makineleri hakkında daha fazla bilgi için Windows VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Windows sanal makine öğreticileri](./tutorial-manage-vm.md)
