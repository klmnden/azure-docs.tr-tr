---
title: 'PowerShell Betiği: özel bir rol Azure DevTest Labs laboratuvarda oluşturun | Microsoft Docs'
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
ms.openlocfilehash: 295f742342fba7d77b556724c8005f3ac4816482
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636724"
---
# <a name="use-powershell-to-create-a-custom-role-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvarda özel bir rol oluşturmak için PowerShell kullanma

Bu örnek PowerShell komut dosyası Azure DevTest Labs laboratuvarda kullanmak için özel bir rol oluşturur. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Bir laboratuvar**. Komut dosyası var olan bir laboratuvar olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-role-in-lab/create-custom-role-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) | Azure RBAC kullanarak güvenliği sağlanabilir işlemlerini bir Azure kaynak sağlayıcısı alır. |
| [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) | Atama için uygun olan tüm Azure RBAC rolü listeler. |
| [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) | Özel bir rol oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Laboratuvar Hizmetleri PowerShell komut dosyası örnekleri bulunabilir [Azure Laboratuvar Hizmetleri PowerShell örnekleri](../samples-powershell.md).