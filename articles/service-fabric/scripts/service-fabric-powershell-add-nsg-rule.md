---
title: "Azure PowerShell Betiği örnek - bir ağ güvenlik grubu kural ekleme | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - belirli bir bağlantı noktasındaki gelen trafiğe izin vermek için ağ güvenlik grubu ekler."
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
ms.openlocfilehash: fd3c648ee63c45bef305658832a4d31dfdb213be
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="add-an-inbound-network-security-group-rule"></a>Bir gelen ağ güvenlik grubu kuralı ekleyin

Bu örnek betik, bağlantı noktası 8081 gelen trafiğe izin verecek şekilde bir ağ güvenlik grubu kural oluşturur.  Komut dosyasını alır `Microsoft.Network/networkSecurityGroups` küme içinde bulunduğu kaynak yeni bir ağ güvenliği yapılandırması kuralı oluşturur ve ağ güvenlik grubunu güncelleştirir. Parametreleri gereken şekilde özelleştirin.

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](/powershell/azure/overview). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-inbound-nsg-rule/add-inbound-nsg-rule.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Alır `Microsoft.Network/networkSecurityGroups` kaynak. |
|[Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)| Ağ güvenlik grubu adına göre alır.|
|[Ekleme AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig)| Bir ağ güvenlik kuralı yapılandırması için ağ güvenlik grubu ekler. |
|[Set-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/set-azurermnetworksecuritygroup)| Hedef durumu için bir ağ güvenlik grubu ayarlar.|

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).
