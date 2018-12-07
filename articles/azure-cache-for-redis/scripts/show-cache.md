---
title: Azure CLI betik örneği - bir Azure önbelleği için Redis ayrıntılarını Get | Microsoft Docs
description: Azure CLI betik örneği - bir Azure önbelleği için Redis ayrıntılarını Al
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 155924e6-00d5-4a8c-ba99-5189f300464a
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 94786a17589cdae93ffa498eaaeeb1c1a81ffaf4
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53020080"
---
# <a name="get-details-of-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis ayrıntılarınıza

Bu senaryoda, bir Azure önbelleği için Redis örneği, sağlama durumu'dahil olmak üzere ayrıntılarını alma hakkında bilgi edinin.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/show-cache/show-cache.sh "Azure Azure Cache for Redis")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir Azure önbelleği için Redis örneğinin ayrıntılarını almak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Bir Azure önbelleği için Redis örneğinin ayrıntılarını alın. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure önbelleği için Redis CLI betiği örnekleri, içinde bulunabilir [Azure önbelleği için Redis belgeleri](../cli-samples.md).