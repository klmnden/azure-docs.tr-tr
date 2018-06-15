---
title: Hizmet parametreleri Azure veritabanı'nda PostgreSQL için yapılandırın.
description: Bu makalede, Azure CLI komut satırını kullanarak PostgreSQL için Azure veritabanı'nda hizmet parametreleri yapılandırmak açıklar.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: df32578c92b1973568c9b811cb7f9e9db067915f
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
ms.locfileid: "29691402"
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Azure CLI kullanarak sunucu yapılandırma parametreleri özelleştirme
Liste, Göster ve komut satırı arabirimi (Azure CLI) kullanarak bir Azure PostgreSQL sunucusu için yapılandırma parametrelerini güncelleştirin. Bir alt kümesini altyapısı yapılandırmaları sunucu düzeyinde gösterilir ve değiştirilebilir. 

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- Azure veritabanı PostgreSQL sunucu için veritabanı izleyerek oluşturup [PostgreSQL için bir Azure veritabanı oluşturma](quickstart-create-server-database-azure-cli.md)
- Yükleme [Azure CLI 2.0](/cli/azure/install-azure-cli) komut satırı arabirimi makinede veya kullanım [Azure bulut Kabuk](../cloud-shell/overview.md) tarayıcınızı kullanarak Azure portalında.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Itanium tabanlı sistemler için liste sunucu yapılandırma parametreleri Azure veritabanı için PostgreSQL sunucusu
Tüm değiştirilebilir parametreler bir sunucu ve değerlerine listelemek için Çalıştır [az postgres sunucu yapılandırması listesi](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) komutu.

Sunucusu için sunucu yapılandırma parametrelerini listeleyebilirsiniz **mydemoserver.postgres.database.azure.com** kaynak grubu altında **myresourcegroup**.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mydemoserver
```
## <a name="show-server-configuration-parameter-details"></a>Sunucu yapılandırması parametre ayrıntıları göster
Bir sunucu için belirli yapılandırma parametresi hakkındaki ayrıntıları görüntülemek için Çalıştır [az postgres sunucu yapılandırması Göster](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_show) komutu.

Bu örnek ayrıntılarını gösterir **günlük\_min\_iletileri** sunucusu için sunucu yapılandırma parametresi **mydemoserver.postgres.database.azure.com** kaynak grubu altında **myresourcegroup.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-server-configuration-parameter-value"></a>Sunucu Yapılandırma parametresi değerini değiştirin
PostgreSQL sunucu altyapısı için temel yapılandırma değeri güncelleştiren bir belirli sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmasını güncelleştirmek için kullanmak [az postgres sunucu yapılandırma kümesi](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) komutu. 

Güncelleştirilecek **günlük\_min\_iletileri** sunucu yapılandırma parametresi sunucunun **mydemoserver.postgres.database.azure.com** kaynak grubu altında  **myresourcegroup.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver --value INFO
```
Bir yapılandırma parametresinin değerini sıfırlamak istiyorsanız, yalnızca isteğe bağlı olarak bırakmak seçtiğiniz `--value` parametre ve hizmet varsayılan değer geçerlidir. İçinde Yukarıdaki örnek, onu gibi görünür:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
Bu komut sıfırlar **günlük\_min\_iletileri** varsayılan değere yapılandırma **uyarı**. Sunucu yapılandırmasını ve izin verilen değerler hakkında daha fazla bilgi için üzerinde PostgreSQL belgelerine bakın. [sunucu yapılandırması](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Sonraki adımlar
- Yapılandırma ve sunucu günlüklerini erişmek için bkz: [PostgreSQL için Azure veritabanında sunucu günlükleri](concepts-server-logs.md)
