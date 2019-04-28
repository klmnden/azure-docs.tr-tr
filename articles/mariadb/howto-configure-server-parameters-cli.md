---
title: Hizmet parametreleri MariaDB için Azure veritabanı'nda yapılandırma
description: Bu makalede hizmet parametreleri Azure CLI komut satırı yardımcı programını kullanarak MariaDB için Azure veritabanı'nda yapılandırma açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 11/09/2018
ms.openlocfilehash: 4e0bf45f1c67a5e07d6ed632f6560d094b673c0a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61040072"
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>Azure CLI kullanarak sunucu yapılandırma parametrelerini özelleştirme
Liste, Göster ve Azure CLI, Azure komut satırı yardımcı programını kullanarak MariaDB server için Azure veritabanı için yapılandırma parametreleri güncelleştirin. Bir alt kümesini altyapısı yapılandırmaları, sunucu düzeyinde kullanıma sunulmuştur ve değiştirilebilir.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- [MariaDB server için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure Cloud Shell kullanın.

## <a name="list-server-configuration-parameters-for-azure-database-for-mariadb-server"></a>Sunucu yapılandırma parametrelerini MariaDB sunucusu için Azure veritabanı listesi
Bir sunucu ve bunların değerlerini tüm değiştirilebilir parametreler listelemek için çalıştırma [az mariadb sunucu yapılandırma listesi](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-list) komutu.

Sunucusu için sunucu yapılandırma parametrelerini listeleyebilirsiniz **mydemoserver.mariadb.database.azure.com** kaynak grubu altında **myresourcegroup**.
```azurecli-interactive
az mariadb server configuration list --resource-group myresourcegroup --server mydemoserver
```

MariaDB başvuru bölümüne bakın her listelenen parametrelerin tanımı için [sunucu sistem değişkenleri](https://mariadb.com/kb/en/library/server-system-variables/).

## <a name="show-server-configuration-parameter-details"></a>Sunucu Yapılandırma parametresi ayrıntılarını göster
Bir sunucu için belirli yapılandırma parametresi hakkındaki ayrıntıları göstermek için çalıştırma [az mariadb server configuration show](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-show) komutu.

Bu örnekte ayrıntılarını gösterir **yavaş\_sorgu\_günlük** sunucusu için sunucu yapılandırma parametresi **mydemoserver.mariadb.database.azure.com** kaynak grubu altında **myresourcegroup.**
```azurecli-interactive
az mariadb server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

## <a name="modify-a-server-configuration-parameter-value"></a>Sunucu Yapılandırma parametresi değerini değiştirin
MariaDB sunucu altyapısı için temel yapılandırma değerini güncelleştiren bir belirli sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmayı güncelleştirmek için [az mariadb server configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) komutu. 

Güncelleştirilecek **yavaş\_sorgu\_günlük** sunucu yapılandırma parametresi sunucunun **mydemoserver.mariadb.database.azure.com** kaynak grubu altında  **myresourcegroup.**
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```

Bir yapılandırma parametresi değeri sıfırlamak istiyorsanız, isteğe bağlı atlamak `--value` parametresini ve service için varsayılan değer geçerlidir. Yukarıdaki örnekte, bunun gibi görünür:
```azurecli-interactive
az mariadb server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```

Bu kod sıfırlar **yavaş\_sorgu\_günlük** varsayılan değerine yapılandırma **OFF**. 

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresi ile çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarını doldurma

Saat dilimi tabloları sunucunuzdaki çağırarak doldurulabilir `az_load_timezone` saklı yordamdan MariaDB komut satırı veya MariaDB Workbench gibi bir araç.

> [!NOTE]
> Çalıştırıyorsanız `az_load_timezone` ilk güvenli güncelleştirme modunu kapat gerekebilir MariaDB Workbench'ten komutunu kullanarak `SET SQL_SAFE_UPDATES=0;`.

```sql
CALL mysql.az_load_timezone();
```

Kullanılabilir saat dilimi değerlerini görüntülemek için aşağıdaki komutu çalıştırın:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Genel bir düzeyinde saat dilimi ayarlama

Genel bir düzeyinde saat dilimi kullanılarak ayarlanabilir [az mariadb server configuration set](/cli/azure/mariadb/server/configuration#az-mariadb-server-configuration-set) komutu.

Aşağıdaki komutu kullanarak güncelleştirmeleri **zaman\_bölge** sunucu yapılandırma parametresi sunucunun **mydemoserver.mariadb.database.azure.com** kaynak grubu altında  **myresourcegroup** için **ABD / Pasifik**.

```azurecli-interactive
az mariadb server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>Oturum düzeyi saat dilimi ayarlama

Oturum düzeyi saat dilimi çalıştırarak ayarlanabilir `SET time_zone` MariaDB komut satırı veya MariaDB Workbench gibi bir araçla komutu. Aşağıdaki örnekte saat dilimini ayarlar **ABD / Pasifik** saat dilimi.  

```sql
SET time_zone = 'US/Pacific';
```

MariaDB belgelerine başvurmak [tarih ve saat işlevleri](https://mariadb.com/kb/en/library/date-time-functions/).

## <a name="next-steps"></a>Sonraki adımlar

- Yapılandırma [Azure portalında sunucu parametreleri](howto-server-parameters.md)
