---
title: "PowerShell Betiği: Azure DevTest labs'deki bir laboratuvara özel bir rol oluşturun | Microsoft Docs"
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
ms.openlocfilehash: 4d0c795dffb40ab7efec9005660439f9baef9f3f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66160585"
---
# <a name="use-powershell-to-create-a-custom-role-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara özel bir rolü oluşturmak için PowerShell kullanma

Bu örnek PowerShell Betiği, Azure DevTest labs'deki bir laboratuvara kullanmak için özel bir rol oluşturur. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Bir laboratuvar**. Komut dosyası var olan bir laboratuvar olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/create-custom-role-in-lab/create-custom-role-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) | Azure RBAC kullanarak güvenli hale getirilebilen işlemleri için bir Azure kaynak sağlayıcısı alır. |
| [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) | Atama için kullanılabilir olan tüm Azure RBAC rollerini listeler. |
| [New-AzRoleDefinition](/powershell/module/az.resources/new-azroledefinition) | Özel bir rol oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Lab Services PowerShell Betiği örnekleri, içinde bulunabilir [Azure Lab Services PowerShell örnekleri](../samples-powershell.md).
