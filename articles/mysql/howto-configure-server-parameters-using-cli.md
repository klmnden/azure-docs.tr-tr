---
title: Hizmet parametreleri, MySQL için Azure veritabanı'nda yapılandırma
description: Bu makalede hizmet parametreleri Azure CLI komut satırı yardımcı programını kullanarak MySQL için Azure veritabanı'nda yapılandırma açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 07/18/2018
ms.openlocfilehash: b0d7bbdc3e1dcad6f6cecb57b15e2e5df6b3fd28
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "60525455"
---
# <a name="customize-server-configuration-parameters-by-using-azure-cli"></a>Azure CLI kullanarak sunucu yapılandırma parametrelerini özelleştirme
Liste göstermek ve Azure CLI, Azure komut satırı yardımcı programını kullanarak MySQL için Azure veritabanı için yapılandırma parametreleri güncelleştirin. Bir alt kümesini altyapısı yapılandırmaları, sunucu düzeyinde kullanıma sunulmuştur ve değiştirilebilir. 

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- [MySQL sunucusu için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI](/cli/azure/install-azure-cli) komut satırı yardımcı programı veya tarayıcıda Azure Cloud Shell kullanın.

## <a name="list-server-configuration-parameters-for-azure-database-for-mysql-server"></a>Sunucu Yapılandırma parametreleri için Azure veritabanı için MySQL server listesi.
Bir sunucu ve bunların değerlerini tüm değiştirilebilir parametreler listelemek için çalıştırma [az mysql server configuration listesi](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-list) komutu.

Sunucusu için sunucu yapılandırma parametrelerini listeleyebilirsiniz **demosunucum.MySQL.Database.Azure.com** kaynak grubu altında **myresourcegroup**.
```azurecli-interactive
az mysql server configuration list --resource-group myresourcegroup --server mydemoserver
```
Her listelenen parametrelerin tanımı için üzerinde MySQL başvuru bölümüne bakın. [sunucu sistem değişkenleri](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html).

## <a name="show-server-configuration-parameter-details"></a>Sunucu Yapılandırma parametresi ayrıntılarını göster
Bir sunucu için belirli yapılandırma parametresi hakkındaki ayrıntıları göstermek için çalıştırma [az mysql server configuration show](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-show) komutu.

Bu örnekte ayrıntılarını gösterir **yavaş\_sorgu\_günlük** sunucusu için sunucu yapılandırma parametresi **demosunucum.MySQL.Database.Azure.com** kaynakgrubunda**myresourcegroup.**
```azurecli-interactive
az mysql server configuration show --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
## <a name="modify-a-server-configuration-parameter-value"></a>Sunucu Yapılandırma parametresi değerini değiştirin
MySQL server altyapısı için temel yapılandırma değerini güncelleştiren bir belirli sunucu yapılandırma parametresinin değerini de değiştirebilirsiniz. Yapılandırmayı güncelleştirmek için [az mysql server configuration set](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) komutu. 

Güncelleştirilecek **yavaş\_sorgu\_günlük** sunucu yapılandırma parametresi sunucunun **demosunucum.MySQL.Database.Azure.com** kaynak grubu altında  **myresourcegroup.**
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver --value ON
```
Bir yapılandırma parametresi değeri sıfırlamak istiyorsanız, isteğe bağlı atlamak `--value` parametresini ve service için varsayılan değer geçerlidir. Yukarıdaki örnekte, bunun gibi görünür:
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server mydemoserver
```
Bu kod sıfırlar **yavaş\_sorgu\_günlük** varsayılan değerine yapılandırma **OFF**. 

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresi ile çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarını doldurma

Saat dilimi tabloları sunucunuzdaki çağırarak doldurulabilir `az_load_timezone` saklı yordamdan MySQL komut satırı veya MySQL Workbench gibi bir araç.

> [!NOTE]
> Çalıştırıyorsanız `az_load_timezone` ilk güvenli güncelleştirme modunu kapat gerekebilir MySQL Workbench'ten komutunu kullanarak `SET SQL_SAFE_UPDATES=0;`.

```sql
CALL mysql.az_load_timezone();
```

Kullanılabilir saat dilimi değerlerini görüntülemek için aşağıdaki komutu çalıştırın:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Genel bir düzeyinde saat dilimi ayarlama

Genel bir düzeyinde saat dilimi kullanılarak ayarlanabilir [az mysql server configuration set](/cli/azure/mysql/server/configuration#az-mysql-server-configuration-set) komutu.

Aşağıdaki komutu kullanarak güncelleştirmeleri **zaman\_bölge** sunucu yapılandırma parametresi sunucunun **demosunucum.MySQL.Database.Azure.com** kaynak grubu altında  **myresourcegroup** için **ABD / Pasifik**.

```azurecli-interactive
az mysql server configuration set --name time_zone --resource-group myresourcegroup --server mydemoserver --value "US/Pacific"
```

### <a name="setting-the-session-level-time-zone"></a>Oturum düzeyi saat dilimi ayarlama

Oturum düzeyi saat dilimi çalıştırarak ayarlanabilir `SET time_zone` MySQL komut satırı veya MySQL Workbench gibi bir araçla komutu. Aşağıdaki örnekte saat dilimini ayarlar **ABD / Pasifik** saat dilimi.  

```sql
SET time_zone = 'US/Pacific';
```

MySQL belgeleri için başvurmak [tarih ve saat işlevleri](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz).


## <a name="next-steps"></a>Sonraki adımlar

- Yapılandırma [Azure portalında sunucu parametreleri](howto-server-parameters.md)