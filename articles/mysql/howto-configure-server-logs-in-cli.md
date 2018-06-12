---
title: Erişim sunucusu, Azure CLI kullanarak MySQL için Azure veritabanı'nda oturum
description: Bu makalede, Azure CLI komut satırı yardımcı programını kullanarak MySQL için Azure veritabanı sunucusu günlüklerine erişmek açıklar.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 85c7840c0e919e77e807e6114c4d0c65601ff334
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265835"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Yapılandırma ve Azure CLI kullanarak sunucu günlüklerine erişme
Azure veritabanı MySQL server günlükleri için Azure CLI, Azure komut satırı yardımcı programını kullanarak yükleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- [MySQL sunucusu için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI 2.0](/cli/azure/install-azure-cli) veya Azure bulut Kabuk tarayıcıda

## <a name="configure-logging-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için günlük tutmayı yapılandırma
Aşağıdaki adımları uygulayarak MySQL yavaş sorgu günlüğü erişmek için sunucu yapılandırabilirsiniz:
1. Ayarlayarak günlük özelliğini açmak **yavaş\_sorgu\_günlük** on parametresi.
2. Diğer parametreler gibi ayarlamak **uzun\_sorgu\_zaman** ve **günlük\_yavaş\_yönetici\_deyimleri**.

Azure CLI aracılığıyla bu parametreleri değerini ayarlama hakkında bilgi edinmek için bkz: [sunucu parametreleri yapılandırmak nasıl](howto-configure-server-parameters-using-cli.md). 

Örneğin, aşağıdaki CLI komutu yavaş sorgu oturum açar, uzun sorgu süresi 10 saniye olarak ayarlar ve yavaş yönetici deyiminin günlüğünü kapatır. Son olarak, gözden geçirmeniz için yapılandırma seçeneklerini listeler.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı için liste günlükleri
Sunucunuz için kullanılabilir günlük dosyaları listelemek için Çalıştır [az mysql server günlüklerini listesi](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) komutu.

Sunucu için günlük dosyalarını listeleyebilirsiniz **mydemoserver.mysql.database.azure.com** kaynak grubu altında **myresourcegroup**. Adlı bir metin dosyasına günlük dosyalarının listesini doğrudan **günlük\_dosyaları\_list.txt**.
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Günlükleri sunucudan indirme
İle [az mysql server-günlükleri indirmek](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) komutu, sunucunuz için ayrı günlük dosyalarına yükleyebilir. 

Sunucu için belirli günlük dosyasını karşıdan yüklemek için aşağıdaki örneği kullanın **mydemoserver.mysql.database.azure.com** kaynak grubu altında **myresourcegroup** yerel ortamınıza.
```azurecli-interactive
az mysql server-logs download --name 20170414-mydemoserver-mysql.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [günlüklerini Azure veritabanı'nda MySQL için](concepts-server-logs.md).
