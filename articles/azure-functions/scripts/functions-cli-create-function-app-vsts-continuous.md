---
title: "Visual Studio Team Services dağıtılan Azure işlevi oluşturma | Microsoft Docs"
description: "Bir işlev uygulaması oluşturma ve Visual Studio Team Services işlevi koddan dağıtma"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 01/09/2018
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: bf9428f23e851bae3485ec3d724dfb9ccd2af4c1
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="create-a-function-in-azure-that-is-deployed-from-visual-studio-team-services"></a>Visual Studio Team Services dağıtılan Azure işlevi oluşturma

Bu konuda Azure işlevlerinin oluşturmak için nasıl kullanılacağı gösterilmektedir bir [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) uygulama işlevini kullanarak [tüketim planı](../functions-scale.md#consumption-plan). İşlevlerinizi için bir kapsayıcıdır, işlev uygulaması, bir Visual Studio Team Services (VSTS) depodan sürekli olarak dağıtılır. Bu konuda tamamlamak için şunlara sahip olmalısınız:

* İşlev uygulaması projenize içeren ve yönetim izinlerine sahip bir VSTS deposu.
* A [kişisel erişim belirteci (PAT)](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) VSTS deponuz erişmek için.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLI yerel olarak kullanırsanız, yüklemeniz ve sürüm 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Azure CLI Sürüm belirlemek için çalıştırın `az --version`. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

Bu örnek bir Azure işlevi uygulamasını oluşturur ve Visual Studio Team Services işlevi koddan dağıtır.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, depolama hesabı, işlev uygulaması ve tüm ilgili kaynaklar oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az depolama hesabı oluşturma](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | App Service planı oluşturur. |
| [az functionapp oluşturma](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [az appservice web kaynak denetimi yapılandırma](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Bir işlev uygulaması Git veya Mercurial deposu ile ilişkilendirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek Azure işlevleri CLI kod örnekleri bulunabilir [Azure işlevleri belgelerine](../functions-cli-samples.md).
