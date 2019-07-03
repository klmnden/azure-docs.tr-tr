---
title: Azure CLI betiği-Cosmos hesap oluşturma Azure Cosmos DB'nin MongoDB API'si ile
description: Azure CLI betik örneği - bir Cosmos hesabı, MongoDB için Azure Cosmos DB API'si ile oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 7/2/2019
ms.reviewer: sngun
ms.openlocfilehash: a256a0ff5164ec9b25aea3849f20643ee3719fac
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541418"
---
# <a name="create-an-azure-cosmos-db-account-with-azure-cosmos-dbs-api-for-mongodb-using-azure-cli"></a>Azure Cosmos DB hesabı, Azure CLI kullanarak MongoDB için Azure Cosmos DB API'si ile oluşturma

Bu örnek CLI betiği, MongoDB için Azure Cosmos DB API'si ile Cosmos hesabı oluşturur.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

[!NOTE] Veritabanı ve kapsayıcı adlandırma kuralları hakkında daha fazla bilgi edinmek için bkz, [veritabanları, kapsayıcıları ve Azure Cosmos DB'de öğeleri ile çalışma](../databases-containers-items.md).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-mongodb-account/create-cosmosdb-mongodb-account.sh "Create a Cosmos account with Azure Cosmos DB's API for MongoDB - account, database, and collection.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Azure Cosmos DB hesabı oluşturur. |
| [az cosmosdb database create](/cli/azure/cosmosdb/database#az-cosmosdb-database-create) | Azure Cosmos DB veritabanı oluşturur. |
| [az cosmosdb collection create](/cli/azure/cosmosdb/collection#az-cosmosdb-collection-create) | MongoDB için Azure Cosmos DB koleksiyonu oluşturur. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure Cosmos DB CLI betiği örnekleri, [Azure Cosmos DB CLI belgelerinde](../cli-samples.md) bulunabilir.
