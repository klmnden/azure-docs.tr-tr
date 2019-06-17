---
title: Azure CLI betik örneği - Premium Azure önbelleği için Redis Kümeleme ile oluşturma | Microsoft Docs
description: Azure CLI betik örneği - Azure Cache Premium katmanı için Redis Kümeleme ile oluşturma
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: yegu
ms.openlocfilehash: 18538cf713d2938b0b595c9d4664c39c6ada5231
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62130458"
---
# <a name="create-a-premium-azure-cache-for-redis-with-clustering"></a>Premium Azure önbelleği için Redis Kümeleme ile oluşturma

Bu senaryoda, Redis kümeleme özelliği etkinleştirilmiş olan ve iki parça için 6 GB Premium katmanı Azure önbelleği oluşturma hakkında bilgi edinin.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu oluşturmak için aşağıdaki komutları kullanır ve bir Premium katmanı Azure önbelleği için Redis Kümeleme ile etkinleştirebilirsiniz. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az redis oluşturma](https://docs.microsoft.com/cli/azure/redis) | Azure önbelleği için Redis örneği oluşturun. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure önbelleği için Redis CLI betiği örnekleri, içinde bulunabilir [Azure önbelleği için Redis belgeleri](../cli-samples.md).
