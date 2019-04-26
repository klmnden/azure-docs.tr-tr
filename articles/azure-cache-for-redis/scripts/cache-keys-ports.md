---
title: Azure CLI betik örneği - bir ana bilgisayar adı, bağlantı noktalarını ve anahtarları Azure önbelleği için Redis için alın | Microsoft Docs
description: Azure CLI betik örneği - bir Azure önbelleği için Redis örneği için ana bilgisayar adı, bağlantı noktalarını ve anahtarları alma
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/30/2017
ms.author: yegu
ms.openlocfilehash: ff410db561879089c4c1f20acb7cb349f0484fda
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60234298"
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
| [az redis show](https://docs.microsoft.com/cli/azure/redis) | Bir Azure önbelleği için Redis örneğinin ayrıntılarını alın. |
| [az redis anahtarları-Listele](https://docs.microsoft.com/cli/azure/redis) | Bir Azure önbelleği için Redis örneği için erişim anahtarlarını alır. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure önbelleği için Redis CLI betiği örnekleri, içinde bulunabilir [Azure önbelleği için Redis belgeleri](../cli-samples.md).
