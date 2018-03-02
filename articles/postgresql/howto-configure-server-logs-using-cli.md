---
title: "Yapılandırma ve Azure CLI kullanarak PostgreSQL için sunucu günlüklerine erişme"
description: "Bu makalede, yapılandırmak ve Azure CLI komut satırını kullanarak PostgreSQL için Azure veritabanı sunucusu günlüklerine erişmek açıklar."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: e12a8907b641b4591ed5ff9fdd5d8458eb75525e
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Yapılandırma ve Azure CLI kullanarak sunucu günlüklerine erişme
Komut satırı arabirimi (Azure CLI) kullanarak PostgreSQL server Hata günlüklerini indirebilirsiniz. Ancak, işlem günlükleri için erişim desteklenmiyor. 

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- Bir [PostgreSQL sunucu için Azure veritabanı](quickstart-create-server-database-azure-cli.md)
- Yükleme [Azure CLI 2.0](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure bulut Kabuğu'nu kullanın.

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Azure veritabanı için günlük kaydını PostgreSQL için yapılandırma
Sorgu ve hata günlükleri erişmek için sunucu yapılandırabilirsiniz. Hata günlüklerini otomatik vakum, bağlantı ve kontrol noktaları bilgiler içerebilir.
1. Günlüğünü etkinleştirme
2. Güncelleştirme günlük\_deyimi ve günlük\_min\_süresi\_sorgu günlüğü etkinleştirmek için deyimi
3. Saklama dönemi güncelleştir

Daha fazla bilgi için bkz: [sunucu yapılandırma parametreleri özelleştirme](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Azure veritabanı için liste günlüklerini PostgreSQL sunucusu
Sunucunuz için kullanılabilir günlük dosyaları listelemek için Çalıştır [az postgres sunucu günlükleri listesi](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) komutu.

Sunucu için günlük dosyalarını listeleyebilirsiniz **mydemoserver.postgres.database.azure.com** kaynak grubu altında **myresourcegroup**hem de adlı bir metin dosyası doğrudan **günlük\_ dosyaları\_list.txt.**
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Günlükleri sunucusundan yerel olarak yükle
[Az postgres server-günlükleri indirmek](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) komutu sunucunuz için ayrı günlük dosyalarını indirmek sağlar. 

Bu örnek sunucu için belirli günlük dosyasını karşıdan **mydemoserver.postgres.database.azure.com** kaynak grubu altında **myresourcegroup** yerel ortamınıza.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Sonraki adımlar
- Sunucu günlükleri hakkında daha fazla bilgi için bkz: [PostgreSQL için Azure veritabanında sunucu günlükleri](concepts-server-logs.md)
- Sunucu parametreleri hakkında daha fazla bilgi için bkz: [Azure CLI kullanarak sunucu yapılandırma parametreleri özelleştirme](howto-configure-server-parameters-using-cli.md)
