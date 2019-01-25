---
title: Azure PowerShell Betiği Örneği - IIS | Microsoft Docs
description: Azure PowerShell Betiği Örneği - IIS
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
ms.openlocfilehash: 6f9322be5ed583435ef0e0396832139f7e77e318
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54882473"
---
# <a name="create-an-iis-vm-with-powershell"></a>PowerShell ile IIS sanal makinesi oluşturma

Bu betik Windows Server 2016 çalıştıran bir Azure Sanal Makinesi oluşturur ve Azure Sanal Makinesi Özel Betik Uzantısını kullanarak IIS yükler. Betiği çalıştırdıktan sonra sanal makinenin genel IP adresi üzerindeki varsayılan IIS web sitesine erişebilirsiniz.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Sanal makine oluşturur ve ağ kartına, sanal ağa, alt ağa ve ağ güvenlik grubuna bağlar. Bu komut ayrıca 80 numaralı bağlantı noktasını açar ve yönetici kimlik bilgilerini ayarlar. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | Sanal makineye bir VM uzantısı ekleyin. Bu örnekte IIS yüklemek için özel betik uzantısı kullanılır. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.
