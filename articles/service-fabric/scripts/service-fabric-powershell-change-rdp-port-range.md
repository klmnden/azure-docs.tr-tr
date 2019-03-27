---
title: Azure PowerShell Betiği Örneği - RDP bağlantı noktası aralığını değiştirme | Microsoft Docs
description: Azure PowerShell Betiği Örneği - Dağıtılan bir kümenin RDP bağlantı noktası aralığını değiştirir.
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 03/19/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: ee2ac3a2051ba7dd63aac5928e1713541f23b81f
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58500109"
---
# <a name="update-the-rdp-port-range-values"></a>RDP bağlantı noktası aralığı değerlerini güncelleştirme

Bu örnek betik, küme dağıtıldıktan sonra küme düğümü sanal makinelerinde RDP bağlantı noktası aralığı değerlerini değiştirir.  Temel sanal makinelerin döngüye girmemesi için Azure PowerShell kullanılır.  Betik, kümenin kaynak grubundaki `Microsoft.Network/loadBalancers` kaynağını alıp `inboundNatPools.frontendPortRangeStart` ve `inboundNatPools.frontendPortRangeEnd` değerlerini güncelleştirir. Parametreleri gereken şekilde özelleştirin.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeyi kullanarak Azure PowerShell’i yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | `Microsoft.Network/loadBalancers` kaynağını alır. |
|[Set-AzResource](/powershell/module/az.resources/set-azresource)|`Microsoft.Network/loadBalancers` kaynağını güncelleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure Service Fabric için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) bölümünde bulabilirsiniz.
