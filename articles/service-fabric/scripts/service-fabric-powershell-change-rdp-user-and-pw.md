---
title: Azure PowerShell Betiği Örneği - RDP kullanıcı adını ve parolasını güncelleştirme | Microsoft Docs
description: Azure PowerShell Betiği Örneği - Belirli bir düğüm türünün tüm Service Fabric küme düğümleri için RDP kullanıcı adını ve parolasını güncelleştirin.
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
ms.openlocfilehash: ff9cfabc4ac7b759a916ddaaeb3f4c95ceecd452
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>Bir kümedeki sanal makinelerin yönetici kullanıcı adını ve parolasını güncelleştirme

Service Fabric kümesindeki her [düğüm türü](../service-fabric-cluster-nodetypes.md) bir sanal makine ölçek kümesidir. Bu örnek betik, belirli bir düğüm türündeki küme sanal makineleri için yönetici kullanıcı adı ve parolasını güncelleştirir.  Yönetici parolası, değiştirilebilir bir ölçek kümesi özelliği olmadığından, ölçek kümesine VMAccessAgent uzantısını ekleyin.  Kullanıcı adı ve parola değişiklikleri, ölçek kümesindeki tüm düğümlere uygulanır. Parametreleri gereken şekilde özelleştirin.

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeyi kullanarak Azure PowerShell’i yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-user-and-pw/change-rdp-user-and-pw.ps1 "Updates a RDP username and password for cluster nodes")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Küme düğümü türünün özelliklerini alır (sanal makine ölçek kümesi).   |
| [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension)| Sanal makine ölçek kümesine bir uzantı ekler.|
| [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)|Sanal makine ölçek kümesinin durumunu, yerel bir VMSS nesnesinin durumuna güncelleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure Service Fabric için ek Azure PowerShell örneklerini [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) bölümünde bulabilirsiniz.
