---
title: Azure PowerShell Örnekleri - Uygulama yükleme | Microsoft Docs
description: Azure PowerShell Örnekleri
services: virtual-machine-scale-sets
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: zarhoads
ms.custom: mvc
ms.openlocfilehash: 46d4b4c24de53de59645eeb18ce2c60c1eb6d517
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49468203"
---
# <a name="install-applications-into-a-virtual-machine-scale-set-with-powershell"></a>PowerShell ile sanal makine ölçek kümesine uygulama yükleme
Bu betik Windows Server 2016 çalıştıran bir sanal makine ölçek kümesi oluşturur ve Özel Betik Uzantısı kullanarak temel bir web uygulaması yükler. Betiği çalıştırdıktan sonra web uygulamasına web tarayıcısı üzerinden erişebilirsiniz.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/install-apps/install-apps.ps1 "Install apps into a scale set")]

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
|  [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |

## <a name="next-steps"></a>Sonraki adımlar
Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine ölçek kümesi PowerShell betik örnekleri, [Azure sanal makine ölçek kümesi belgelerinde](../powershell-samples.md) bulunabilir.