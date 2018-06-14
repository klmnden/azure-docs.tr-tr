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
ms.openlocfilehash: cbe7bf95574ca7a77d666981691da05357ce9a0d
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
ms.locfileid: "29843587"
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
| [az login](https://docs.microsoft.com/cli/azure/reference-index#az_login) | Azure'da oturum açın. |
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Konum ile bir kaynak grubu oluşturun |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account) | Depolama hesabı oluşturma |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Yeni bir işlev uygulaması oluşturun |
| [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Temizleme |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
