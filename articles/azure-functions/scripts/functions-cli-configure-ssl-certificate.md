---
title: Azure CLI Betik Örneği - Özel bir SSL sertifikasını bir işlev uygulamasına bağlama | Microsoft Docs
description: Azure CLI Betik Örneği - Azure’da özel bir SSL sertifikasını bir işlev uygulamasına bağlama
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 04/10/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: fd4c69036960364e12aeea5d9e5f65e7b36eff0d
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>Özel bir SSL sertifikasını bir işlev uygulamasına bağlama

Bu örnek betik, App Service'te ilgili kaynaklarıyla birlikte bir işlev uygulaması oluşturur, ardından bu işlev uygulamasına özel bir etki alanı adının SSL sertifikasını bağlar. Bu örnekte şunlar gereklidir:

* Etki alanı kayıt şirketinizin DNS yapılandırma sayfasına erişim.
* Geçerli bir .PFX dosyası ve karşıya yükleyip bağlamak istediğiniz SSL sertifikası için parolası.

Bir SSL sertifikası bağlamak için işlev uygulamanızın bir tüketim planı değil App Service planı içinde oluşturulmuş olması gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI’yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | SSL sertifikalarını bağlamak için gereken bir App Service planı oluşturur. |
| [az functionapp create]() | Bir işlev uygulaması oluşturur. |
| [az appservice web config hostname add](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Özel bir etki alanını işlev uygulaması ile eşler. |
| [az appservice web config ssl upload](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#az_appservice_web_config_ssl_upload) | SSL sertifikasını bir işlev uygulamasına yükler. |
| [az appservice web config ssl bind](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#az_appservice_web_config_ssl_bind) | Karşıya yüklenmiş SSL sertifikasını bir işlev uygulamasına bağlar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek App Service CLI betik örnekleri, [Azure App Service belgelerinde]() bulunabilir.
