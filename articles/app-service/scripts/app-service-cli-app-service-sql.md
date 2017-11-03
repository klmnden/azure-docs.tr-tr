---
title: "Azure CLI betik örnek - bir web uygulamasını bir SQL veritabanına bağlama | Microsoft Docs"
description: "Azure CLI betik örnek - bir web uygulaması bir SQL veritabanına bağlan"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 7c2efdd0-f553-4038-a77a-e953021b3f77
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 12e373a6503127d57f5fc1ed719c82bf56aed60c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-a-web-app-to-a-sql-database"></a>Bir web uygulaması bir SQL veritabanına bağlanma

Bu senaryoda, bir Azure SQL database ve Azure web uygulaması nasıl oluşturulacağını öğreneceksiniz. Daha sonra uygulama ayarları kullanarak web uygulaması SQL veritabanına bağlar.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL Database")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Komut dosyası açıklaması

Bu komut, bir kaynak grubu, web uygulaması, SQL veritabanı ve tüm ilgili kaynaklar oluşturmak için aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| Komut | Notlar |
|---|---|
| [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az uygulama hizmeti planı oluşturma](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | App Service planı oluşturur. Bu, Azure web uygulamanız için bir sunucu grubu gibidir. |
| [az webapp oluşturma](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Azure web uygulaması oluşturur. |
| [az sql server oluşturun](https://docs.microsoft.com/cli/azure/sql/server#az_sql_server_create) | Bir SQL veritabanı sunucusuna oluşturur.  |
| [az sql db oluştur](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_create) | SQL veritabanı sunucusu ile yeni bir veritabanı oluşturur. |
| [az webapp config appsettings ayarlama](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) | Oluşturur veya bir Azure web uygulaması için bir uygulama ayarı güncelleştirir. Uygulama ayarları uygulamanız için ortam değişkenleri olarak sunulur. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz: [Azure CLI belgelerine](https://docs.microsoft.com/cli/azure/overview).

Ek uygulama hizmeti CLI kod örnekleri bulunabilir [Azure App Service belgeleri](../app-service-cli-samples.md).
