---
title: Azure PowerShell Betik Örneği - Bir yönetilen diskin anlık görüntüsünü aynı veya farklı aboneliğe kopyalama (taşıma) | Microsoft Docs
description: Azure PowerShell Betik Örneği - Bir yönetilen diskin anlık görüntüsünü aynı veya farklı aboneliğe kopyalama (taşıma)
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: aadcdc679d8d0df82c7ddc41b4dcdd6766bb7426
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2019
ms.locfileid: "57248741"
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>PowerShell ile bir yönetilen diskin anlık görüntüsünü aynı aboneliğe veya farklı aboneliğe kopyalama

Bu betik bir yönetilen diskin anlık görüntüsünü aynı veya farklı bir aboneliğe kopyalar. Aşağıdaki senaryolar için bu betiği kullanın:

1. Premium depolama (Premium_LRS) bir anlık görüntü (Standard_LRS veya Standard_ZRS) maliyetlerinizi azaltmak için standart Depolama'ya geçirin.
1. Anlık görüntü, bölgesel olarak yedekli depolama (ZRS depolama daha yüksek güvenilirliği yararlanmak için Standard_ZRS) için yerel olarak yedekli depolama alanından (Premium_LRS, Standard_LRS) geçirin.
1. Anlık görüntü, aynı bölgede daha uzun bekletme süresi için farklı bir aboneliğe taşıyın.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, kaynak anlık görüntünün kimliğini kullanarak hedef abonelikte bir anlık görüntü oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Yeni AzSnapshotConfig](https://docs.microsoft.com/powershell/module/az.compute/New-AzSnapshotConfig) | Anlık görüntü oluşturmak için kullanılan anlık görüntü yapılandırmasını oluşturur. Üst anlık görüntünün kaynak kimliğini ve üst anlık görüntünün konumuyla aynı olan konumu içerir.  |
| [Yeni AzSnapshot](https://docs.microsoft.com/powershell/module/az.compute/New-AzDisk) | Parametre olarak geçirilen anlık görüntü yapılandırmasını, anlık görüntü adını ve kaynak grubu adını kullanarak bir anlık görüntü oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

[Anlık görüntüden sanal makine oluşturma](./virtual-machines-linux-powershell-sample-create-vm-from-snapshot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek sanal makine PowerShell betiği örnekleri, [Azure Linux VM belgeleri](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.