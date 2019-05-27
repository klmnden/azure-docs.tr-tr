---
title: Azure CLI betik örneği - bir uygulamayı bir SQL veritabanı'na bağlanma | Microsoft Docs
description: Azure CLI betik örneği - bir SQL veritabanı'na bir uygulamaya Bağlan
services: appservice
documentationcenter: appservice
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 7c2efdd0-f553-4038-a77a-e953021b3f77
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: e23ef4a12182aaeb6db84615106ed4e8919938bf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66136773"
---
# <a name="connect-an-app-service-app-to-a-sql-database-using-cli"></a>Bir App Service uygulaması CLI kullanarak bir SQL veritabanına bağlanma

Bu örnek betik bir Azure SQL veritabanı ile bir App Service uygulaması oluşturur. Daha sonra uygulama ayarlarını kullanarak uygulamaya SQL veritabanına bağlar.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL Database")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, App Service uygulaması, SQL veritabanı ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | App Service planı oluşturur. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Bir App Service uygulaması oluşturur. |
| [`az sql server create`](/cli/azure/sql/server?view=azure-cli-latest#az-sql-server-create) | SQL Veritabanı Sunucusu oluşturur.  |
| [`az sql db create`](/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-create) | SQL Veritabanı Sunucusu ile yeni bir veritabanı oluşturur. |
| [`az sql db show-connection-string`](/cli/azure/sql/db?view=azure-cli-latest#az-sql-db-show-connection-string) | Veritabanına bir bağlantı dizesi oluşturur. |
| [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) | Oluşturur veya bir App Service uygulaması için uygulama ayarı güncelleştirir. Uygulama ayarları, uygulamanız için ortam değişkenleri olarak kullanıma sunulur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek App Service CLI betik örnekleri, [Azure App Service belgelerinde](../samples-cli.md) bulunabilir.
