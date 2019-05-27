---
title: Azure CLI betiği - Azure veritabanı'nda indirme sunucu, MariaDB için kaydeder.
description: Bu örnek Azure CLI betiği, MariaDB sunucusu için Azure veritabanı sunucu günlüklerini etkinleştirmeyi ve indirmeyi gösterilmektedir.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 11/28/2018
ms.openlocfilehash: da2a8ed610873d957866117a9ef7aab424edd452
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66171338"
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mariadb-server-using-azure-cli"></a>Etkinleştirme ve Azure CLI kullanarak MariaDB için Azure veritabanı sunucusunun yavaş sorgu günlüklerini indirme
Bu örnek CLI betiği, etkinleştirir ve tek bir Azure veritabanı MariaDB sunucusu için yavaş sorgu günlüklerini indirir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin. Değiştirin &lt;log_file_name&gt; içinde `az monitor` komutları ile kendi sunucu günlük dosyası adı.
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/mariadb/server-logs/delete-mariadb.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komutu** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az mariadb sunucusu oluşturma](/cli/azure/mariadb/server#az-mariadb-server-create) | Veritabanlarını barındıran bir MariaDB sunucu oluşturur. |
| [az mariadb sunucu yapılandırma listesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) | Bir sunucu için yapılandırma değerlerini listeleyin. |
| [az mariadb server configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) | Bir sunucunun yapılandırmasını güncelleştirin. |
| [az mariadb server-logs listesi](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-list) | Bir sunucunun günlük dosyalarını listeleyin. |
| [az mariadb sunucu günlüklerini indirme](/cli/azure/mariadb/server-logs#az-mariadb-server-logs-download) | Günlük dosyalarını indirin. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi edinin: [Azure CLI belgeleri](/cli/azure).
- Ek betikleri deneyin: [MariaDB için Azure veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
