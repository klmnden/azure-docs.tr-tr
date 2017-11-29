---
title: "Azure CLI betik ölçekli Azure veritabanı için PostgreSQL | Microsoft Docs"
description: "Azure CLI komut dosyası örneği - PostgreSQL Sunucusu ölçümleri sorgulama sonra farklı performans düzeyi için ölçek Azure veritabanı."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 11/27/2017
ms.openlocfilehash: a5a24e9aeea193df28bd49d5c428a72b5ec75d1b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>İzleme ve Azure CLI kullanarak tek bir PostgreSQL sunucu ölçekleme
Bu örnek CLI betik ölçümleri sorgulama sonra PostgreSQL sunucusu farklı performans düzeyi için tek bir Azure veritabanı ölçeklendirir. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Örnek komut dosyası
Bu örnek betik yönetici kullanıcı adı ve parola özelleştirmek için vurgulanan satırları değiştirin. Kendi abonelik tanımlayıcısı az İzleyici komutlarıyla kullanılan Subscriptionıd değiştirin.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme
Komut dosyası örneği çalıştırdıktan sonra kaynak grubu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanılabilir.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Komut dosyası açıklaması
Bu komut dosyasını aşağıdaki komutları kullanır. Komut belirli belgeleri tablo bağlanan her komut.

| **Komutu** | **Notlar** |
|---|---|
| [az grubu oluşturma](/cli/azure/group#az_group_create) | Tüm kaynaklar depolandığı bir kaynak grubu oluşturur. |
| [az postgres sunucu oluşturma](/cli/azure/postgres/server#az_postgres_server_create) | Veritabanlarını barındıran bir PostgreSQL sunucusu oluşturur. |
| [az İzleyici ölçümleri listesi](/cli/azure/monitor/metrics#az_monitor_metrics_list) | Kaynaklar için ölçüm değeri listeleyin. |
| [az grubu Sil](/cli/azure/group#az_group_delete) | Tüm iç içe kaynaklar dahil olmak üzere bir kaynak grubu siler. |

## <a name="next-steps"></a>Sonraki adımlar
- Azure CLI hakkında daha fazla bilgi okuyun: [Azure CLI belgeleri](/cli/azure/overview)
- Ek komut dosyaları deneyin: [PostgreSQL Azure veritabanı için Azure CLI örnekleri](../sample-scripts-azure-cli.md)
- Ölçeklendirme hakkında daha fazla bilgi okuyun: [hizmet katmanları](../concepts-service-tiers.md) ve [işlem birimleri ve depolama birimleri](../concepts-compute-unit-and-storage.md)
