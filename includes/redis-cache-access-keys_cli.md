---
title: include dosyası
description: include dosyası
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 03/28/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: d32ea8af6ad8282872572076db4b9eef34e05166
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53011867"
---
### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Azure CLI kullanarak ana bilgisayar adı, bağlantı noktaları ve erişim anahtarlarını alma

Ana bilgisayar adı ve bağlantı noktaları çağırabilirsiniz Azure CLI kullanarak alınacak [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show)ve çağırabilirsiniz anahtarlarını almak için [az redis anahtarları-Listele](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys). Aşağıdaki betik bu iki komutu çağırır ve ana bilgisayar, bağlantı noktaları ve anahtarları konsola yansıtır.

```azurecli
#/bin/bash

# Retrieve the hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve the hostname and ports for an Azure Azure Cache for Redis instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve the keys for an Azure Azure Cache for Redis instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display the retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

Bu betik hakkında daha fazla bilgi için bkz. [ana bilgisayar adı, bağlantı noktalarını ve anahtarları Azure Azure önbelleği için Redis için alma](../articles/azure-cache-for-redis/scripts/cache-keys-ports.md). Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) ve [Azure CLI ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).
