---
title: Azure PowerShell Betiği örnek - aynı veya farklı bir abonelik yönetilen bir diske kopyalama (Taşı) görüntüsünü | Microsoft Docs
description: Azure PowerShell Betiği örnek - aynı veya farklı bir abonelik yönetilen bir diske görüntüsünü kopyala (taşıma)
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
ms.openlocfilehash: 064b5355da10fe683563fa078cfafc65080f7ea2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23879662"
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>Aynı abonelik ya da farklı bir abonelik PowerShell ile yönetilen bir disk görüntüsünü Kopyala

Bu komut dosyasının bir kopyasını bir anlık görüntü aynı aynı abonelik ya da farklı bir abonelik oluşturur. Veri saklama için farklı bir abonelik için bir anlık görüntü taşımak için bu komut dosyasını kullanın. Farklı Abonelikteki depolama anlık görüntüler, anlık görüntü ana aboneliğinizde yanlışlıkla silinmeye karşı koruyun. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası kaynağı anlık görüntü kimliğini kullanarak hedef abonelikte bir anlık görüntü oluşturmak için komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [AzureRmSnapshotConfig yeni](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | Anlık görüntü oluşturmak için kullanılan anlık görüntü yapılandırması oluşturur. Kaynak Kimliği üst anlık görüntü ve üst anlık görüntü olarak aynı konumu içerir.  |
| [AzureRmSnapshot yeni](/powershell/module/azurerm.compute/New-AzureRmDisk) | Anlık görüntü yapılandırması, anlık görüntü adı ve parametre olarak geçirilen kaynak grubu adı kullanarak bir anlık görüntüsü oluşturur. |


## <a name="next-steps"></a>Sonraki adımlar

[Bir sanal makine bir anlık görüntüden oluşturun](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Ek sanal makine PowerShell komut dosyası örnekleri bulunabilir [Azure Windows VM belgelerine](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).