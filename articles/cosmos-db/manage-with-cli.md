---
title: Azure CLI kullanarak Azure Cosmos DB kaynaklarını yönetme
description: Azure CLI, Azure Cosmos DB hesabı, veritabanı ve kapsayıcıları yönetmek için kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 4/8/2019
ms.author: mjbrown
ms.openlocfilehash: 1d19e58b2d1381725de490b68d9e4d00a2ca4cb6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66151205"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>Azure CLI kullanarak Azure Cosmos kaynaklarını yönetme

Aşağıdaki kılavuzda, Azure Cosmos DB hesapları, veritabanları ve Azure CLI kullanarak kapsayıcı yönetimini otomatikleştirmek için sık kullanılan komutlar açıklanır. Tüm Azure Cosmos DB CLI komutlarına ait başvuru sayfalarına [Azure CLI Başvurusu](https://docs.microsoft.com/cli/azure/cosmosdb) sayfasından erişilebilir. Daha fazla örnekleri de bulabilirsiniz [Azure Cosmos DB için Azure CLI örnekleri](cli-samples.md), oluşturma ve Cosmos DB hesapları, veritabanları ve kapsayıcılar, MongoDB, Gremlin, Cassandra ve tablo API'si için yönetme de dahil olmak üzere.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturun

Oturum tutarlılığı bölgelerde, Doğu ABD ve Batı ABD SQL API'si ile bir Azure Cosmos DB hesabı oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az cosmosdb create \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup \
   --kind GlobalDocumentDB \
   --default-consistency-level Session \
   --locations EastUS=0 WestUS=1 \
   --enable-multiple-write-locations false
```

> [!IMPORTANT]
> Azure Cosmos hesap adı küçük harfle yazılmalıdır.

## <a name="create-a-database"></a>Veritabanı oluşturma

Bir Cosmos DB veritabanı oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az cosmosdb database create \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir bölüm anahtarı ve 400 RU/sn ile Cosmos DB kapsayıcısı oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
# Create a container
az cosmosdb collection create \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --partition-key-path /myPartitionKey \
   --throughput 400
```

## <a name="change-the-throughput-of-a-container"></a>Bir kapsayıcının aktarım hızını değiştirme

1000 RU/s olarak bir Cosmos DB kapsayıcısı aktarım hızını değiştirmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
# Update container throughput
az cosmosdb collection update \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --throughput 1000
```

## <a name="list-account-keys"></a>Hesap anahtarlarını Listele

Cosmos hesabınız için anahtarları almak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
# List account keys
az cosmosdb list-keys \
   --name  mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="list-connection-strings"></a>Liste bağlantı dizeleri

Cosmos hesabınız için bağlantı dizelerini almak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="regenerate-account-key"></a>Hesap anahtarını yeniden oluştur

Cosmos hesabınız için yeni bir birincil anahtarı yeniden oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
# Regenerate account key
az cosmosdb regenerate-key \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup \
   --key-kind primary
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz:

- [Azure CLI'yı yükleme](/cli/azure/install-azure-cli)
- [Azure CLI Başvurusu](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Azure Cosmos DB için ek Azure CLI örnekleri](cli-samples.md)
