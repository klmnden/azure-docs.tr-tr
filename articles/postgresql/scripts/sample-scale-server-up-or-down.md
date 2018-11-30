---
title: Azure CLI betiği - PostgreSQL için Azure Veritabanı’nı ölçeklendirme
description: Örnek Azure CLI Betiği - Ölçümleri sorguladıktan sonra PostgreSQL için Azure Veritabanı sunucusunu farklı bir performans düzeyinde olacak şekilde ölçeklendirin.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 04/05/2018
ms.openlocfilehash: 2602165a6fb5aea85383d4e435479f0bf0f7e48c
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582122"
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Azure CLI kullanarak tek bir PostgreSQL sunucusunu izleme ve ölçeklendirme
Bu örnek CLI betiği, ölçümleri sorguladıktan sonra tek bir PostgreSQL için Azure Veritabanı sunucusunu farklı bir performans düzeyinde olacak şekilde ölçeklendirir. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin. `az monitor` komutlarında kullanılan SubscriptionID değerini kendi abonelik kimliğinizle değiştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az postgres server create](/cli/azure/postgres/server#az_postgres_server_create) | Veritabanlarını barındıran bir PostgreSQL sunucusu oluşturur. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az_monitor_metrics_list) | Kaynaklar için ölçüm değerini listeleyin. |
| [az group delete](/cli/azure/group#az_group_delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkındaki diğer bilgileri okuyun: [Azure CLI belgeleri](/cli/azure)
- Ek betikleri deneyin: [PostgreSQL için Azure Veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- Ölçeklendirme hakkında daha fazla bilgi edinin: [Hizmet Katmanları](../concepts-service-tiers.md) ve [İşlem Birimleri ve Depolama Birimleri](../concepts-compute-unit-and-storage.md)
