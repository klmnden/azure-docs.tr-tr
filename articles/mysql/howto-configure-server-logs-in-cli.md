---
title: Erişim sunucusu, Azure CLI kullanarak MySQL için Azure veritabanı'nda oturum
description: Bu makalede, Azure CLI komut satırı yardımcı programını kullanarak MySQL için Azure veritabanı'nda sunucu günlüklerini erişmeye açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 207e9965f6600477e1df93845bc41bd33b5c028c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60525473"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Yapılandırma ve Azure CLI kullanarak sunucu günlüklerine erişme
Sunucu günlükleri MySQL için Azure veritabanı Azure CLI, Azure komut satırı yardımcı programını kullanarak indirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- [MySQL sunucusu için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) veya tarayıcıda Azure Cloud Shell

## <a name="configure-logging-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için günlük tutmayı yapılandırma
MySQL yavaş sorgu günlüğü aşağıdaki adımları izleyerek erişmek için sunucu yapılandırabilirsiniz:
1. Günlük özelliğini açar ayarlayarak **yavaş\_sorgu\_günlük** on parametresi.
2. Diğer parametreler gibi ayarlayın **uzun\_sorgu\_zaman** ve **günlük\_yavaş\_yönetici\_deyimleri**.

Azure CLI aracılığıyla bu parametre değeri hakkında bilgi edinmek için bkz: [sunucu parametrelerini yapılandırma](howto-configure-server-parameters-using-cli.md). 

Örneğin, aşağıdaki CLI komutunu yavaş sorgu oturum açar, uzun sorgu süresi 10 saniye olarak ayarlar ve ardından yavaş yönetici deyiminin günlüğünü kapatır. Son olarak, gözden geçirmeniz için yapılandırma seçeneklerini listeler.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server mydemoserver --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server mydemoserver --value OFF
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı için liste günlükleri
Sunucunuz için kullanılabilir günlük dosyaları için Çalıştır [az mysql server-logs listesi](/cli/azure/mysql/server-logs#az-mysql-server-logs-list) komutu.

Sunucu için günlük dosyalarını listeleyebilirsiniz **demosunucum.MySQL.Database.Azure.com** kaynak grubu altında **myresourcegroup**. Ardından adlı bir metin dosyasına günlük dosyalarının listesi doğrudan **günlük\_dosyaları\_list.txt**.
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Sunucudan günlükleri indir
İle [az mysql server-logs indirme](/cli/azure/mysql/server-logs#az-mysql-server-logs-download) komutunu sunucunuz için ayrı günlük dosyalarına karşıdan yükleyebilirsiniz. 

Sunucu için belirli günlük dosyasını indirmek için aşağıdaki örneği kullanın **demosunucum.MySQL.Database.Azure.com** kaynak grubu altında **myresourcegroup** yerel ortamınıza.
```azurecli-interactive
az mysql server-logs download --name 20170414-mydemoserver-mysql.log --resource-group myresourcegroup --server mydemoserver
```

## <a name="next-steps"></a>Sonraki adımlar
- Hakkında bilgi edinin [server, MySQL için Azure veritabanı'nda oturum](concepts-server-logs.md).
