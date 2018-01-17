---
title: "Azure CLI betik: PostgreSQL sunucu için bir Azure veritabanı geri yükleme"
description: "Bu örnek Azure CLI betik MySQL sunucusu için bir Azure veritabanı ve veritabanlarını zaman içinde daha önceki bir noktaya geri gösterilmektedir."
services: postgresql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: dfc53ae10055b0e8583fb0c705e605bbbb999760
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="restore-an-azure-database-for-postgresql-server-using-azure-cli"></a>Azure CLI kullanarak PostgreSQL sunucu için bir Azure veritabanı geri yükleme
Bu örnek CLI betik PostgreSQL sunucusu için tek bir Azure veritabanına zamandaki önceki bir noktaya geri yükler.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu örnek, Azure CLI Sürüm 2.0 veya sonraki sürümünü çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası
Bu örnek betik yönetici kullanıcı adı ve parola özelleştirmek için vurgulanan satırları değiştirin. Kendi abonelik kimliğiyle az İzleyici komutlarda kullanılan abonelik kimliği Değiştir
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/backup-restore/backup-restore.sh?highlight=15-16 "Restore Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/backup-restore/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Komut dosyası açıklaması
Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| **Komutu** | **Notlar** |
|---|---|
| [az grubu oluşturma](/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az postgresql sunucu oluşturma](/cli/azure/postgresql/server#az_msql_server_create) | Veritabanlarını barındıran bir PostgreSQL sunucusu oluşturur. |
| [az postgresql sunucusu geri yüklemesi](/cli/azure/postgresql/server#az_msql_server_restore) | Bir sunucu yedekten geri yükleyin. |
| [az grubu Sil](/cli/azure/group#az_group_delete) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgelerine](/cli/azure/overview).
- Ek komut dosyaları deneyin: [PostgreSQL Azure veritabanı için Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- [Yedekleme ve Azure veritabanındaki bir sunucu için Azure portalını kullanarak PostgreSQL geri yükleme](../howto-restore-server-portal.md)
