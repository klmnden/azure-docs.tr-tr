---
title: "Github'dan dağıtılan Azure işlevi oluşturma | Microsoft Docs"
description: "Bir işlev uygulaması oluşturma ve Azure işlevleri kullanarak bir GitHub deposuna işlevi koddan dağıtın."
services: functions
ms.service: functions
keywords: 
ms.devlang: azurecli
author: syntaxc4
ms.author: cfowler
ms.date: 01/09/2018
ms.topic: sample
ms.custom: mvc
ms.openlocfilehash: c4224bc7973cd1e3ca36799db9f23a124fcba807
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="create-a-function-in-azure-that-is-deployed-from-github"></a>Github'dan dağıtılan Azure işlevi oluşturma

Bu örnek komut dosyası kullanarak bir işlev uygulaması oluşturur [tüketim planı](../functions-scale.md#consumption-plan) ile ilgili kaynaklarını ve sürekli olarak GitHub deposunu işlevi kodunuzdan dağıtır. Bu örnekte, aşağıdakiler gerekir:

* Yönetici izinlerine sahip işlevleri kodu, GitHub deposuyla.
* A [kişisel erişim belirteci (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) GitHub hesabınız için.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLI yerel olarak kullanırsanız, yüklemeniz ve sürüm 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Azure CLI Sürüm belirlemek için çalıştırın `az --version`. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

Bu örnek bir Azure işlevi uygulamasını oluşturur ve işlev kodu github'dan dağıtır.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Komut belirli belgeleri tablo bağlanan her komut. Bu komut dosyasını aşağıdaki komutları kullanır:

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az depolama hesabı oluşturma](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | App Service planı oluşturur. |
| [az functionapp oluşturma](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [az appservice web kaynak denetimi yapılandırma](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Bir işlev uygulaması Git veya Mercurial deposu ile ilişkilendirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek Azure işlevleri CLI kod örnekleri bulunabilir [Azure işlevleri belgelerine](../functions-cli-samples.md).
