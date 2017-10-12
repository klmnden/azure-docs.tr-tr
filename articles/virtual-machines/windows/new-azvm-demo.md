---
title: "Azure Cloud Shell’de New-AzVM cmdlet’ini kullanarak Windows VM oluşturma| Microsoft Docs"
description: "Azure Cloud Shell’de New-AzVM cmdlet’i ile Windows sanal makinesi oluşturmayı hızlı bir şekilde öğrenin."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/21/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 3be46c8c02ad136edb1936fbb39560d479b27277
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-windows-virtual-machine-with-the-new-azvm-preview-in-cloud-shell"></a>Cloud Shell’de New-AzVM (Önizleme) ile Windows sanal makinesi oluşturma 

New-AzVM (Önizleme) cmdlet’i, PowerShell kullanarak yeni bir VM oluşturmanın basitleştirilmiş bir yoludur. Bu kılavuzda, Windows Server 2016 çalıştıran yeni bir Azure sanal makinesi oluşturmak için New-AzVM cmdlet’i önceden yüklenerek Azure Cloud Shell içinde PowerShell kullanma hakkında ayrıntılı bilgiler sağlanmaktadır. Dağıtım tamamlandıktan sonra RDP kullanılarak sunucuya bağlanılır.  

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]

## <a name="create-the-vm"></a>Sanal makine oluşturma

Azure Market’ten Windows Server 2016 Veri Merkezi görüntüsünü kullanma dahil olmak üzere akıllı varsayılan ayarlara sahip bir VM oluşturmak için **New-AzVM** cmdlet’ini kullanabilirsiniz. New-AzVM’yi tek başına kullanırsanız, kaynak adları için varsayılan değerler kullanılır. Bu örnekte, **-Name** parametresini *myVM* olarak ayarlayacağız. Cmdlet kaynak adı için ön ek olarak *myVM* değerini kullanarak gerekli tüm kaynakları oluşturur. 

Cloud Shell’de **PowerShell**’in seçili olduğundan emin olun ve aşağıdakileri yazın:

```azurepowershell-interactive
New-AzVm -Name myVM
```

VM için bir kullanıcı adı ve parola oluşturmanız istenir. Bu bilgiler bu konunun ilerleyen bölümlerinde VM’ye bağlanırken kullanılır. Parola 12-123 karakter uzunluğunda olmalıdır ve şu dört karmaşıklık gereksiniminden en az üçünü karşılamalıdır: Bir küçük harf karakter, bir büyük harf karakter, bir sayı ve bir özel karakter.

VM’yi ve ilişkili kaynakları oluşturmak bir dakika sürer. Tamamlandığında, [Find-AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource) cmdlet’ini kullanarak oluşturulan tüm kaynakları görebilirsiniz.

```azurepowershell-interactive
Find-AzureRmResource `
    -ResourceGroupNameEquals myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

Dağıtım tamamlandıktan sonra sanal makine ile bir uzak masaüstü bağlantısı oluşturun.

Sanal makinenin genel IP adresini döndürmek için [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) komutunu kullanın. Bu IP Adresini not alın.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Yerel makinenizde, yeni VM’nizle bir uzak masaüstü oturumu başlatmak için bir komut istemi açın ve **mstsc** komutunu kullanın. &lt;publicIPAddress&gt; ifadesini, sanal makinenizin IP adresiyle değiştirin. İstendiğinde, VM’niz oluşturulurken belirlediğiniz kullanıcı adını ve parolayı girin.

```
mstsc /v:<publicIpAddress>
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) komutunu kullanarak kaynak grubunu, VM’yi ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVMResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu konu başlığında, New-AzVM kullanarak basit bir sanal makine dağıttınız ve RDP üzerinden bu makineye bağlandınız. Azure sanal makineleri hakkında daha fazla bilgi için Windows VM’lerine yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Windows sanal makine öğreticileri](./tutorial-manage-vm.md)
