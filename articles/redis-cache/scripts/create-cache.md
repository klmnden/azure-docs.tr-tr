---
title: "Azure CLI örnek komut dosyası - bir Azure Redis önbelleği oluşturma | Microsoft Docs"
description: "Azure CLI örnek komut dosyası - bir Azure Redis önbelleği oluşturma"
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: afd7f6e0-9297-4c98-a95e-597be939cef7
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: f65ecde40e3b5ba6dc935a6f61c321c14abe4ea9
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="create-an-azure-redis-cache"></a>Azure Redis Cache oluşturma

Bu senaryoda, bir Azure Redis önbelleği oluşturmayı öğrenin.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-cache/create-cache.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu komut, bir kaynak grubu ve redis önbelleği oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az redis oluşturma](https://docs.microsoft.com/cli/azure/redis#az_redis_create) | Redis önbelleği örneği oluşturun. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure Redis önbelleği CLI kod örnekleri bulunabilir [Azure Redis önbelleği belgelerine](../cli-samples.md).