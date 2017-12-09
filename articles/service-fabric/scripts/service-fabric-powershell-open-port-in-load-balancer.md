---
title: "Azure PowerShell Betiği örnek - yük dengeleyici açık uygulama bağlantı noktası | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - açık Azure yük dengeleyici Service Fabric uygulaması için bir bağlantı noktası."
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
ms.date: 08/15/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 9dbb0bedd02752c4735ae097a7bd64b7b5383d6e
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>Azure yük dengeleyici bir uygulama bağlantı noktasını açın

Azure'da çalışan Service Fabric uygulaması Azure yük dengeleyici bulunur. Bu örnek betik, bir bağlantı noktası bir Azure yük dengeleyici açar, böylece Service Fabric uygulaması dış istemcilerle iletişim kurabilir. Parametreleri gereken şekilde özelleştirin. 

Gerekirse, ile Service Fabric PowerShell modülünü yüklemek [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut özgü belgelere Tablo bağlantıları her komut.

| Komut | Notlar |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Bir Azure kaynağı alır.  |
| [Get-AzureRmLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) | Azure yük dengeleyici alır. |
| [Ekleme AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | Bir araştırma yapılandırması bir yük dengeleyiciye ekler.|
| [Get-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | Bir yük dengeleyici için bir araştırma yapılandırmasını alır. |
| [Ekleme AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | Kural yapılandırması bir yük dengeleyiciye ekler. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Hedef durumu bir yük dengeleyici için ayarlar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Azure Service Fabric ek Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md).
