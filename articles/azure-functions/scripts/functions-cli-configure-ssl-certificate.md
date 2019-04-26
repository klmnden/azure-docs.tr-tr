---
title: Azure CLI Betik Örneği - Özel bir SSL sertifikasını bir işlev uygulamasına bağlama | Microsoft Docs
description: Azure CLI Betik Örneği - Azure’da özel bir SSL sertifikasını bir işlev uygulamasına bağlama
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: azure-functions
ms.devlang: azurecli
ms.topic: sample
ms.date: 07/03/2013
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 84cc4e92419e683f8d2c29883b26148fdef23356
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60325740"
---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>Özel bir SSL sertifikasını bir işlev uygulamasına bağlama

Bu örnek betik, App Service'te ilgili kaynaklarıyla birlikte bir işlev uygulaması oluşturur, ardından bu işlev uygulamasına özel bir etki alanı adının SSL sertifikasını bağlar. Bu örnekte şunlar gereklidir:

* Etki alanı kayıt şirketinizin DNS yapılandırma sayfasına erişim.
* Geçerli bir .PFX dosyası ve karşıya yükleyip bağlamak istediğiniz SSL sertifikası için parolası.
* Özel etki alanınızda web uygulamanızın varsayılan etki alanı adına işaret eden bir A kaydını yapılandırma. Daha fazla bilgi için bkz. [Azure App Service için özel etki alanını eşleme yönergeleri](https://aka.ms/appservicecustomdns).

Bir SSL sertifikası bağlamak için işlev uygulamanızın bir tüketim planı değil App Service planı içinde oluşturulmuş olması gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI’yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az storage account create](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-create) | İşlev uygulaması için gereken bir depolama hesabı oluşturur. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az-appservice-plan-create) | SSL sertifikalarını bağlamak için gereken bir App Service planı oluşturur. |
| [az functionapp create](https://docs.microsoft.com/cli/azure/functionapp#az-functionapp-create) | App Service planında bir işlev uygulaması oluşturur. |
| [az functionapp config hostname add](https://docs.microsoft.com/cli/azure/functionapp/config/hostname#az-functionapp-config-hostname-add) | Özel bir etki alanını işlev uygulaması ile eşler. |
| [az functionapp config ssl upload](https://docs.microsoft.com/cli/azure/functionapp/config/ssl#az-functionapp-config-ssl-upload) | SSL sertifikasını bir işlev uygulamasına yükler. |
| [az functionapp config ssl bind](https://docs.microsoft.com/cli/azure/functionapp/config/ssl#az-functionapp-config-ssl-bind) | Karşıya yüklenmiş SSL sertifikasını bir işlev uygulamasına bağlar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek App Service CLI betik örnekleri, [Azure App Service belgelerinde](../functions-cli-samples.md) bulunabilir.
