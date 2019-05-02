---
title: Azure PowerShell betiği örneği - İki sanal ağı eşleme | Microsoft Docs
description: Azure PowerShell betiği örneği - İki sanal ağı eşleme
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: kumud
ms.openlocfilehash: 9f75d1078926772d71e10fe59e2759656aec9ab9
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64710223"
---
# <a name="peer-two-virtual-networks-script-sample"></a>İki sanal ağı eşleme betiği örneği

Bu betik, Azure ağı aracılığıyla aynı bölgede iki sanal ağ oluşturur ve bu sanal ağları birbirine bağlar. Betiği çalıştırdıktan sonra iki sanal ağ arasında eşleme oluşturmuş olursunuz.

Azure [Cloud Shell](https://shell.azure.com/powershell)’den veya yerel bir PowerShell yüklemesinden betiği yürütebilirsiniz. PowerShell'i yerel olarak kullanıyorsanız bu betik Az PowerShell modülünün 5.4.1 gerektirir veya üzeri. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, sanal makineyi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın:

```powershell
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Aşağıdaki tabloda yer alan her komut, komuta özgü belgelere yönlendirir:

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)| Bir Azure sanal ağı ve alt ağ oluşturur. |
| [AzVirtualNetworkPeering ekleyin](/powershell/module/az.network/add-azvirtualnetworkpeering) | İki sanal ağ arasında eşleme oluşturur.  |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal ağ PowerShell betiği örnekleri, [Sanal ağ PowerShell örnekleri](../powershell-samples.md) bölümünde bulunabilir.