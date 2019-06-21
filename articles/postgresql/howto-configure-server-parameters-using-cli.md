---
title: Hizmet parametreleri - tek bir sunucu PostgreSQL için Azure veritabanı'nda yapılandırma
description: Bu makalede hizmet parametreleri - Azure CLI komut satırını kullanarak tek bir sunucu PostgreSQL için Azure veritabanı'nda yapılandırma açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: f276247076438a03973148b5cf65ddbeb409b024
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274765"
---
# <a name="customize-server-configuration-parameters-for-azure-database-for-postgresql---single-server-using-azure-cli"></a>PostgreSQL - Azure CLI kullanarak tek bir sunucu için Azure veritabanı sunucu yapılandırma parametrelerini özelleştirme
Liste göstermek ve komut satırı arabirimi (Azure CLI) kullanarak Azure PostgreSQL sunucusu için yapılandırma parametreleri güncelleştirin. Bir alt kümesini altyapısı yapılandırmaları, sunucu düzeyinde kullanıma sunulan ve değiştirilebilir. 

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- PostgreSQL sunucusu için Azure veritabanı ve veritabanı oluşturma [PostgreSQL için Azure veritabanı oluşturma](quickstart-create-server-database-azure-cli.md)
- Yükleme [Azure CLI](/cli/azure/install-azure-cli) komut satırı arabirimini kullanın veya makine [Azure Cloud Shell](../cloud-shell/overview.md) tarayıcınızı kullanarak Azure portalında.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Sunucu Yapılandırma parametreleri için PostgreSQL sunucusu için Azure veritabanı listesi
Bir sunucu ve bunların değerlerini tüm değiştirilebilir parametreler listelemek için çalıştırma [az postgres server configuration listesi](/cli/azure/postgres/server/configuration) komutu.

Sunucusu için sunucu yapılandırma parametrelerini listeleyebilirsiniz **demosunucum.postgres.Database.Azure.com** kaynak grubu altında **myresourcegroup**.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mydemoserver
```
## <a name="show-server-configuration-parameter-details"></a>Sunucu Yapılandırma parametresi ayrıntılarını göster
Bir sunucu için belirli yapılandırma parametresi hakkındaki ayrıntıları göstermek için çalıştırma [az postgres server configuration show](/cli/azure/postgres/server/configuration) komutu.

Bu örnekte ayrıntılarını gösterir **günlük\_min\_iletileri** sunucusu için sunucu yapılandırma parametresi **demosunucum.postgres.Database.Azure.com** kaynak grubu altında **myresourcegroup.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-server-configuration-parameter-value"></a>Sunucu Yapılandırma parametresi değerini değiştirin
PostgreSQL server altyapısı için temel yapılandırma değerini güncelleştiren bir belirli sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmayı güncelleştirmek için [az postgres server configuration set](/cli/azure/postgres/server/configuration) komutu. 

Güncelleştirilecek **günlük\_min\_iletileri** sunucu yapılandırma parametresi sunucunun **demosunucum.postgres.Database.Azure.com** kaynak grubu altında  **myresourcegroup.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver --value INFO
```
Bir yapılandırma parametresi değeri sıfırlamak istiyorsanız, yalnızca isteğe bağlı olarak bırakmak seçtiğiniz `--value` parametresini ve service için varsayılan değer geçerlidir. Yukarıdaki örnekte, bunun gibi görünür:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mydemoserver
```
Bu komut sıfırlar **günlük\_min\_iletileri** varsayılan değerine yapılandırma **uyarı**. Sunucu Yapılandırması ve izin verilen değerler hakkında daha fazla bilgi için PostgreSQL belgeleri bakın [sunucu yapılandırması](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Sonraki adımlar
- [Sunucuyu yeniden öğrenin](howto-restart-server-cli.md)
- Yapılandırma ve sunucu günlüklerine erişmek için bkz: [PostgreSQL için Azure veritabanı'nda sunucu günlükleri](concepts-server-logs.md)
