---
title: Azure PowerShell Betiği örnek - günlük analizi çalışma alanı oluşturun. | Microsoft Docs
description: Azure PowerShell Betiği örnek - için günlük analizi çalışma alanı oluşturma
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: magoedte
ms.openlocfilehash: 30d036ae56acc3a798d2776f292243f65cbea43d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23855442"
---
# <a name="create-a-log-analytics-workspace-with-powershell"></a>PowerShell ile günlük analizi çalışma alanı oluşturma

Bu komut, hazır ve çalışır hızla üzerinde veri toplama, çözümleme ve alma eylemi başlatmak istiyorsanız, gerekli olan bir Azure günlük analizi çalışma alanı ile alır.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/log-analytics/log-analytics-create-new-resource/log-analytics-create-new-resource.ps1 "Create new Log Analytics workspace")]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası, aboneliğinizde yeni bir günlük analizi çalışma alanı oluşturmak için komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [Get-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightsworkspace) | Varolan bir çalışma alanı hakkındaki bilgileri alır. |
| [AzureRmOperationalInsightsWorkspace yeni](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace) | Belirtilen kaynak grubunu ve konumu bir çalışma alanı oluşturur. |


## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

