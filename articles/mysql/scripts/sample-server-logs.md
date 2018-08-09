---
title: Azure CLI betiği - MySQL için Azure Veritabanı’nda sunucu günlüklerini indirme
description: Bu örnek Azure CLI betiği, bir MySQL için Azure Veritabanı sunucusunun sunucu günlüklerini etkinleştirmeyi ve indirmeyi gösterir.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 02/28/2018
ms.openlocfilehash: 98f1179dd6dff4cd8b0125266899374d80893d26
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420429"
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Azure CLI kullanarak bir MySQL için Azure Veritabanı sunucusunun yavaş sorgu günlüklerini etkinleştirme ve indirme
Bu örnek CLI betiği, tek bir MySQL için Azure Veritabanı sunucusunun yavaş sorgu günlüklerini etkinleştirir ve indirir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI aracını yerel olarak çalıştırmayı tercih ederseniz bu makale için Azure CLI aracının 2.0 veya sonraki bir sürümü gerekir. `az --version` komutunu çalıştırarak sürümü denetleyin. Azure CLI aracını yüklemek veya sürümünüzü yükseltmek için bkz. [Azure CLI 2.0’ı yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek betik
Bu örnek betikte vurgulanan satırları düzenleyerek yönetici kullanıcı adını ve parolasını kendi değerlerinizle güncelleştirin. `az monitor` komutlarındaki <log_file_name> öğesini, kendi sunucu günlük dosyası adınızla değiştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/server-logs.sh?highlight=18-19 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Betik çalıştırıldıktan sonra aşağıdaki komutu kullanarak kaynak grubunu ve bununla ilişkili tüm kaynakları kaldırın. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Betik açıklaması
Bu betik, aşağıdaki tabloda ana hatları verilen komutları kullanır:

| **Komut** | **Notlar** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az mysql server create](/cli/azure/mysql/server#az-msql-server-create) | Veritabanlarını barındıran bir MySQL sunucusu oluşturur. |
| [az mysql server configuration list](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-list) | Bir sunucu için yapılandırma değerlerini listeleyin. |
| [az mysql server configuration set](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) | Bir sunucunun yapılandırmasını güncelleştirin. |
| [az mysql server-logs list](/cli/azure/mysql/server-logs#az-mysql-server-logs-list) | Bir sunucunun günlük dosyalarını listeleyin. |
| [az mysql server-logs download](/cli/azure/mysql/server-logs#az-mysql-server-logs-download) | Günlük dosyalarını indirin. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgeleri](/cli/azure).
- Ek betikleri deneyin: [MySQL için Azure Veritabanı Azure CLI örnekleri](../sample-scripts-azure-cli.md)
