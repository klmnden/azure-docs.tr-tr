---
title: Azure CLI betiği-oluşturma Azure Cosmos DB MongoDB API hesabı, veritabanı ve koleksiyonu
description: Azure CLI betik örneği - bir Azure Cosmos DB MongoDB API hesabı, veritabanı ve koleksiyonu oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: sample
ms.date: 10/26/2018
ms.openlocfilehash: ff211658b88d8df9120e881f13d83584123f1b74
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53714075"
---
# <a name="create-an-azure-cosmos-db-account-for-mongodb-api-using-azure-cli"></a>Azure CLI kullanarak MongoDB API'si için bir Azure Cosmos DB hesabı oluşturma

Bu örnek CLI betiği, bir Azure Cosmos DB MongoDB API hesabı, veritabanı ve koleksiyonu oluşturur.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-mongodb-account/create-cosmosdb-mongodb-account.sh "Create an Azure Cosmos DB for MongoDB API account, database, and collection.")]

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
