---
title: Azure PowerShell Örnekleri - Alanlar arası yedekli ölçek kümesi | Microsoft Docs
description: Azure PowerShell Örnekleri
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 6a5ebc823be38036082a9e0052e01ff46416c8d3
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54888303"
---
# <a name="create-a-zone-redundant-virtual-machine-scale-set-with-powershell"></a>PowerShell ile alanlar arası yedekli sanal makine ölçek kümesi oluşturma
Bu betik, birden çok Kullanılabilirlik Alanında Windows Server 2016 çalıştıran bir sanal makine ölçek kümesi oluşturur. Betiği çalıştırdıktan sonra sanal makineye RDP üzerinden erişebilirsiniz.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.ps1 "Create zone-redundant scale set")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Kaynak grubunu, ölçek kümesini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) | Sanal makine ölçek kümesi ve sanal ağ, yük dengeleyici ve NAT kurallarının dahil olduğu tüm destekleyici kaynakları oluşturur. |
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Sanal makine ölçek kümesi hakkında bilgi alır. |
| [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) | Özel Betik için sanal makine uzantısı ekleyerek temel bir web uygulaması yükler. |
| [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) | Sanal makine uzantısını uygulamak için sanal makine ölçek kümesi modelini güncelleştirir. |
| [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) | Yük dengeleyici tarafından kullanılan atanan genel IP adresi hakkında bilgi alır. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |


## <a name="next-steps"></a>Sonraki adımlar
Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine ölçek kümesi PowerShell betik örnekleri, [Azure sanal makine ölçek kümesi belgelerinde](../powershell-samples.md) bulunabilir.