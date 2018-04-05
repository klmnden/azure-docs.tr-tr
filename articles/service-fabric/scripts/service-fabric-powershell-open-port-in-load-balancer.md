---
title: Azure PowerShell Betiği Örneği - Yük dengeleyicide uygulama bağlantı noktasını açma | Microsoft Docs
description: Azure PowerShell Betiği Örneği - Service Fabric uygulaması için Azure Load Balancer’da bir bağlantı noktasını açın.
services: service-fabric
documentationcenter: ''
author: rwike77
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 03/19/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 1edaca9c354a7b65b47213c7970e823aee3cbe87
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>Azure Load Balancer’da bir uygulama bağlantı noktasını açma

Azure’da çalıştırılan bir Service Fabric uygulaması, Azure Load Balancer’ın ardında yer alır. Bu örnek betik, Service Fabric uygulamasının dış istemcilerle iletişim kurabilmesi için Azure Load Balancer’da bir bağlantı noktasını açar. Parametreleri gereken şekilde özelleştirin. Kümeniz bir ağ güvenliği grubundaysa, gelen trafiğe izin vermek için [bir gelen ağ güvenliği grubu kuralı ekleyin](service-fabric-powershell-add-nsg-rule.md).

Gerekirse, [Service Fabric SDK’sı](../service-fabric-get-started.md) ile Service Fabric PowerShell modülünü yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Bir Azure kaynağını alır.  |
| [Get-AzureRmLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) | Azure Load Balancer’ı alır. |
| [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | Yük dengeleyiciye bir araştırma yapılandırması ekler.|
| [Get-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | Yük dengeleyici için bir araştırma yapılandırması alır. |
| [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | Yük dengeleyiciye bir kural yapılandırması ekler. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Yük dengeleyici için hedef durumunu ayarlar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure Service Fabric için ek Powershell örneklerine [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) içinden erişilebilir.
