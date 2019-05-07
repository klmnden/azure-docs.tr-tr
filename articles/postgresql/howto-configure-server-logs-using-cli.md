---
title: Yapılandırma ve PostgreSQL - Azure CLI kullanarak tek sunucu için sunucu günlüklerine erişme
description: Bu makalede, PostgreSQL - Azure CLI komut satırını kullanarak tek sunucu için Azure veritabanı'nda sunucu günlüklerini erişmek ve yapılandırma açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 4702db31ffeb15481584b9638f5be1aa640ff39e
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067201"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Yapılandırma ve Azure CLI kullanarak sunucu günlüklerine erişme
Komut satırı arabirimi (Azure CLI) kullanarak PostgreSQL sunucusu hata günlüklerini indirebilirsiniz. Ancak, işlem günlükleri erişimi desteklenmiyor. 

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- [PostgreSQL sunucusu için Azure veritabanı](quickstart-create-server-database-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure Cloud Shell

## <a name="configure-logging"></a>Günlük tutmayı yapılandırma
Sorgu ve hata günlükleri erişmek için sunucu yapılandırabilirsiniz. Hata günlüklerini otomatik elektrikli, bağlantı ve denetim noktası bilgilerini sağlayabilirsiniz.
1. Günlük özelliğini açar.
2. Sorgu günlük kaydını etkinleştirmek için güncelleştirme **günlük\_deyimi** ve **günlük\_min\_süresi\_deyimi**.
3. Saklama dönemi güncelleştirin.

Daha fazla bilgi için [sunucu yapılandırma parametrelerini özelleştirme](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs"></a>Liste günlükleri
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
