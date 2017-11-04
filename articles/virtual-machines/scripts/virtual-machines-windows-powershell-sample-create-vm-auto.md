---
title: "Azure'da yeni AzVM kullanarak bir Windows VM oluşturma | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - yeni AzVM cmdlet'ini kullanarak bir Windows VM oluşturma."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/29/2017
ms.author: cynthn
ms.openlocfilehash: 36660ea1e8609ea8a03f4fa63e6f12f765bf9721
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-fully-configured-virtual-machine-with-the-new-azvm-powershell-cmdlet-preview"></a>Yeni AzVM PowerShell cmdlet'iyle (Önizleme) tam olarak yapılandırılmış bir sanal makine oluşturun

Bu komut dosyasını Windows Server 2016 çalıştıran bir Azure sanal makine oluşturmak için bulut Kabuğu'nda yeni AzVM cmdlet kullanır. Betiği çalıştırdıktan sonra sanal makine üzerinde RDP erişebilir. Bulut Kabuk yeni AzVM yeni işlevselliği denetlemek kolaylaştırır varsayılan olarak yüklenen içeren modülü vardır.

Bu örnek önizlemede yeni AzVM cmdlet'ini kullanır. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

## <a name="sample-script"></a>Örnek komut dosyası


```azurepowershell-interactive
New-AzVM -Name myVM `
    -VirtualNetworkName myVNet `
    -Location westeurope `
    -SecurityGroupName myNSG `
    -PublicIpAddressName myPIP 
```


## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVMResourceGroup
```


## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek sanal makine PowerShell komut dosyası örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
