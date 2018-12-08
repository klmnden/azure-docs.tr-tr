---
title: Azure PowerShell betik örneği - bir Log Analytics çalışma alanı oluşturma | Microsoft Docs
description: Azure PowerShell betik örneği - bir Log Analytics çalışma alanına oluşturma
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: log-analytics
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: magoedte
ms.openlocfilehash: 5ad04c52da4709a7097ff7915d7af7404d6725eb
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109510"
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>PowerShell ile bir Log Analytics çalışma alanı oluşturma

Bu betik, çalışmaya hızlıca üzerinde veri toplama, çözümleme ve alma eylemi başlatmak istiyorsanız, gerekli olan Azure Log Analytics çalışma alanıyla alır.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/log-analytics/log-analytics-create-new-resource/log-analytics-create-new-resource.ps1 "Create new Log Analytics workspace")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, aboneliğinizde yeni bir Log Analytics çalışma alanı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-Azurermoperationalınsightsworkspace](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightsworkspace) | Mevcut bir çalışma alanı hakkında bilgi alır. |
| [Yeni-Azurermoperationalınsightsworkspace](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace) | Belirtilen kaynak grubu ve konumda bir çalışma alanı oluşturur. |


## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

