---
title: Azure CLI betik örneği - bir ana bilgisayar adı, bağlantı noktalarını ve anahtarları Azure önbelleği için Redis için alın | Microsoft Docs
description: Azure CLI betik örneği - bir Azure önbelleği için Redis örneği için ana bilgisayar adı, bağlantı noktalarını ve anahtarları alma
services: azure-cache-for-redis
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: wesmc
ms.openlocfilehash: 0c6f7b637e56d2bf39d8f03122ccb28bd7b1c773
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53096754"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-cache-for-redis"></a>Ana bilgisayar adı, bağlantı noktalarını ve anahtarları, Azure önbelleği için Redis için Al

Bu senaryoda, ana bilgisayar adı, bağlantı noktaları ve bir Azure önbelleği için Redis örneği bağlanmak için kullanılan anahtarlarını alma hakkında bilgi edinin.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Cache for Redis")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, ana bilgisayar adı, anahtarlar ve bağlantı noktaları, bir Azure önbelleği için Redis örneği almak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Bir Azure önbelleği için Redis örneğinin ayrıntılarını alın. |
| [az redis anahtarları-Listele](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Bir Azure önbelleği için Redis örneği için erişim anahtarlarını alır. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure önbelleği için Redis CLI betiği örnekleri, içinde bulunabilir [Azure önbelleği için Redis belgeleri](../cli-samples.md).