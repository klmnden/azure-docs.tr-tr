---
title: Bir Azure Depolama’ya bağlanan bir Azure İşlevi oluşturma | Microsoft Docs
description: Azure CLI Betiği Örneği - Azure Depolama’ya bağlanan bir Azure İşlevi oluşturma
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
ms.date: 04/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 9bc4d25b587b7167601765758a0529868d1c6f15
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988740"
---
# <a name="create-a-function-app-that-connects-to-an-azure-storage-account"></a>Bir Azure Depolama hesabına bağlanan bir işlev uygulaması oluşturma

Bu Azure İşlevleri örnek betiği, bir işlev uygulaması oluşturur ve işlevi bir Azure Depolama hesabına bağlar. Bağlantıyı içeren oluşturulan uygulama ayarı, bir [depolama tetikleyicisi veya bağlaması](..\functions-bindings-storage-blob.md) ile birlikte kullanılabilir. 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI’yi yerel olarak kullanırsanız, Azure CLI sürüm 2.0 veya sonraki bir sürümü çalıştırdığınızdan emin olun. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

Bu örnek bir Azure İşlev uygulaması oluşturur ve depolama bağlantı dizesini bir uygulama ayarına ekler.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-storage/create-function-app-connect-to-storage-account.sh "Integrate Function App into Azure Storage Account")]


## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın:

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Konum ile bir kaynak grubu oluşturun. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Depolama hesabı oluşturma. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Sunucusuz [tüketim planında](../functions-scale.md#consumption-plan) bir işlev uygulaması oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
