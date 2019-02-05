---
title: Azure CLI betiği - Sunucu yapılandırmalarını değiştirme
description: Bu örnek CLI betiği, tüm kullanılabilir sunucu yapılandırma seçeneklerini listeler ve seçeneklerden birinin değerini güncelleştirir.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: 6b5a855c8db5cb87f313e14c42396ae70b407e61
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55692214"
---
# <a name="list-and-update-configurations-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Azure CLI kullanarak bir PostgreSQL için Azure Veritabanı sunucusunun yapılandırmalarını listeleme ve güncelleştirme
Bu örnek CLI betiği, PostgreSQL için Azure Veritabanı sunucusuna ait tüm kullanılabilir yapılandırma parametrelerini izin verilen değerleriyle birlikte listeler ve *log_retention_days* öğesini varsayılandan farklı bir değere ayarlar.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI’yi Yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az postgres server create](/cli/azure/postgres/server) | Veritabanlarını barındıran bir PostgreSQL sunucusu oluşturur. |
| [az postgres server configuration list](/cli/azure/postgres/server/configuration) | Bir PostgreSQL için Azure Veritabanı sunucusunun yapılandırmalarını listeleyin. |
| [az postgres server configuration set](/cli/azure/postgres/server/configuration) | Bir PostgreSQL için Azure Veritabanı sunucusunun yapılandırmalarını güncelleştirin. |
| [az postgres server configuration show](/cli/azure/postgres/server/configuration) | Bir PostgreSQL için Azure Veritabanı sunucusunun yapılandırmasını gösterin. |
| [az group delete](/cli/azure/group) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi edinin: [Azure CLI belgeleri](/cli/azure).
- Ek betikleri deneyin: [PostgreSQL için Azure Veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- Sunucu parametreleri hakkında daha fazla bilgi için bkz. [Azure portalında sunucu parametrelerini yapılandırma](../howto-configure-server-parameters-using-portal.md).
