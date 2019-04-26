---
title: Yapılandırma ve Azure CLI kullanarak PostgreSQL için sunucu günlüklerine erişme
description: Bu makalede, Azure CLI komut satırını kullanarak PostgreSQL için Azure veritabanı'nda sunucu günlüklerini erişmek ve yapılandırma açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 3ca77c1ffa5f5a3f384009299701f4aa674baf59
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60421212"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Yapılandırma ve Azure CLI kullanarak sunucu günlüklerine erişme
Komut satırı arabirimi (Azure CLI) kullanarak PostgreSQL sunucusu hata günlüklerini indirebilirsiniz. Ancak, işlem günlükleri erişimi desteklenmiyor. 

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- [PostgreSQL sunucusu için Azure veritabanı](quickstart-create-server-database-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure Cloud Shell

## <a name="configure-logging-for-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı için günlük tutmayı yapılandırma
Sorgu ve hata günlükleri erişmek için sunucu yapılandırabilirsiniz. Hata günlüklerini otomatik elektrikli, bağlantı ve denetim noktası bilgilerini sağlayabilirsiniz.
1. Günlük özelliğini açar.
2. Sorgu günlük kaydını etkinleştirmek için güncelleştirme **günlük\_deyimi** ve **günlük\_min\_süresi\_deyimi**.
3. Saklama dönemi güncelleştirin.

Daha fazla bilgi için [sunucu yapılandırma parametrelerini özelleştirme](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>PostgreSQL sunucusu için Azure veritabanı için liste günlükleri
Sunucunuz için kullanılabilir günlük dosyaları için Çalıştır [az postgres server-logs listesi](/cli/azure/postgres/server-logs) komutu.

Sunucu için günlük dosyalarını listeleyebilirsiniz **demosunucum.postgres.Database.Azure.com** kaynak grubu altında **myresourcegroup**. Ardından adlı bir metin dosyasına günlük dosyalarının listesi doğrudan **günlük\_dosyaları\_list.txt**.
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Günlükleri sunucudan yerel olarak indir
İle [az postgres server-logs indirme](/cli/azure/postgres/server-logs) komutunu sunucunuz için ayrı günlük dosyalarına karşıdan yükleyebilirsiniz. 

Sunucu için belirli günlük dosyasını indirmek için aşağıdaki örneği kullanın **demosunucum.postgres.Database.Azure.com** kaynak grubu altında **myresourcegroup** yerel ortamınıza.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Sonraki adımlar
- Sunucu günlükleri hakkında daha fazla bilgi için bkz: [sunucusu, PostgreSQL için Azure veritabanı'nda oturum](concepts-server-logs.md).
- Sunucu parametreleri hakkında daha fazla bilgi için bkz: [Azure CLI kullanarak sunucu yapılandırma parametrelerini özelleştirme](howto-configure-server-parameters-using-cli.md).
