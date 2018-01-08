---
title: "Azure CLI komut dosyası örneği - özel bir SSL sertifikası bir işlev uygulaması bağlama | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - bağ azure'da bir işlev uygulaması için özel bir SSL sertifikası"
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
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
ms.openlocfilehash: f8e8570d9c3093b5f49b000916644888304eed4e
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="bind-a-custom-ssl-certificate-to-a-function-app"></a>Bir işlev uygulaması için özel bir SSL sertifikası bağlama

Bu örnek betik bir işlev uygulaması App Service'te ile ilgili kaynaklarını oluşturur, ardından özel etki alanı adı SSL sertifikası kendisine bağlar. Bu örnek için şunlar gerekir:

* Etki alanı kayıt şirketinizin DNS yapılandırma sayfasına erişim.
* Geçerli. PFX dosyası ve SSL sertifikasının parolası karşıya yüklemek ve bağlamak istediğiniz.

Bir SSL sertifikası bağlamak için işlevi uygulamanıza bir uygulama hizmeti planı yer alan ve tüketim planı oluşturulması gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI yerel olarak kullanmak isterseniz, Azure CLI Sürüm 2.0 veya sonraki sürümünü çalıştırması gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az uygulama hizmeti planı oluşturma](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | SSL sertifikaları bağlamak için gerekli bir uygulama hizmeti planı oluşturur. |
| [az functionapp oluşturma]() | Bir işlev uygulaması oluşturur. |
| [az appservice web yapılandırma ana bilgisayar adı ekleyin](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Özel bir etki alanı işlev uygulaması eşler. |
| [az appservice web yapılandırma ssl karşıya yükleme](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#az_appservice_web_config_ssl_upload) | Bir SSL sertifikası bir işlev uygulaması yükler. |
| [az appservice web yapılandırma ssl bağlama](https://docs.microsoft.com/cli/azure/appservice/web/config/ssl#az_appservice_web_config_ssl_bind) | Karşıya yüklenen bir SSL sertifikası bir işlev uygulaması bağlar. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama hizmeti CLI kod örnekleri bulunabilir [Azure App Service belgeleri]().
