---
title: Azure CLI Betiği-Azure Cosmos DB hesap anahtarını yeniden oluşturma | Microsoft Docs
description: Azure CLI Betik Örneği - Azure Cosmos DB hesap anahtarını yeniden oluşturma
services: cosmos-db
documentationcenter: cosmosdb
author: SnehaGunda
manager: kfile
tags: azure-service-management
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/02/2017
ms.author: sngun
ms.openlocfilehash: fb6e821f65f07866725921c646b71af9238fb6e0
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975029"
---
# <a name="regenerate-an-azure-cosmos-db-account-key-using-the-azure-cli"></a>Azure CLI’yı kullanarak Azure Cosmos DB hesap anahtarını yeniden oluşturma

Bu örnek, Azure CLI’yı kullanarak herhangi bir türden Azure Cosmos DB hesap anahtarını yeniden oluşturur. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-regenerate-keys/secure-cosmosdb-regenerate-keys.sh?highlight=27-31 "Regenerate Azure Cosmos DB account keys")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az cosmosdb create](https://docs.microsoft.com/cli/azure/cosmosdb#az-cosmosdb-create) | Azure Cosmos DB hesabını güncelleştirir. |
| [az cosmosdb regenerate-key](/cli/azure/cosmosdb#az-cosmosdb-regenerate-key) | Azure Cosmos DB hesap anahtarlarını yeniden oluşturur. |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure Cosmos DB CLI betiği örnekleri, [Azure Cosmos DB CLI belgelerinde](../cli-samples.md) bulunabilir.
