---
title: "Yapılandırma ve Azure CLI kullanarak PostgreSQL için sunucu günlüklerine erişim | Microsoft Docs"
description: "Bu makalede, yapılandırmak ve Azure CLI komut satırını kullanarak PostgreSQL için Azure veritabanı sunucusu günlüklerine erişmek açıklar."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: 5e10ddd8c850d457b4ad77cd5ea4d92edc07017e
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Yapılandırma ve Azure CLI kullanarak sunucu günlüklerine erişme
Komut satırı arabirimi (Azure CLI) kullanarak PostgreSQL server Hata günlüklerini indirebilirsiniz. Ancak, işlem günlükleri için erişim desteklenmiyor. 

## <a name="prerequisites"></a>Ön koşullar
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
Sunucunuz için kullanılabilir günlük dosyaları listelemek için Çalıştır [az postgres sunucu günlükleri listesi](/cli/azure/postgres/server-logs#list) komutu.

Sunucu için günlük dosyalarını listeleyebilirsiniz **mypgserver 20170401.postgres.database.azure.com** kaynak grubu altında **myresourcegroup**hem de adlı bir metin dosyası doğrudan **günlük\_dosyaları\_list.txt.**
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Günlükleri sunucusundan yerel olarak yükle
[Az postgres server-günlükleri indirmek](/cli/azure/postgres/server-logs#download) komutu sunucunuz için ayrı günlük dosyalarını indirmek sağlar. 

Bu örnek sunucu için belirli günlük dosyasını karşıdan **mypgserver 20170401.postgres.database.azure.com** kaynak grubu altında **myresourcegroup** yerel ortamınıza.
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a>Sonraki adımlar
- Sunucu günlükleri hakkında daha fazla bilgi için bkz: [PostgreSQL için Azure veritabanında sunucu günlükleri](concepts-server-logs.md)
- Sunucu parametreleri hakkında daha fazla bilgi için bkz: [Azure CLI kullanarak sunucu yapılandırma parametreleri özelleştirme](howto-configure-server-parameters-using-cli.md)
