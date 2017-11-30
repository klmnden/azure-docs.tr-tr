---
title: "Bir Azure Cosmos DB'ye bağlanacak bir Azure işlevi oluşturma | Microsoft Docs"
description: "Azure CLI betik örnek - bir Azure Cosmos DB'ye bağlanacak bir Azure işlevi oluşturma"
services: functions
documentationcenter: functions
author: ggailey777
manager: cfowler
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 7e9ccd26e7c066189576903c39be63a421d1da23
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Bir Azure Cosmos DB'ye bağlanacak bir Azure işlevi oluşturma

Bu örnek betik Azure işlev uygulaması oluşturur ve bir Azure Cosmos DB veritabanına bağlar.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI yerel olarak kullanırsanız, Azure CLI Sürüm 2.0 veya sonraki sürümünü çalıştırdığından emin olun. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

Bu örnek bir Azure işlev uygulaması oluşturur ve uygulama ayarlarını Cosmos DB uç noktası ve erişim anahtarı ekler.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Komut dosyası örneği çalıştırdıktan sonra kaynak grubunu kaldırmak için izleme komut kullanılabilir ve tüm kaynakları ilgili.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır: komutu belirli belgeleri tablo bağlanan her komutu.

| Komut | Notlar |
|---|---|
| [az oturum açma](https://docs.microsoft.com/cli/azure/#login) | Azure'da oturum açın. |
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Konumu ile kaynak grubu oluştur |
| [az depolama hesabı oluşturma](https://docs.microsoft.com/cli/azure/storage/account) | Depolama hesabı oluşturma |
| [az functionapp oluşturma](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Yeni bir işlev uygulaması oluşturma |
| [az cosmosdb oluşturma](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_create) | Cosmosdb veritabanı oluşturma |
| [az grubu Sil](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Temizleme |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek Azure işlevleri CLI kod örnekleri bulunabilir [Azure işlevleri belgelerine](../functions-cli-samples.md).




