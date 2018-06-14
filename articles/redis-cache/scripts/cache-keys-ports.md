---
title: Azure CLI komut dosyası örneği - Get ana bilgisayar adı, bağlantı noktaları ve Azure Redis önbelleği için anahtarları | Microsoft Docs
description: Azure CLI komut dosyası örneği - Get ana bilgisayar adı, bağlantı noktaları ve Azure Redis önbelleği örneği için anahtarları
services: redis-cache
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
ms.openlocfilehash: 9a2a6a7ed4bb56f93c965a5bfd24b21368117c7b
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
ms.locfileid: "29846460"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-redis-cache"></a>Azure Redis önbelleği için ana bilgisayar adı, bağlantı noktalarını ve anahtarları alma

Bu senaryoda, ana bilgisayar adı, bağlantı noktaları ve Azure Redis önbelleği örneğine bağlanmak için kullanılan anahtarlarını alma hakkında bilgi edinin.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a>Betik açıklaması

Bu komut, ana bilgisayar adı, anahtarlar ve bağlantı noktaları bir Azure Redis önbelleği örneğinin almak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az redis Göster](https://docs.microsoft.com/cli/azure/redis#az_redis_show) | Bir Azure Redis önbelleği örneği ayrıntılarını alma. |
| [az redis listesi anahtarlar](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) | Bir Azure Redis önbelleği örneği için erişim anahtarları alır. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure Redis önbelleği CLI kod örnekleri bulunabilir [Azure Redis önbelleği belgelerine](../cli-samples.md).