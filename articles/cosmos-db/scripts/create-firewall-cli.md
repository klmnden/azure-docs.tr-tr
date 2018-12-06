---
title: Azure CLI betiği-oluşturma Azure Cosmos DB için güvenlik duvarı
description: Azure CLI Betik Örneği - Azure Cosmos DB için güvenlik duvarı oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/26/2018
ms.openlocfilehash: 793fc68bfcd2d7afdc7a5a4b1916737889d27ba5
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52955375"
---
# <a name="azure-cosmos-db-create-a-firewall-using-azure-cli"></a>Azure Cosmos DB: Azure CLI’yı kullanarak güvenlik duvarı oluşturma

Bu örnek CLI betiği her türlü Azure Cosmos DB hesabı için bir güvenlik duvarı ilkesi oluşturur.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-create-firewall/secure-cosmosdb-create-firewall.sh "Create an Azure Cosmos DB firewall")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli-$resourceGroupName
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Bir Azure CosmosDB hesabı oluşturur. |
| [az cosmosdb update](/cli/azure/cosmosdb#az-cosmosdb-update) | Azure CosmosDB hesabını güncelleştirir. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure Cosmos DB CLI betiği örnekleri, [Azure Cosmos DB CLI belgelerinde](../cli-samples.md) bulunabilir.
