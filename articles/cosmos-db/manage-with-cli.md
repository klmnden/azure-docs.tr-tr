---
title: Azure CLI kullanarak Azure Cosmos DB kaynaklarını yönetme | Microsoft Docs
description: Azure CLI, Azure Cosmos DB hesabı, veritabanı ve kapsayıcıları yönetmek için kullanın.
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: 3446f4f71349d0b7290a2514edf46efb37203324
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019341"
---
# <a name="manage-azure-cosmos-db-resources-using-azure-cli"></a>Azure CLI kullanarak Azure Cosmos DB kaynaklarını yönetme

Aşağıdaki kılavuzda, Azure Cosmos DB hesapları, veritabanları ve Azure CLI kullanarak kapsayıcı yönetimini otomatikleştirmek için komutları açıklanır. Ayrıca, kapsayıcının aktarım hızını ölçeklendirme komutları içerir. Tüm Azure Cosmos DB CLI komutlarına ait başvuru sayfalarına [Azure CLI Başvurusu](https://docs.microsoft.com/cli/azure/cosmosdb) sayfasından erişilebilir. Daha fazla örnekleri de bulabilirsiniz [Azure Cosmos DB için Azure CLI örnekleri](cli-samples.md), oluşturma ve Cosmos DB hesapları, veritabanları ve kapsayıcılar, MongoDB, Gremlin, Cassandra ve tablo API'si için yönetme de dahil olmak üzere.

Bu örnek CLI betiği, bir Azure Cosmos DB SQL API hesabı, veritabanı ve kapsayıcı oluşturur.  

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Oturum tutarlılığı, Doğu ABD ve Batı ABD, etkin çok yöneticili SQL API'si ile bir Azure Cosmos DB hesabı oluşturmak için Azure CLI'yi açın veya cloud shell ve aşağıdaki komutu çalıştırın:

```azurecli-interactive
az cosmosdb create \
   –-name "myCosmosDbAccount" \
   --resource-group "myResourceGroup" \
   --kind GlobalDocumentDB \
   --default-consistency-level "Session" \
   --locations "EastUS=0" "WestUS=1" \
   --enable-multiple-write-locations true \
```

## <a name="create-a-database"></a>Veritabanı oluşturma

Bir Cosmos DB veritabanı oluşturmak için Azure CLI'yi açın veya cloud shell ve aşağıdaki komutu çalıştırın:

```azurecli-interactive
az cosmosdb database create \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup"
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir Cosmos DB kapsayıcısı ile 1000 RU/sn ve bölüm anahtarı oluşturmak için Azure CLI'yi açın veya cloud shell ve aşağıdaki komutu çalıştırın:

```azurecli-interactive
# Create a container
az cosmosdb collection create \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --partition-key-path = "/myPartitionKey" \
   --throughput 1000
```

## <a name="change-the-throughput-of-a-container"></a>Bir kapsayıcının aktarım hızını değiştirme

Bir Cosmos DB kapsayıcısı aktarım hızını 400 RU/s olarak değiştirmek için Azure CLI'yi açın veya cloud shell ve aşağıdaki komutu çalıştırın:

```azurecli-interactive
# Update container throughput
az cosmosdb collection update \
   --collection-name "myContainer" \
   --name "myCosmosDbAccount" \
   --db-name "myDatabase" \
   --resource-group "myResourceGroup" \
   --throughput 400
```

## <a name="list-account-keys"></a>Hesap anahtarlarını Listele

Azure Cosmos DB hesabı oluşturduğunuzda, hizmet, Azure Cosmos DB hesabına erişim sağlandığında kimlik doğrulaması için kullanılan iki ana erişim anahtarları oluşturur. İki erişim tuşu sağlayarak, Azure Cosmos DB, kesinti olmadan Azure Cosmos DB hesabınız için anahtarları yeniden sağlar. Salt okunur anahtarlar, salt okunur işlemler kimlik doğrulaması için de kullanılabilir. Var. iki okuma-yazma anahtarları (birincil ve ikincil) ve iki salt okunur anahtarlar (birincil ve ikincil) Aşağıdaki komutu çalıştırarak, hesabınız için anahtarları alabilirsiniz:

```azurecli-interactive
# List account keys
az cosmosdb list-keys \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="list-connection-strings"></a>Liste bağlantı dizeleri

Uygulamanızın Cosmos DB hesabına bağlanmak için bağlantı dizesi, aşağıdaki komutu kullanarak alınabilir.

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup"
```

## <a name="regenerate-account-key"></a>Hesap anahtarını yeniden oluştur

Düzenli aralıklarla bağlantılarınızı daha güvenli olmasına yardımcı olmak için Azure Cosmos DB hesabınızın erişim anahtarlarını değiştirmeniz gerekir. Bir erişim anahtarı yeniden oluşturmak, bir erişim anahtarı kullanarak Azure Cosmos DB hesabına bağlantıları sağlamak için iki erişim tuşu atanır.

```azurecli-interactive
# Regenerate account key
az cosmosdb regenerate-key \
   --name "myCosmosDbAccount"\
   --resource-group "myResourceGroup" \
   --key-kind primary
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz:

- [Azure CLI'yı yükleme](/cli/azure/install-azure-cli)
- [Azure CLI Başvurusu](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Azure Cosmos DB için ek Azure CLI örnekleri](cli-samples.md)