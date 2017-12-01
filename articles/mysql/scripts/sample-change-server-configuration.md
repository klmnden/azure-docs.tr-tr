---
title: "Azure CLI betik - değişiklik sunucu yapılandırmaları | Microsoft Docs"
description: "Bu örnek CLI betik tüm kullanılabilir sunucu yapılandırmaları listeler ve innodb_lock_wait_timeout değerini güncelleştirir."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/03/2017
ms.openlocfilehash: 9a94f257e5cd3534127e8594ddee3c5f837876df
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Azure CLI kullanarak MySQL sunucusu için bir Azure veritabanı'nın listesi ve güncelleştirme yapılandırmaları
Bu örnek CLI komut dosyası izin verilen değerleri yanı sıra tüm kullanılabilir yapılandırma parametreleri Azure veritabanı için MySQL sunucusu için listelenir ve ayarlar *innodb_lock_wait_timeout* varsayılan dışında olan bir değer.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası
Bu örnek betik yönetici kullanıcı adı ve parola özelleştirmek için vurgulanan satırları değiştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Komut dosyası açıklaması
Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| **Komutu** | **Notlar** |
|---|---|
| [az grubu oluşturma](/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az mysql sunucusu oluşturun](/cli/azure/mysql/server#az_msql_server_create) | Veritabanlarını barındıran MySQL server oluşturur. |
| [az mysql server yapılandırma listesi](/cli/azure/mysql/server/configuration#az_msql_server_configuration_list) | MySQL sunucusu için bir Azure veritabanı yapılandırmaları listeler. |
| [az mysql server yapılandırma kümesi](/cli/azure/mysql/server/configuration#az_msql_server_configuration_set) | MySQL sunucusu için bir Azure veritabanı'nın yapılandırmasını güncelleştirin. |
| [az mysql server yapılandırması Göster](/cli/azure/mysql/server/configuration#az_msql_server_configuration_show) | MySQL sunucusu için bir Azure veritabanının yapılandırmasını gösterir. |
| [az grubu Sil](/cli/azure/group#az_group_delete) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgelerine](/cli/azure/overview).
- Ek komut dosyaları deneyin: [Azure veritabanı için MySQL için Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- Sunucu parametreleri hakkında daha fazla bilgi için bkz: [nasıl için yapılandırma sunucusu parametrelerinde Azure veritabanı için MySQL](../howto-server-parameters.md).
