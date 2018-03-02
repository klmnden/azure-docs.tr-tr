---
title: "Erişim sunucusu Azure veritabanı'nda MySQL için Azure CLI kullanarak kaydeder."
description: "Bu makalede, Azure CLI komut satırı yardımcı programını kullanarak MySQL için Azure veritabanı sunucusu günlüklerine erişmek açıklar."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 8cd83722569eef503030b7e7438a73209cb812d6
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Yapılandırma ve Azure CLI kullanarak sunucu günlüklerine erişme
Azure veritabanı için MySQL server günlüklerini Azure CLI, Azure'nın komut satırı yardımcı programını kullanarak yükleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- [MySQL sunucusu için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI 2.0](/cli/azure/install-azure-cli) veya tarayıcıda Azure bulut Kabuğu'nu kullanın.

## <a name="configure-logging-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için günlük tutmayı yapılandırma
MySQL yavaş sorgu günlüğü erişmek için sunucu yapılandırabilirsiniz.
1. Ayarlayarak günlük özelliğini açmak **yavaş\_sorgu\_günlük** on parametresi.
2. Gibi diğer parametreleri ayarlamak **uzun\_sorgu\_zaman** ve **günlük\_yavaş\_yönetici\_deyimleri**.

Bkz: [sunucu parametreleri yapılandırmak için nasıl](howto-configure-server-parameters-using-cli.md) Azure CLI aracılığıyla bu parametreleri değerini ayarlama hakkında bilgi edinmek için.

Örneğin, aşağıdaki CLI komutu yavaş sorgu oturum açar, uzun sorgu süresi 10 saniye olarak ayarlar ve yavaş yönetici deyiminin günlüğünü kapatır. Son olarak, gözden geçirmeniz için yapılandırma seçeneklerini listeler.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı için liste günlükleri
Sunucunuz için kullanılabilir günlük dosyaları listelemek için Çalıştır [az mysql server günlüklerini listesi](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) komutu.

Sunucu için günlük dosyalarını listeleyebilirsiniz **mydemoserver.mysql.database.azure.com** kaynak grubu altında **myresourcegroup**hem de adlı bir metin dosyası doğrudan **günlük\_dosyaları \_list.txt.**
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Günlükleri sunucudan indirme
[Az mysql server-günlükleri indirmek](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) komutu sunucunuz için ayrı günlük dosyalarını indirmek sağlar. 

Bu örnek sunucu için belirli günlük dosyasını karşıdan **mydemoserver.mysql.database.azure.com** kaynak grubu altında **myresourcegroup** yerel ortamınıza.
```azurecli-interactive
az mysql server-logs download --name 20170414-mydemoserver-mysql.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Sonraki Adımlar
- Hakkında bilgi edinin [sunucu, Azure veritabanı'nda MySQL için kaydeder](concepts-server-logs.md)
