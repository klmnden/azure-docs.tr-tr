---
title: "Erişim sunucusu, Azure CLI kullanarak MySQL için Azure veritabanı'nda oturum | Microsoft Docs"
description: "Bu makalede, Azure CLI komut satırı yardımcı programını kullanarak MySQL için Azure veritabanı sunucusu günlüklerine erişmek açıklar."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 10/18/2017
ms.openlocfilehash: 6ee2c2c6e6cff824d3167ea600a1ddc778ad011b
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Yapılandırma ve Azure CLI kullanarak sunucu günlüklerine erişme
Azure veritabanı için MySQL server günlüklerini Azure CLI, Azure'nın komut satırı yardımcı programını kullanarak yükleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
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
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server myserver4demo --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server myserver4demo --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server myserver4demo --value OFF
az mysql server configuration list --resource-group myresourcegroup --server myserver4demo
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı için liste günlükleri
Sunucunuz için kullanılabilir günlük dosyaları listelemek için Çalıştır [az mysql server günlüklerini listesi](/cli/azure/mysql/server-logs#list) komutu.

Sunucu için günlük dosyalarını listeleyebilirsiniz **myserver4demo.mysql.database.azure.com** kaynak grubu altında **myresourcegroup**hem de adlı bir metin dosyası doğrudan **günlük\_dosyaları \_list.txt.**
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server myserver4demo > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Günlükleri sunucudan indirme
[Az mysql server-günlükleri indirmek](/cli/azure/mysql/server-logs#download) komutu sunucunuz için ayrı günlük dosyalarını indirmek sağlar. 

Bu örnek sunucu için belirli günlük dosyasını karşıdan **myserver4demo.mysql.database.azure.com** kaynak grubu altında **myresourcegroup** yerel ortamınıza.
```azurecli-interactive
az mysql server-logs download --name 20170414-myserver4demo-mysql.log --resource-group myresourcegroup --server myserver4demo
```

## <a name="next-steps"></a>Sonraki Adımlar
- Hakkında bilgi edinin [sunucu, Azure veritabanı'nda MySQL için kaydeder](concepts-server-logs.md)
