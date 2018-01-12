---
title: "Azure CLI: Azure veritabanı'nda yükleme sunucu, MySQL için kaydeder."
description: "Bu örnek Azure CLI betik etkinleştirmek ve MySQL sunucusu için bir Azure veritabanı'nın sunucu günlükleri indirmek nasıl gösterir."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/11/2018
ms.openlocfilehash: b0d34009d189ab136dcb6f28fdccc49b6da9e108
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Etkinleştirme ve Azure CLI kullanarak MySQL sunucusu için bir Azure veritabanı'nın sunucu yavaş sorgu günlüklerini indirin
Bu örnek CLI betik sağlar ve MySQL sunucusu için tek bir Azure veritabanının yavaş sorgu günlüklerini indirir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu örnek, Azure CLI Sürüm 2.0 veya sonraki sürümünü çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası
Bu örnek betik yönetici kullanıcı adı ve parola özelleştirmek için vurgulanan satırları değiştirin. < GÜNLÜK_DOSYASI_ADI > az İzleyici komutlar kendi server günlük dosyası adı ile değiştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Komut dosyası açıklaması
Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| **Komutu** | **Notlar** |
|---|---|
| [az grubu oluşturma](/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az mysql sunucusu oluşturun](/cli/azure/mysql/server#az_msql_server_create) | Veritabanlarını barındıran MySQL server oluşturur. |
| [az mysql server yapılandırma listesi](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) | Bir sunucu için yapılandırma değerlerini listeler. |
| [az mysql server yapılandırma kümesi](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) | Bir sunucu yapılandırmasını güncelleştirin. |
| [az mysql server günlükleri listesi](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) | Bir sunucu için günlük dosyaları listelenmektedir. |
| [az mysql server günlüklerini indirme](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) | Günlük dosyalarını indirin. |
| [az grubu Sil](/cli/azure/group#az_group_delete) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgelerine](/cli/azure/overview).
- Ek komut dosyaları deneyin: [Azure veritabanı için MySQL için Azure CLI örnekleri](../sample-scripts-azure-cli.md)
