---
title: "Azure CLI komut dosyası örneği - özel bir SSL sertifikasını bir web uygulamasına bağlama | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - bağlaması bir web uygulaması için özel bir SSL sertifikası"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 12/11/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: a0f6f70749c5a0f22ee015d199be490863e38738
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a>Bir web uygulaması için özel bir SSL sertifikası bağlama

Bu örnek komut dosyası ile ilgili kaynaklarını App Service'te bir web uygulaması oluşturur, ardından özel etki alanı adı SSL sertifikası kendisine bağlar. Bu örnek için şunlar gerekir:

* Etki alanı kayıt şirketinizin DNS yapılandırma sayfasına erişim.
* Geçerli. PFX dosyası ve SSL sertifikasının parolası karşıya yüklemek ve bağlamak istediğiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, 2.0 veya üstü Azure CLI sürüm gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | App Service planı oluşturur. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) | Azure web uygulaması oluşturur. |
| [`az webapp config hostname add`](/cli/azure/webapp/config/hostname?view=azure-cli-latest#az_webapp_config_hostname_add) | Özel bir etki alanı için bir web uygulaması eşler. |
| [`az webapp config ssl upload`](/cli/azure/webapp/config/ssl?view=azure-cli-latest#az_webapp_config_ssl_upload) | Bir web uygulaması için bir SSL sertifikası yükler. |
| [`az webapp config ssl bind`](/cli/azure/webapp/config/ssl?view=azure-cli-latest#az_webapp_config_ssl_bind) | Karşıya yüklenen bir SSL sertifikası bir web uygulamasına bağlar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama hizmeti CLI kod örnekleri bulunabilir [Azure App Service belgeleri](../app-service-cli-samples.md).
