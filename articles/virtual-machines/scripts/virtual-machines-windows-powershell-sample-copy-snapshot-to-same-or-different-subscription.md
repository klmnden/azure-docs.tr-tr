---
title: Azure PowerShell Betik Örneği - Bir yönetilen diskin anlık görüntüsünü aynı veya farklı aboneliğe kopyalama (taşıma) | Microsoft Docs
description: Azure PowerShell Betik Örneği - Bir yönetilen diskin anlık görüntüsünü aynı veya farklı aboneliğe kopyalama (taşıma)
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 2905747b6ece93f018bd6ef67fae1b23ec9c9d5d
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "41920647"
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>PowerShell ile bir yönetilen diskin anlık görüntüsünü aynı aboneliğe veya farklı aboneliğe kopyalama

Bu betik, aynı abonelikte veya farklı bir abonelikte anlık görüntünün kopyasını oluşturur. Bu betiği kullanarak bir anlık görüntüyü veri saklama amacıyla farklı bir aboneliğe taşıyabilirsiniz. Anlık görüntülerin farklı bir abonelikte depolanması, ana aboneliğinizdeki anlık görüntülerin yanlışlıkla silinmesine karşı koruma sağlar. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kaynak anlık görüntünün kimliğini kullanarak hedef abonelikte bir anlık görüntü oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzureRmSnapshotConfig](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | Anlık görüntü oluşturmak için kullanılan anlık görüntü yapılandırmasını oluşturur. Üst anlık görüntünün kaynak kimliğini ve üst anlık görüntünün konumuyla aynı olan konumu içerir.  |
| [New-AzureRmSnapshot](/powershell/module/azurerm.compute/New-AzureRmDisk) | Parametre olarak geçirilen anlık görüntü yapılandırmasını, anlık görüntü adını ve kaynak grubu adını kullanarak bir anlık görüntü oluşturur. |


## <a name="next-steps"></a>Sonraki adımlar

[Anlık görüntüden sanal makine oluşturma](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Windows VM belgeleri](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.