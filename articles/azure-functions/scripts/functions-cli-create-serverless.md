---
title: "Azure CLI betik örnek - sunucusuz yürütme için bir işlev uygulaması oluşturma | Microsoft Docs"
description: "Azure CLI betik örnek - sunucusuz yürütme için bir işlev uygulaması oluşturma"
services: functions
documentationcenter: functions
author: syntaxc4
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 10/04/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 9436cafa775f2ad658c0c9901f7f8eae769c7052
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-app-for-serverless-execution"></a>Sunucusuz yürütme için bir işlev uygulaması oluşturma

Bu örnek betik işlevlerinizi için bir kapsayıcı olan bir Azure işlevi uygulaması oluşturur. İşlev uygulaması kullanılarak oluşturulan [tüketim planı](../functions-scale.md#consumption-plan), olay denetimli sunucusuz iş yükleri için ideal olduğu.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu makalede, Azure CLI Sürüm 2.0 veya sonraki sürümünü çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

Bir Azure işlevi uygulamasını kullanarak bu betiği oluşturur [tüketim planı](../functions-scale.md#consumption-plan).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-consumption/create-function-app-consumption.sh "Create an Azure Function on a consumption plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Komut belirli belgeleri tablo bağlanan her komut. Bu komut dosyasını aşağıdaki komutları kullanır:

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az depolama hesabı oluşturma](/cli/azure/storage/account#az_storage_account_create) | Bir Azure Storage hesabı oluşturur. |
| [az functionapp oluşturma](/cli/azure/functionapp#az_functionapp_create) | Bir işlev uygulaması oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](/cli/azure/overview).

Ek Azure işlevleri CLI kod örnekleri bulunabilir [Azure işlevleri belgelerine](../functions-cli-samples.md).
