---
title: "Azure CLI Betiği-Azure Cosmos DB MongoDB API hesabı, veritabanı ve koleksiyonu oluşturma | Microsoft Docs"
description: "Azure CLI Betiği Örneği-Azure Cosmos DB MongoDB API hesabı, veritabanı ve koleksiyonu oluşturma"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: mimig
ms.openlocfilehash: bff458531294562069f80986ee78bc6f9cfcd979
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="azure-cosmos-db-create-an-mongodb-api-account-using-the-azure-cli"></a>Azure Cosmos DB: Azure CLI’yı kullanarak MongoDB API hesabı oluşturma

Bu örnek CLI betiği, Azure Cosmos DB MongoDB API hesabı, veritabanı ve koleksiyonu oluşturur. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-mongodb-account/create-cosmosdb-mongodb-account.sh?highlight=15-35 "Create an Azure Cosmos DB MongoDB API account, database, and collection")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az cosmosdb create](/cli/azure/cosmosdb#az_cosmosdb_create) | Azure Cosmos DB hesabı oluşturur. |
| [az group delete](/cli/azure/resource#az_resource_delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure Cosmos DB CLI betiği örnekleri, [Azure Cosmos DB CLI belgelerinde](../cli-samples.md) bulunabilir.
