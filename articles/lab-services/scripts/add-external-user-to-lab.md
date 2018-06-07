---
title: 'PowerShell Betiği: Azure DevTest Labs laboratuarda bir dış kullanıcı ekleme | Microsoft Docs'
description: Bu PowerShell Betiği Azure DevTest Labs laboratuarda bir dış kullanıcı ekler.
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
ms.openlocfilehash: bd86e44c21ca3c0b9c061f9a5c24bd19c4b207f9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636452"
---
# <a name="use-powershell-to-add-an-external-user-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuarda bir dış kullanıcı eklemek için PowerShell kullanın

Bu örnek PowerShell komut dosyası Azure DevTest Labs laboratuarda bir dış kullanıcı ekler. 

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
| [New-AzureRmRoleAssignment](/module/azurerm.resources/new-azurermroleassignment) | Belirtilen kapsamda belirtilen asıl belirtilen rol atar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Laboratuvar Hizmetleri PowerShell komut dosyası örnekleri bulunabilir [Azure Laboratuvar Hizmetleri PowerShell örnekleri](../samples-powershell.md).