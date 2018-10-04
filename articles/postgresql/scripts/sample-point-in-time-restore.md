---
title: Azure CLI betiği - Bir PostgreSQL için Azure Veritabanı sunucusunu geri yükleme
description: Bu örnek Azure CLI betiğinde bir PostgreSQL için Azure Veritabanı sunucunu ve veritabanlarını zamanda önceki bir noktaya geri yükleme gösterilmektedir.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: b696eefd36eb83154f5bdf6629a68d5f26b7aa51
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994205"
---
# <a name="restore-an-azure-database-for-postgresql-server-using-azure-cli"></a>Azure CLI kullanarak bir PostgreSQL için Azure Veritabanı sunucusunu geri yükleme
Bu örnek CLI betiği tek bir PostgreSQL için Azure Veritabanı sunucusunu zamanda önceki bir noktaya geri yükler.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin. `az monitor` komutlarında kullanılan abonelik kimliğini kendi abonelik kimliğinizle değiştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/backup-restore/backup-restore.sh?highlight=18-19 "Restore Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/backup-restore/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az postgresql server create](/cli/azure/postgres/server#az-postgres-server-create) | Veritabanlarını barındıran bir PostgreSQL sunucusu oluşturur. |
| [az postgresql server restore](/cli/azure/postgres/server#az-postgres-server-restore) | Bir sunucuyu yedekten geri yükler. |
| [az group delete](/cli/azure/group#az_group_delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgeleri](/cli/azure).
- Ek betikleri deneyin: [PostgreSQL için Azure Veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- [Azure portalını kullanarak PostgreSQL için Azure Veritabanı’nda sunucu yedekleme ve geri yükleme](../howto-restore-server-portal.md)
