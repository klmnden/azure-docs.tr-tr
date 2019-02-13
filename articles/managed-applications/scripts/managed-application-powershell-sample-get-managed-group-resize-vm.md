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
ms.openlocfilehash: 9e8930c95495673c0082a82757ed6d8137900b6f
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56110906"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-powershell"></a>Bir yönetilen kaynak grubundaki kaynakları alma ve Vm'leri PowerShell ile yeniden boyutlandırma

Bu betik bir yönetilen kaynak grubundan kaynakları alır ve bu kaynak grubundaki VM’leri yeniden boyutlandırır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/managed-applications/get-application/get-application.ps1 "Get application")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yönetilen uygulamayı dağıtmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Get-AzManagedApplication](https://docs.microsoft.com/powershell/module/az.resources/get-azmanagedapplication) | Yönetilen uygulamaları listeler. Sonuçlara odaklanmak için kaynak grubu adını belirtin. |
| [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource) | Kaynakları listeler. Sonuca odaklanmak için bir kaynak grubu ve kaynak türü sağlar. |
| [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) | Sanal makinenin boyutunu güncelleştirir. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/get-started-azureps).
