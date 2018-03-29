---
title: Yapılandırma ve Azure CLI kullanarak PostgreSQL için sunucu günlüklerine erişme
description: Bu makalede, yapılandırma ve Azure CLI komut satırını kullanarak Azure veritabanı sunucu günlüklerinde PostgreSQL için erişim açıklar.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 951dcca562c08698b4ce4528d005fc91152ea337
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Yapılandırma ve Azure CLI kullanarak sunucu günlüklerine erişme
Komut satırı arabirimi (Azure CLI) kullanarak PostgreSQL server Hata günlüklerini indirebilirsiniz. Ancak, işlem günlükleri erişimi desteklenmiyor. 

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- [Azure veritabanı PostgreSQL sunucu için](quickstart-create-server-database-azure-cli.md)
- [Azure CLI 2.0](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya Azure bulut Kabuk tarayıcıda

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Azure veritabanı için günlük kaydını PostgreSQL için yapılandırma
Sorgu ve hata günlükleri erişmek için sunucu yapılandırabilirsiniz. Hata günlüklerini otomatik vakum, bağlantı ve denetim noktası bilgileri olabilir.
1. Günlük özelliğini açın.
2. Sorgu günlüğü etkinleştirmek için güncelleştirme **günlük\_deyimi** ve **günlük\_min\_süresi\_deyimi**.
3. Saklama dönemi güncelleştirin.

Daha fazla bilgi için bkz: [sunucu yapılandırma parametreleri özelleştirme](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Azure veritabanı için liste günlüklerini PostgreSQL sunucusu
Sunucunuz için kullanılabilir günlük dosyaları listelemek için Çalıştır [az postgres sunucu günlükleri listesi](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) komutu.

Sunucu için günlük dosyalarını listeleyebilirsiniz **mydemoserver.postgres.database.azure.com** kaynak grubu altında **myresourcegroup**. Adlı bir metin dosyasına günlük dosyalarının listesini doğrudan **günlük\_dosyaları\_list.txt**.
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Günlükleri sunucusundan yerel olarak yükle
İle [az postgres server-günlükleri indirmek](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) komutu, sunucunuz için ayrı günlük dosyalarına yükleyebilir. 

Sunucu için belirli günlük dosyasını karşıdan yüklemek için aşağıdaki örneği kullanın **mydemoserver.postgres.database.azure.com** kaynak grubu altında **myresourcegroup** yerel ortamınıza.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Sonraki adımlar
- Sunucu günlükleri hakkında daha fazla bilgi için bkz: [günlüklerini Azure veritabanı'nda PostgreSQL için](concepts-server-logs.md).
- Sunucu parametreleri hakkında daha fazla bilgi için bkz: [Azure CLI kullanarak sunucu yapılandırma parametreleri özelleştirmek](howto-configure-server-parameters-using-cli.md).
