---
title: "-Bir web uygulaması yedekleme Azure CLI komut dosyası örneği | Microsoft Docs"
description: "-Bir web uygulaması yedekleme Azure CLI komut dosyası örneği"
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
tags: azure-service-management
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 12/07/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: e585c9b203145dcdaf2b63a6044ba2e3bded9572
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="back-up-a-web-app"></a>Bir web uygulamasını kurup yedekleyin

Bu örnek komut dosyası ile ilgili kaynaklarını App Service'te bir web uygulaması oluşturur ve bunun için bir kerelik bir yedekleme oluşturur. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, 2.0 veya üstü Azure CLI sürüm gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-onetime/backup-onetime.sh?highlight=3-7 "Back up a web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [`az storage account create`](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create) | Bir depolama hesabı oluşturur. |
| [`az storage container create`](/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_create) | Bir Azure depolama kapsayıcısını oluşturur. |
| [`az storage container generate-sas`](/cli/azure/storage/container?view=azure-cli-latest#az_storage_container_generate_sas) | Bir Azure depolama kapsayıcısı için bir SAS belirteci oluşturur.  |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) | App Service planı oluşturur. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) | Azure web uygulaması oluşturur. |
| [`az webapp config backup create`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_create) | Bir web uygulaması için bir yedek oluşturur. |
| [`az webapp config backup list`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az_webapp_config_backup_list) | Bir web uygulaması için yedekleme listesini alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama hizmeti CLI kod örnekleri bulunabilir [Azure App Service belgeleri](../app-service-cli-samples.md).
