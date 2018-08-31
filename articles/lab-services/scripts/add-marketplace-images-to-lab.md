---
title: "PowerShell Betiği: bir Market görüntüsü için Azure DevTest labs'deki bir laboratuvara ekleme | Microsoft Docs"
description: Bu PowerShell Betiği, Azure DevTest labs'deki bir laboratuvara Market görüntüsü ekler.
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
ms.openlocfilehash: a3a007bf19a28e6f361837856f83a191a761ef9b
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43247423"
---
# <a name="use-powershell-to-add-a-marketplace-image-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara Market görüntüsü eklemek için PowerShell'i kullanma

Bu örnek PowerShell Betiği, Azure DevTest labs'deki bir laboratuvara Market görüntüsü ekler. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Bir laboratuvar**. Komut dosyası var olan bir laboratuvar olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-marketplace-images-to-lab/add-marketplace-images-to-lab.ps1 "Add marketplace images to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Find-AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource) | Belirtilen parametrelere bağlı kaynakları arar. |
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Kaynakları alır. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Bir kaynağı değiştirir. |
| [New-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Bir kaynak oluşturun. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Lab Services PowerShell Betiği örnekleri, içinde bulunabilir [Azure Lab Services PowerShell örnekleri](../samples-powershell.md).
