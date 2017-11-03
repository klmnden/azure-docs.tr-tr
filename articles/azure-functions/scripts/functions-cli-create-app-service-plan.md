---
title: "Azure CLI betik örnek - bir uygulama hizmeti planında bir işlev uygulaması oluşturma | Microsoft Docs"
description: "Azure CLI betik örnek - bir uygulama hizmeti planında bir işlev uygulaması oluşturma"
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
ms.date: 04/11/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: c7868dda1e00882a944ac61d838c8b8987d5e740
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-app-in-an-app-service-plan"></a>Bir uygulama hizmeti planında işlev uygulaması oluşturma

Bu örnek betik işlevlerinizi için bir kapsayıcı olan bir Azure işlevi uygulaması oluşturur. İşlev uygulaması server kaynaklarınızı her zaman açıktır anlamına gelir adanmış bir uygulama hizmeti planı kullanılarak oluşturulur.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

Bu komut dosyası ayrılmış bir kullanarak bir Azure işlevi uygulamasını oluşturur [uygulama hizmeti planı](../functions-scale.md#app-service-plan).

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "Create an Azure Function on an App Service plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Komut belirli belgeleri tablo bağlanan her komut. Bu komut dosyasını aşağıdaki komutları kullanır:

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az depolama hesabı oluşturma](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Bir Azure Storage hesabı oluşturur. |
| [az uygulama hizmeti planı oluşturma](https://docs.microsoft.com/cli/azure/appserviceplan#az_appserviceplan_create) | App Service planı oluşturur. |
| [az functionapp oluşturma](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_delete) | Bir Azure işlev uygulaması oluşturur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek Azure işlevleri CLI kod örnekleri bulunabilir [Azure işlevleri belgelerine](../functions-cli-samples.md).
