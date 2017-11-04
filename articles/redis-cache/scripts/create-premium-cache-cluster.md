---
title: "Azure CLI betik örnek - kümeleme Premium Azure Redis önbelleği oluşturma | Microsoft Docs"
description: "Azure CLI betik örnek - Kümeleme ile Azure Redis önbelleği Premium katmanı oluşturma"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: sdanie
ms.openlocfilehash: 45b2a500751830f6ca19fe8450a7781cb0fcb618
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-premium-azure-redis-cache-with-clustering"></a>Kümeleme Premium Azure Redis önbelleği oluşturma

Bu senaryoda, kümeleme özelliği etkinleştirilmiş bir 6 GB Premium katmanı Azure Redis önbelleği oluşturma ve iki parça öğrenin.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyası, bir kaynak grubu ve Premium katmanı redis önbelleği etkinleştir kümeleme oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az redis oluşturma](https://docs.microsoft.com/cli/azure/redis#az_redis_create) | Redis önbelleği örneği oluşturun. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek Azure Redis önbelleği CLI kod örnekleri bulunabilir [Azure Redis önbelleği belgelerine](../cli-samples.md).