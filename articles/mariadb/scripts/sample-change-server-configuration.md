---
title: Azure CLI betiği - Sunucu yapılandırmalarını değiştirme
description: Bu örnek CLI betiği, kullanılabilir tüm sunucu yapılandırmalarını listeler ve innodb_lock_wait_timeout değerini güncelleştirir.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 11/28/2018
ms.openlocfilehash: 514f2fe7e8c2b94e175bc6f6fb0cec8ea3d0c9c6
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125891"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>Azure CLI kullanarak MariaDB için Azure veritabanı yapılandırmalarını listeleme ve güncelleştirme
Bu örnek CLI betiği, tüm kullanılabilir yapılandırma parametrelerini ve bunun yanı sıra izin verilen değerleriyle MariaDB sunucusu için Azure veritabanı listelenir ve ayarlar *innodb_lock_wait_timeout* varsayılandan olan bir değer.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for MariaDB.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/change-server-configurations/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az mariadb sunucusu oluşturma](/cli/azure/mariadb/server#az-mariadb-server-create) | Veritabanlarını barındıran bir MariaDB sunucu oluşturur. |
| [az mariadb sunucu yapılandırma listesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) | MariaDB server için Azure veritabanı yapılandırmalarını listeleyin. |
| [az mariadb server configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) | MariaDB server için Azure veritabanı yapılandırmasını güncelleştirin. |
| [az mariadb server configuration show](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-show) | MariaDB server için Azure veritabanı yapılandırmasını gösterir. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi edinin: [Azure CLI belgeleri](/cli/azure).
- Ek betikleri deneyin: [MariaDB için Azure veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- Sunucu parametreleri hakkında daha fazla bilgi için bkz. [nasıl sunucu parametrelerini yapılandırma MariaDB için Azure veritabanı'nda](../howto-server-parameters.md).
