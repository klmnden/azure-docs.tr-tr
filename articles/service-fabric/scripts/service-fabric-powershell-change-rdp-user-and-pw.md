---
title: "Azure PowerShell Betiği örnek - RDP kullanıcı adı ve parola güncelleştirme | Microsoft Docs"
description: "Azure PowerShell Betiği örnek - RDP kullanıcı adı ve parola belirli bir düğüm türü tüm Service Fabric küme düğümlerinin güncelleştirin."
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
ms.date: 11/17/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: c4a02bd06b8d0b3b99055760d345b5a824bbb946
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>Yönetici kullanıcı adı ve parola VM'lerin bir kümede güncelleştirme

Her düğüm bir Service Fabric kümesindeki bir sanal makine ölçek kümesi türüdür. Bu örnek betik yönetici kullanıcı adı ve belirli bir düğüm türü'nde küme sanal makinelerin parolasını güncelleştirir.  Yönetici parolası özelliği değiştirilebilir bir ölçek kümesi olmadığından VMAccessAgent uzantısı ölçek kümesine ekleyin.  Ölçek kümesindeki tüm düğümler için kullanıcı adı ve parola değişiklikleri uygulayın. Parametreleri gereken şekilde özelleştirin.

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell'i yükleme [Azure PowerShell Kılavuzu](/powershell/azure/overview). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-user-and-pw/change-rdp-user-and-pw.ps1 "Updates a RDP username and password for cluster nodes")]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır: komutu belirli belgeleri tablo bağlanan her komutu.

| Komut | Notlar |
|---|---|
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Bir küme düğümü türü (sanal makine ölçek kümesi) özelliklerini alır.   |
| [Ekleme AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension)| Uzantı, sanal makine ölçek kümesine ekler.|
| [Güncelleştirme AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss)|Yerel bir VMSS nesne durumu için bir sanal makine ölçek durumunu güncelleştirir.|

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz: [Azure PowerShell belgelerine](/powershell/azure/overview).

Azure Service Fabric ek Azure Powershell örnekleri bulunabilir [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md).
