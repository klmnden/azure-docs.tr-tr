---
title: "Azure CLI komut dosyası yeniden Azure Cosmos DB hesap anahtarı | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - yeniden Azure Cosmos DB hesap anahtarı"
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
ms.openlocfilehash: b667aed1ecdf403654ab86b2b7601d5cbddaa725
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="regenerate-an-azure-cosmos-db-account-key-using-the-azure-cli"></a>Azure CLI kullanarak Azure Cosmos DB hesap anahtarı yeniden oluştur

Bu örnek, her türlü Azure CLI kullanarak Azure Cosmos DB hesap anahtarı oluşturur. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-regenerate-keys/secure-cosmosdb-regenerate-keys.sh?highlight=27-31 "Regenerate Azure Cosmos DB account keys")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](/cli/azure/group#create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az cosmosdb oluşturma](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_create) | Bir Azure Cosmos DB hesabını güncelleştirir. |
| [az cosmosdb yeniden anahtar](/cli/azure/cosmosdb/regenerate-key) | Regeneratates Azure Cosmos DB hesabı anahtarları. |
| [az grubu Sil](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek Azure Cosmos DB CLI kod örnekleri bulunabilir [Azure Cosmos DB CLI belgelerine](../cli-samples.md).
