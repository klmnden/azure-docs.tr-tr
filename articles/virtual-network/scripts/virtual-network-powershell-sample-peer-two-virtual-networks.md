---
title: Azure PowerShell komut dosyası örneği - eş iki sanal ağlar | Microsoft Docs
description: Azure PowerShell komut dosyası örneği - eş iki sanal ağlar
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: c0efdf759a0bdb87de4dc8ff9566a8e817503c5e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="peer-two-virtual-networks"></a>Eş iki sanal ağlar

Bu komut dosyasını oluşturur ve iki sanal ağ aynı bölgede Azure ağ üzerinden bağlanır. Komut dosyasını çalıştırdıktan sonra iki sanal ağ arasında eşleme oluşturur.

Azure'dan komut dosyasını çalıştırabilir [bulut Kabuk](https://shell.azure.com/powershell), veya bir yerel PowerShell yüklemesinden. PowerShell'i yerel olarak kullanırsanız, bu komut dosyası AzureRM PowerShell modülü sürümü 5.4.1 gerektirir veya sonraki bir sürümü. Yüklü olan sürümü bulmak için Çalıştır `Get-Module -ListAvailable AzureRM`. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-azurerm-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Login-AzureRmAccount` komutunu da çalıştırmanız gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubu, VM ve tüm ilgili kaynaklar kaldırmak için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Aşağıdaki tabloda bağlantıları komutu belirli belgelere her komut:

| Komut | Notlar |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. | 
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)| Bir Azure sanal ağı ve alt ağ oluşturur. |
| [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering) | İki sanal ağ arasında eşleme oluşturur.  |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal ağ PowerShell komut dosyası örnekleri bulunabilir [sanal ağ PowerShell örnekleri](../powershell-samples.md).