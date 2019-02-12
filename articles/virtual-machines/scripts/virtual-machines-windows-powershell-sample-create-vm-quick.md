---
title: Azure PowerShell Betiği Örneği - Windows Sanal Makinesi Oluşturma | Microsoft Docs
description: Azure PowerShell Betiği Örneği - Windows Sanal Makinesi Oluşturma
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: f2008a795addb035cf3f19606170dbac5ffd1a5b
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "55993393"
---
# <a name="create-a-virtual-machine-with-powershell"></a>PowerShell ile sanal makine oluşturma

Bu betik Windows Server 2016 çalıştıran bir Azure Sanal Makinesi oluşturur. Betiği çalıştırdıktan sonra sanal makineye RDP üzerinden erişebilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-detailed/create-windows-vm-quick.ps1 "Create VM")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) | Sanal makine oluşturur ve ağ kartına, sanal ağa, alt ağa ve ağ güvenlik grubuna bağlar. Bu komut ayrıca 80 numaralı bağlantı noktasını açar ve yönetici kimlik bilgilerini ayarlar. |
|[Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.
