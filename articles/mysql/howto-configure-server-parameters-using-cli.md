---
title: "Hizmet parametreleri Azure veritabanı'nda MySQL için yapılandırın."
description: "Bu makalede, Azure CLI komut satırı yardımcı programını kullanarak MySQL için Azure veritabanı'nda hizmet parametreleri yapılandırmak açıklar."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 9caf6b164f2433ab3c1b701554f562211cc2de80
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>Azure CLI kullanarak sunucu yapılandırma parametreleri özelleştirme
Listesinde, Göster ve Azure CLI, Azure komut satırı yardımcı programını kullanarak bir Azure veritabanı için MySQL sunucusu için yapılandırma parametreleri güncelleştirin. Bir alt kümesini altyapısı yapılandırmaları sunucu düzeyinde gösterilir ve değiştirilebilir. 

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- [MySQL sunucusu için bir Azure veritabanı](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI 2.0](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure bulut Kabuğu'nu kullanın.

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>Itanium tabanlı sistemler için liste sunucu yapılandırma parametreleri Azure veritabanı için MySQL sunucusu için
Tüm değiştirilebilir parametreler bir sunucu ve değerlerine listelemek için Çalıştır [az mysql server yapılandırma listesi](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) komutu.

Sunucusu için sunucu yapılandırma parametrelerini listeleyebilirsiniz **mydemoserver.mysql.database.azure.com** kaynak grubu altında **myresourcegroup**.
```azurecli-interactive
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```
MySQL başvuru bölümüne bakarak her listelenen parametrelerden biri tanımı [sunucu sistem değişkenleri](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html).

## <a name="show-server-configuration-parameter-details"></a>Sunucu yapılandırması parametre ayrıntıları göster
Bir sunucu için belirli yapılandırma parametresi hakkındaki ayrıntıları görüntülemek için Çalıştır [az mysql server yapılandırması Göster](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_show) komutu.

Bu örnek ayrıntılarını gösterir **yavaş\_sorgu\_günlük** sunucusu için sunucu yapılandırma parametresi **mydemoserver.mysql.database.azure.com** kaynakgrubundaki**myresourcegroup.**
```azurecli-interactive
az mysql server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-a-server-configuration-parameter-value"></a>Sunucu Yapılandırma parametresi değerini değiştirin
MySQL server altyapısı için temel yapılandırma değeri güncelleştiren bir belirli sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmasını güncelleştirmek için kullanmak [az mysql server yapılandırma kümesi](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) komutu. 

Güncelleştirilecek **yavaş\_sorgu\_günlük** sunucu yapılandırma parametresi sunucunun **mydemoserver.mysql.database.azure.com** kaynak grubu altında  **myresourcegroup.**
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```
Bir yapılandırma parametresinin değerini sıfırlamak istiyorsanız, isteğe bağlı atlayın `--value` parametre ve hizmet varsayılan değer geçerlidir. Yukarıdaki örnekte, aşağıdaki gibidir:
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
Bu kod sıfırlar **yavaş\_sorgu\_günlük** varsayılan değere yapılandırma **OFF**. 

## <a name="next-steps"></a>Sonraki adımlar
- Nasıl yapılandırılacağı [Azure portalında sunucu parametreleri](howto-server-parameters.md)