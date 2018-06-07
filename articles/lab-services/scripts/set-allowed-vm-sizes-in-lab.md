---
title: "PowerShell Betiği: Azure Laboratuvar Hizmetleri'nde VM boyutları izin | Microsoft Docs"
description: Bu PowerShell komut dosyası izin verilen VM boyutları Azure Laboratuvar Hizmetleri'nde ayarlar.
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
ms.openlocfilehash: 159f175e7bb27b2d89001e1eba737c67adb89e50
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34638152"
---
# <a name="use-powershell-to-set-allowed-vm-sizes-in-azure-lab-services"></a>Ayarlamak için PowerShell kullanın VM boyutları Azure Laboratuvar Hizmetleri'nde izin verilir.

Bu örnek PowerShell komut dosyası izin verilen sanal makine (VM) boyutları Azure Laboratuvar Hizmetleri'nde ayarlar.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Bir laboratuvar**. Komut dosyası var olan bir laboratuvar olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/set-allowed-vm-sizes-in-lab/set-allowed-vm-sizes-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Find-AzureRmResource](/module/azurerm.resources/find-azurermresource) | Belirtilen parametrelere göre kaynakları arar. |
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Kaynakları alır. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Bir kaynak değiştirir. |
| [New-AzureRmResource](/powershell/module/azurerm.resources/new-azurermresource) | Bir kaynağı oluşturun. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Laboratuvar Hizmetleri PowerShell komut dosyası örnekleri bulunabilir [Azure Laboratuvar Hizmetleri PowerShell örnekleri](../samples-powershell.md).