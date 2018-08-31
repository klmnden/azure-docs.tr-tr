---
title: "PowerShell Betiği: dış kullanıcı Azure DevTest labs'deki bir laboratuvara ekleme | Microsoft Docs"
description: Bu PowerShell Betiği bir dış kullanıcı Azure DevTest labs'deki bir laboratuvara ekler.
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
ms.openlocfilehash: 0acff1eb4cee441187205b11a7e07cc718072cbf
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43248198"
---
# <a name="use-powershell-to-add-an-external-user-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara bir dış kullanıcı eklemek için PowerShell'i kullanma

Bu örnek PowerShell Betiği bir dış kullanıcı Azure DevTest labs'deki bir laboratuvara ekler. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Bir laboratuvar**. Komut dosyası var olan bir laboratuvar olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzureRmADUser](/powershell/module/azurerm.resources/get-azurermaduser) | Azure active Directory'den kullanıcı nesnesi yeniden dener. |
| [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) | Belirtilen kapsamda belirtilen sorumluyu belirtilen rolü atar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Lab Services PowerShell Betiği örnekleri, içinde bulunabilir [Azure Lab Services PowerShell örnekleri](../samples-powershell.md).
