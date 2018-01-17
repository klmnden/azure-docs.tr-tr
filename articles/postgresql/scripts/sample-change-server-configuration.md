---
title: "Azure CLI betik: sunucu yapılandırmasını değiştirir"
description: "Bu örnek CLI betik tüm kullanılabilir sunucu yapılandırma seçeneği listelenmiştir ve seçeneklerden birini değerini güncelleştirir."
services: postgres
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 334322557c83d5576c9a11def6bb0dc0e3ad1fec
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="list-and-update-configurations-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Listesinde ve Azure CLI kullanarak PostgreSQL sunucu için bir Azure veritabanı yapılandırmalarını güncelleştirme
Bu örnek CLI betik Azure veritabanı için izin verilen değerleri yanı sıra tüm kullanılabilir yapılandırma parametreleri PostgreSQL sunucusu için listeler ve ayarlar *log_retention_days* varsayılan dışında olan bir değer.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası
Bu örnek betik yönetici kullanıcı adı ve parola özelleştirmek için vurgulanan satırları değiştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Komut dosyası açıklaması
Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| **Komutu** | **Notlar** |
|---|---|
| [az grubu oluşturma](/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az postgres sunucu oluşturma](/cli/azure/postgres/server#az_postgres_server_create) | Veritabanlarını barındıran bir PostgreSQL sunucusu oluşturur. |
| [az postgres sunucu yapılandırması listesi](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Azure veritabanı PostgreSQL sunucu için yapılandırmaları listeler. |
| [az postgres sunucu yapılandırma kümesi](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Bir PostgreSQL sunucusuna Azure veritabanının yapılandırmasını güncelleştirin. |
| [az postgres sunucu yapılandırması Göster](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_show) | Bir PostgreSQL sunucusuna Azure veritabanının yapılandırmasını gösterir. |
| [az grubu Sil](/cli/azure/group#az_group_delete) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgelerine](/cli/azure/overview).
- Ek komut dosyaları deneyin: [PostgreSQL Azure veritabanı için Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- Sunucu parametreleri hakkında daha fazla bilgi için bkz: [nasıl yapılandırılır sunucu parametreleri Azure portalında](../howto-configure-server-parameters-using-portal.md).
