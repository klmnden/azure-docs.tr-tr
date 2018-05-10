---
title: 'PowerShell Betiği: Azure Laboratuvar Hizmetleri özel bir laboratuvarda bir dış kullanıcı ekleme | Microsoft Docs'
description: Bu PowerShell Betiği Azure Laboratuvar Hizmetleri özel bir laboratuvarda bir dış kullanıcı ekler.
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
ms.openlocfilehash: b089067a889f0ffd3b317fcc3f0784d176473b91
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="use-powershell-to-add-an-external-user-to-a-custom-lab"></a>Özel bir laboratuvar için bir dış kullanıcı eklemek için PowerShell kullanın

Bu örnek PowerShell komut dosyası Azure Laboratuvar Hizmetleri özel bir laboratuvarda bir dış kullanıcı ekler. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Özel bir laboratuvar**. Komut dosyası var olan bir özel laboratuvarı olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a custom lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzureRmADUser](/powershell/module/azurerm.resources/get-azurermaduser) | Azure active Directory'den kullanıcı nesnesi yeniden dener. |
| [New-AzureRmRoleAssignment](/module/azurerm.resources/new-azurermroleassignment) | Belirtilen kapsamda belirtilen asıl belirtilen rol atar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Laboratuvar Hizmetleri PowerShell komut dosyası örnekleri bulunabilir [Azure Laboratuvar Hizmetleri PowerShell örnekleri](../samples-powershell.md).