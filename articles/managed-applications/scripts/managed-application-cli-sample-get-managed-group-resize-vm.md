---
title: Azure CLI betik örneği - Yönetilen kaynak grubu alma ve VM’leri yeniden boyutlandırma | Microsoft Docs
description: Azure CLI betik örneği - Yönetilen kaynak grubu alma ve VM’leri yeniden boyutlandırma
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/25/2017
ms.author: tomfitz
ms.openlocfilehash: bbf03a0d53769c93a8aab304d3128ae0cc875a8f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61365903"
---
# <a name="get-resources-in-a-managed-resource-group-and-resize-vms-with-azure-cli"></a>Azure CLI ile bir yönetilen kaynak grubundaki kaynakları alma ve VM’leri yeniden boyutlandırma

Bu betik bir yönetilen kaynak grubundan kaynakları alır ve bu kaynak grubundaki VM’leri yeniden boyutlandırır.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/managed-applications/get-application/get-application.sh "Get application")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, yönetilen uygulamayı dağıtmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az managedapp list](https://docs.microsoft.com/cli/azure/managedapp#az-managedapp-list) | Yönetilen uygulamaları listeler. Sonuçlara odaklanmak için sorgu değerleri sağlar. |
| [az resource list](https://docs.microsoft.com/cli/azure/resource#az-resource-list) | Kaynakları listeler. Sonuca odaklanmak için bir kaynak grubu ve sorgu değerleri sağlar. |
| [az vm resize](https://docs.microsoft.com/cli/azure/vm#az-vm-resize) | Sanal makinenin boyutunu güncelleştirir. |


## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamalara giriş için [Azure Yönetilen Uygulamalara genel bakış](../overview.md) konusunu inceleyin.
* Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).
