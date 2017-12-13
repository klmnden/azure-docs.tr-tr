---
title: "Azure PowerShell Betiği örnek - OMS | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - OMS"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2f9303568838113335343a420913b8dcb84cb49c
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2017
---
# <a name="create-an-operations-management-suite-monitored-vm-with-powershell"></a>Oluşturma VM PowerShell ile bir Operations Management Suite izlenen

Bu komut dosyasını bir Azure sanal makinesi oluşturur, Operations Management Suite (OMS) Aracısı'nı yükler ve bir OMS çalışma sistemiyle kaydeder. Betik çalıştıktan sonra sanal makine OMS konsolunda görünür. Ayrıca, OMS çalışma alanı kimliği ve çalışma alanı anahtarı güncelleştirmeniz gerekir.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-detailed-oms.ps1 "Create VM OMS")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası dağıtımı oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her öğe.

| Komut | Notlar |
|---|---|
| [Yeni-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Sanal makine oluşturur ve ağ kartı, sanal ağ, alt ağ ve ağ güvenlik grubu bağlanır. Bu komut ayrıca 80 numaralı bağlantı noktasını açar ve yönetici kimlik bilgilerini ayarlar. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | VM uzantısı sanal makineye ekleyin. Bu durumda, Operations Management Suite Aracı Uzantısı OMS Aracısı'nı yüklemek ve bir OMS çalışma alanında VM kaydetmek için kullanılır. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubu ve içerdiği tüm kaynaklar kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek sanal makine PowerShell komut dosyası örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
