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
ms.openlocfilehash: 0289604cce7f956406d65743d5b058ec92111724
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32182664"
---
### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Azure CLI kullanarak ana bilgisayar adı, bağlantı noktaları ve erişim anahtarlarını alma
Azure CLI 2.0’ı kullanarak ana bilgisayar adı ve bağlantı noktalarını almak için [az redis show](https://docs.microsoft.com/cli/azure/redis#az_redis_show) yöntemini, anahtarları almak için [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#az_redis_list_keys) yöntemini çağırabilirsiniz. Aşağıdaki betik bu iki komutu çağırır ve ana bilgisayar, bağlantı noktaları ve anahtarları konsola yansıtır.

```azurecli
#/bin/bash

# Retrieve the hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve the hostname and ports for an Azure Redis Cache instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve the keys for an Azure Redis Cache instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display the retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

Bu betik hakkında daha fazla bilgi için bkz. [Azure Redis Cache için ana bilgisayar adı, bağlantı noktası ve anahtar alma](../articles/redis-cache/scripts/cache-keys-ports.md). Azure CLI 2.0 hakkında daha fazla bilgi için bkz. [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) ve [Azure CLI 2.0 ile çalışmaya başlama](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).
