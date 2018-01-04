---
title: "Azure CLI betik örnek - github'dan sürekli dağıtımı ile bir web uygulaması oluşturma | Microsoft Docs"
description: "Azure CLI betik örnek - github'dan sürekli dağıtımı ile bir web uygulaması oluşturma"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0205c991-0989-4ca3-bb41-237dcc964460
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 12/11/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: bbfc66a2913c4459bfe8c80770b170ae1b2bc356
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>Sürekli dağıtım github'dan bir web uygulaması oluşturma

Bu örnek komut dosyası ile ilgili kaynaklarını App Service'te bir web uygulaması oluşturur ve GitHub depodan sürekli dağıtım ayarlar. Sürekli dağıtım olmadan GitHub dağıtımı için bkz: [bir web uygulaması oluşturma ve dağıtma github'dan kod](app-service-cli-deploy-github.md). Bu örnek için şunlar gerekir:

* Yönetici izinlerine sahip uygulama kodu, GitHub deposuyla.
* A [kişisel erişim belirteci (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) GitHub hesabınız için.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, 2.0 veya üstü Azure CLI sürüm gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "Create a web app with continuous deployment from GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | App Service planı oluşturur. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) | Azure web uygulaması oluşturur. |
| [`az webapp deployment source config`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config) | Azure web uygulaması Git veya Mercurial deposu ile ilişkilendirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama hizmeti CLI kod örnekleri bulunabilir [Azure App Service belgeleri](../app-service-cli-samples.md).
