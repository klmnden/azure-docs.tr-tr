---
title: 'PowerShell Betiği: Azure Lab Services sanal makine boyutlarını izin | Microsoft Docs'
description: Bu PowerShell Betiği, Azure Lab Services içinde izin verilen VM boyutları ayarlar.
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
ms.openlocfilehash: 0c82e304d3e3d8df1206c7c05883399b74229af7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66160541"
---
# <a name="use-powershell-to-set-allowed-vm-sizes-in-azure-lab-services"></a>Ayarlamak için PowerShell kullanarak VM boyutları Azure Lab Services içinde izin verilir.

Bu örnek PowerShell Betiği, Azure Lab Services içinde izin verilen sanal makine (VM) boyutları ayarlar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Önkoşullar
* **Bir laboratuvar**. Komut dosyası var olan bir laboratuvar olmasını gerektirir. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/devtest-lab/set-allowed-vm-sizes-in-lab/set-allowed-vm-sizes-in-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| AzResource Bul | Belirtilen parametrelere bağlı kaynakları arar. |
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Kaynakları alır. |
| [Set-AzResource](/powershell/module/az.resources/set-azresource) | Bir kaynağı değiştirir. |
| [Yeni AzResource](/powershell/module/az.resources/new-azresource) | Bir kaynak oluşturun. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Lab Services PowerShell Betiği örnekleri, içinde bulunabilir [Azure Lab Services PowerShell örnekleri](../samples-powershell.md).
