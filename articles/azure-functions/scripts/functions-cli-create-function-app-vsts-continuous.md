---
title: Azure’da Visual Studio Team Services’ten dağıtılmış bir işlev oluşturma | Microsoft Docs
description: Bir İşlev Uygulaması oluşturma ve işlev kodunu Visual Studio Team Services’ten dağıtma
services: functions
keywords: ''
author: syntaxc4
ms.author: glenga
ms.date: 07/03/2018
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: 1b54cfebd3ae36fc8025aeb4ea9c91d336bc5343
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988961"
---
# <a name="create-a-function-app-and-deploy-function-code-from-visual-studio-team-services"></a>Bir işlev uygulaması oluşturma ve işlev kodunu Visual Studio Team Services’ten dağıtma

Bu konu başlığında, [tüketim planını](../functions-scale.md#consumption-plan) kullanarak [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) bir işlev uygulaması oluşturmak için Azure İşlevleri kullanma gösterilir. İşlevleriniz için bir kapsayıcı olan işlev uygulaması, bir Visual Studio Team Services (VSTS) deposundan sürekli olarak dağıtılır. 

[!INCLUDE [upgrade runtime](../../../includes/functions-cli-version-note.md)]

Bu konuyu tamamlamak için şunlara sahip olmalısınız:

* İşlev uygulaması projenizi içeren ve yönetim izinlerine sahip olduğunuz bir VSTS deposu.
* VSTS deponuza erişmek için bir [kişisel erişim belirteci (PAT)](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLI’yi yerel olarak kullanırsanız, sürüm 2.0 veya sonraki bir sürümü yükleyip kullanmanız gerekir. Azure CLI sürümünü belirlemek için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

Bu betik, bir Azure İşlev uygulaması oluşturur ve işlev kodunu Visual Studio Team Services’ten dağıtır.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, depolama hesabı, işlev uygulaması ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | İşlev uygulaması için gereken depolama hesabını oluşturur. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | Sunucusuz [tüketim planında](../functions-scale.md#consumption-plan) bir işlev uygulaması oluşturur. |
| [az functionapp deployment source config](https://docs.microsoft.com/cli/azure/functionapp/deployment/source#az-functionapp-deployment-source-config) | Bir işlev uygulamasını Git veya Mercurial deposu ile ilişkilendirir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure İşlevleri CLI betiği örnekleri, [Azure İşlevleri belgelerinde](../functions-cli-samples.md) bulunabilir.
