---
title: Azure PowerShell betik örneği - bir ağ güvenlik grubu kuralı ekleme | Microsoft Docs
description: Azure PowerShell betik örneği - belirli bir bağlantı noktasında gelen trafiğe izin veren bir ağ güvenlik grubu ekler.
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 11/28/2017
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 92efd6c4e63959b6098829892c5cbbaeeabe9f83
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66159992"
---
# <a name="add-an-inbound-network-security-group-rule"></a>Bir gelen ağ güvenlik grubu Kuralı Ekle

Bu örnek betik, 8081 numaralı bağlantı noktasında gelen trafiğe izin veren bir ağ güvenlik grubu kuralı oluşturur.  Komut dosyasını alır `Microsoft.Network/networkSecurityGroups` küme içinde bulunduğu kaynak yeni bir ağ güvenlik yapılandırma kuralı oluşturur ve ağ güvenlik grubu güncelleştirir. Parametreleri gereken şekilde özelleştirin.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Gerekli olursa, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell kılavuzunda](/powershell/azure/overview). 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-inbound-nsg-rule/add-inbound-nsg-rule.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | `Microsoft.Network/networkSecurityGroups` kaynağını alır. |
|[Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup)| Ağ güvenlik grubu adına göre alır.|
|[Add-AzNetworkSecurityRuleConfig](/powershell/module/az.network/add-aznetworksecurityruleconfig)| Bir ağ güvenlik kuralı yapılandırması için ağ güvenlik grubu ekler. |
|[Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup)| Bir ağ güvenlik grubu için hedef durumunu ayarlar.|

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).
