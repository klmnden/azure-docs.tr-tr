---
title: Azure CLI Betiği Örneği - App Service planında bir İşlev Uygulaması oluşturma | Microsoft Docs
description: Azure CLI Betiği Örneği - App Service planında bir İşlev Uygulaması oluşturma
services: functions
documentationcenter: functions
author: syntaxc4
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 07/03/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: c12297075e967446572898b94d3abbcaf79f9e84
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989069"
---
# <a name="create-a-function-app-in-an-app-service-plan"></a>App Service planında bir İşlev Uygulaması oluşturma

Bu Azure İşlevleri örnek betiği, işlevleriniz için kapsayıcı olan bir işlev uygulaması oluşturur. Oluşturulan işlev uygulaması, ayrılmış bir App Service planı kullandığından sunucu kaynaklarınız her zaman açıktır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI sürüm 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

Bu betik, ayrılmış bir [App Service planını](../functions-scale.md#app-service-plan) kullanarak bir Azure İşlev uygulaması oluşturur.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "Create an Azure Function on an App Service plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Tablodaki her komut, komuta özgü belgelere yönlendirir. Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | Azure Depolama hesabı oluşturur. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az-appservice-plan-create) | App Service planı oluşturur. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | App Service planında bir işlev uygulaması oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
