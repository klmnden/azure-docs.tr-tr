---
title: 'PowerShell Betiği: Azure Laboratuvar Hizmetleri özel bir laboratuvarda bir Market görüntüsü eklemek | Microsoft Docs'
description: Bu PowerShell Betiği Azure Laboratuvar Hizmetleri özel bir laboratuvarda bir Market görüntüsü ekler.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: spelluru
ms.openlocfilehash: 64d168c132edce4ecd128b795fbfa5ab2607cb19
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="use-powershell-to-add-a-marketplace-image-to-a-custom-lab"></a>Bir Market görüntüsü için özel bir laboratuvar eklemek için PowerShell kullanın

Bu örnek PowerShell komut dosyası Azure Laboratuvar Hizmetleri özel bir laboratuvarda bir Market görüntüsü ekler.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Özel bir laboratuvar**. Komut dosyası var olan bir özel laboratuvarı olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-marketplace-images-to-lab/add-marketplace-images-to-lab.ps1 "Add marketplace images to a custom lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Bul AzureRmResource](/module/azurerm.resources/find-azurermresource) | Belirtilen parametrelere göre kaynakları arar. |
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Kaynakları alır. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Bir kaynak değiştirir. |
| [New-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Bir kaynağı oluşturun. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Laboratuvar Hizmetleri PowerShell komut dosyası örnekleri bulunabilir [Azure Laboratuvar Hizmetleri PowerShell örnekleri](../samples-powershell.md).