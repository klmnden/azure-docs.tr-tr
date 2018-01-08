---
title: "Bir işlev uygulaması oluşturma ve Visual Studio Team Services işlevi koddan dağıtma | Microsoft Docs"
description: "Bir işlev uygulaması oluşturma ve Visual Studio Team Services işlevi koddan dağıtma"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 04/28/2017
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 15d4001e656c456c2fbe3b3d63cdd094498940c8
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="create-an-app-service"></a>Bir uygulama hizmeti oluşturma

Bu senaryoda, kullanan bir işlev uygulaması oluşturmayı öğrenmek [tüketim planı](../functions-scale.md#consumption-plan) ve Visual Studio Team Services (VSTS) depodan sürekli dağıtım ayarlayın. Bu örnek için şunlar gerekir:

* VSTS deposu, yönetici izinlerine sahip işlevleri koda sahip.
* A [kişisel erişim belirteci (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) GitHub hesabınız için.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI yerel olarak kullanmak isterseniz, Azure CLI Sürüm 2.0 veya sonraki sürümünü çalıştırması gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

Bu örnek bir Azure işlevi uygulamasını oluşturur ve Visual Studio Team Services işlevi koddan dağıtır.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, web uygulaması, documentdb ve tüm ilişkili kaynakları oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az depolama hesabı oluşturma](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | App Service planı oluşturur. |
| [az functionapp oluşturma](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [az appservice web kaynak denetimi yapılandırma](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Bir işlev uygulaması Git veya Mercurial deposu ile ilişkilendirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek Azure işlevleri CLI kod örnekleri bulunabilir [Azure işlevleri belgelerine](../functions-cli-samples.md).
