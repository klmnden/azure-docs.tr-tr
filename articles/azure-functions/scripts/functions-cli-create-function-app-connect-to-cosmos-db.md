---
title: Azure Cosmos DB’ye bağlanan bir Azure İşlevi oluşturma | Microsoft Docs
description: Azure CLI Betiği Örneği - Azure Cosmos DB'ye bağlanan bir Azure İşlevi oluşturma
services: functions
documentationcenter: functions
author: ggailey777
manager: cfowler
editor: ''
tags: functions
ms.assetid: ''
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 07/03/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 9154cef897d38e617c2f9dccdc8a47fe1af72104
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989470"
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Azure Cosmos DB’ye bağlanan bir Azure İşlevi oluşturma

Bu Azure İşlevleri örnek betiği, bir işlev uygulaması oluşturur ve işlevi bir Azure Cosmos DB veritabanına bağlar. Bağlantıyı içeren oluşturulan uygulama ayarı, [Azure Cosmos DB tetikleyicisi veya bağlaması](..\functions-bindings-cosmosdb.md) ile birlikte kullanılabilir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI’yi yerel olarak kullanırsanız, Azure CLI sürüm 2.0 veya sonraki bir sürümü çalıştırdığınızdan emin olun. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

Bu örnek, bir Azure İşlevi uygulaması oluşturur ve uygulama ayarlarına Cosmos DB uç noktası ve erişim anahtarı ekler.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Konum ile bir kaynak grubu oluşturma |
| [az storage accounts create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Depolama hesabı oluşturma |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Sunucusuz [tüketim planında](../functions-scale.md#consumption-plan) bir işlev uygulaması oluşturur. |
| [az cosmosdb create](https://docs.microsoft.com/cli/azure/cosmosdb#az-cosmosdb-create) | Azure Cosmos DB veritabanı oluşturun. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.




