---
title: Azure PowerShell betik örneği - yönetilen kaynak grubu alma ve Vm'leri yeniden boyutlandırma | Microsoft Docs
description: Azure PowerShell betik örneği - yönetilen kaynak grubu alma ve Vm'leri yeniden boyutlandırma
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: baad12379c01ac955f1b0e036398e5889f3681b2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203207"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-powershell"></a>Bir yönetilen kaynak grubundaki kaynakları alma ve Vm'leri PowerShell ile yeniden boyutlandırma

Bu betik bir yönetilen kaynak grubundan kaynakları alır ve bu kaynak grubundaki VM’leri yeniden boyutlandırır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/managed-applications/get-application/get-application.ps1 "Get application")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yönetilen uygulamayı dağıtmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzureRmManagedApplication](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermmanagedapplication) | Yönetilen uygulamaları listeler. Sonuçlara odaklanmak için kaynak grubu adını belirtin. |
| [Get-AzureRmResource](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresource) | Kaynakları listeler. Sonuca odaklanmak için bir kaynak grubu ve kaynak türü sağlar. |
| [Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) | Sanal makinenin boyutunu güncelleştirir. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/get-started-azureps).
