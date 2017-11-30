---
title: "Azure PowerShell Betiği örnek - değişiklik RDP bağlantı noktası aralığı | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - dağıtılan küme RDP bağlantı noktası aralığını değiştirir."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 9089d071c69830323a94140a5ebb4e9e865499c6
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="update-the-rdp-port-range-values"></a>RDP bağlantı noktası aralığı değerleri güncelleştirin

Küme dağıtıldıktan sonra bu örnek betik VM'ler küme düğümünde RDP bağlantı noktası aralığı değerlerini değiştirir.  Temel alınan VM'ler değil döngüsü böylece azure PowerShell kullanılır.  Komut dosyasını alır `Microsoft.Network/loadBalancers` küme kaynak grubu ve güncelleştirmeleri kaynak `inboundNatPools.frontendPortRangeStart` ve `inboundNatPools.frontendPortRangeEnd` değerleri. Parametreleri gereken şekilde özelleştirin.

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](/powershell/azure/overview). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Alır `Microsoft.Network/loadBalancers` kaynak. |
|[Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource)|Güncelleştirmeleri `Microsoft.Network/loadBalancers` kaynak.|

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Azure Service Fabric ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md).
