---
title: "Azure CLI betik: indirme sunucu, PostgreSQL için Azure veritabanında kaydeder"
description: "Bu örnek Azure CLI betik etkinleştirmek ve bir Azure veritabanının PostgreSQL sunucusu için sunucu günlükleri indirmek nasıl gösterir."
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
ms.openlocfilehash: 8bd6e193ca52401b16e141162a76d39bdef840f1
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Etkinleştirme ve Azure CLI kullanarak PostgreSQL sunucu için bir Azure veritabanı sunucusu yavaş sorgu günlüklerini indirin
Bu örnek CLI betik sağlar ve PostgreSQL sunucusu için tek bir Azure veritabanının yavaş sorgu günlüklerini indirir.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu örnek, Azure CLI Sürüm 2.0 veya sonraki sürümünü çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası
Bu örnek betik yönetici kullanıcı adı ve parola özelleştirmek için vurgulanan satırları değiştirin. < GÜNLÜK_DOSYASI_ADI > az İzleyici komutlar kendi server günlük dosyası adı ile değiştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Komut dosyası açıklaması
Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| **Komutu** | **Notlar** |
|---|---|
| [az grubu oluşturma](/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az postgres sunucu oluşturma](/cli/azure/postgres/server#az_msql_server_create) | Veritabanlarını barındıran bir PostgreSQL sunucusu oluşturur. |
| [az postgres sunucu yapılandırması listesi](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Bir sunucu için yapılandırma değerlerini listeler. |
| [az postgres sunucu yapılandırma kümesi](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Bir sunucu yapılandırmasını güncelleştirin. |
| [az postgres sunucu günlükleri listesi](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) | Bir sunucu için günlük dosyaları listelenmektedir. |
| [az postgres sunucu günlükleri indirme](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) | Günlük dosyalarını indirin. |
| [az grubu Sil](/cli/azure/group#az_group_delete) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgelerine](/cli/azure/overview).
- Ek komut dosyaları deneyin: [PostgreSQL Azure veritabanı için Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- [Yapılandırma ve erişim sunucusu Azure portalında oturum](../howto-configure-server-logs-in-portal.md)
