---
title: Yedekleme ve MariaDB için Azure veritabanı'nda bir sunucu geri yükleme
description: Yedekleme ve Azure CLI kullanarak MariaDB için Azure veritabanı'nda bir sunucuya geri yükleme hakkında bilgi edinin.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 11/10/2018
ms.openlocfilehash: 409fe7b76306036cad19980459ca718c87118d8f
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66171380"
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mariadb-using-the-azure-cli"></a>Azure CLI kullanarak MariaDB için Azure veritabanı'nda bir sunucu geri nasıl

## <a name="backup-happens-automatically"></a>Yedekleme otomatik olarak gerçekleşir

Sunucuları düzenli aralıklarla geri yükleme özellikleri etkinleştirmek için yedeklenen MariaDB için Azure veritabanı. Bu özelliği kullanarak, sunucuyu ve tüm veritabanlarında için bir önceki-belirli bir noktaya, yeni bir sunucuya geri yükleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunda tamamlanması gerekir:

- Bir [MariaDB sunucu ve veritabanı için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Bu nasıl yapılır kılavuzunda, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Azure CLI komut isteminde sürümünü onaylamak için girin `az --version`. Yüklemek veya yükseltmek için bkz: [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="set-backup-configuration"></a>Yedekleme kümesi yapılandırması

Sunucu oluşturma sırasında yerel olarak yedekli yedeklemeler veya coğrafi olarak yedekli yedekleme için sunucunuzu yapılandırma arasında seçim yapmanızı ister.

> [!NOTE]
> Sahip, yedeklilik türünü bir sunucu oluşturulduktan sonra yerel olarak yedekli, coğrafi olarak yedekli vs moduna geçiş yapılamaz.
>

Sunucu üzerinden oluşturulurken `az mariadb server create` komutu `--geo-redundant-backup` parametresi yedekleme Yedeklilik seçeneğinizi karar verir. Varsa `Enabled`, coğrafi olarak yedekli yedeklemeleri alınır. Veya `Disabled` yerel olarak yedekli yedeklemeleri alınır.

Yedekleme saklama dönemi parametresi tarafından ayarlanmış `--backup-retention`.

Oluşturma sırasında bu değerleri ayarlama hakkında daha fazla bilgi için bkz. [MariaDB sunucu CLI Hızlı Başlangıç için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-cli.md).

Bir sunucunun yedekleme saklama dönemi şu şekilde değiştirilebilir:

```azurecli-interactive
az mariadb server update --name mydemoserver --resource-group myresourcegroup --backup-retention 10
```

Yukarıdaki örnekte demosunucum yedekleme bekletme süresi 10 gün olarak değiştirir.

Yedekleme bekletme süresi ne kadar geri mevcut yedekleme dayalı olduğundan zaman içinde nokta geri alınabilir zaman yönetir. Belirli bir noktaya geri yükleme, sonraki bölümde daha ayrıntılı açıklanmıştır.

## <a name="server-point-in-time-restore"></a>Sunucu zaman içinde nokta geri yükleme

Sunucu zaman içinde önceki bir noktaya geri yükleyebilirsiniz. Yeni bir sunucuya geri yüklenen veriler kopyalanır ve sunucunun mevcut olduğu gibi bırakılır. Örneğin, bir tabloyu yanlışlıkla bugün öğlen kesilirse, zamana gösterimlerini hemen önce geri yükleyebilirsiniz. Ardından, geri yüklenen sunucu kopyasından eksik tablo ve veri alabilirsiniz.

Sunucuyu geri yüklemek için Azure CLI kullanmak [az mariadb sunucusu geri yüklemesi](/cli/azure/mariadb/server#az-mariadb-server-restore) komutu.

### <a name="run-the-restore-command"></a>Geri yükleme komutunu çalıştırın

Azure CLI komut isteminde sunucuyu geri yüklemek için aşağıdaki komutu girin:

```azurecli-interactive
az mariadb server restore --resource-group myresourcegroup --name mydemoserver-restored --restore-point-in-time 2018-03-13T13:59:00Z --source-server mydemoserver
```

`az mariadb server restore` Komut takip eden parametreleri gerektiriyor:

| Ayar | Önerilen değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Kaynak sunucunun bulunduğu kaynak grubu.  |
| ad | mydemoserver-restored | Geri yükleme komutu tarafından oluşturulan yeni sunucunun adı. |
| restore-point-in-time | 2018-03-13T13:59:00Z | Bir noktaya geri yüklemek için saat seçin. Bu tarih ve saat, kaynak sunucunun yedekleme saklama dönemi içinde olmalıdır. ISO8601 tarih ve saat biçimini kullanın. Örneğin, kendi yerel saat dilimi gibi kullanabileceğiniz `2018-03-13T05:59:00-08:00`. Örneğin, şeklindeki UTC Zulu biçimini kullanabilirsiniz `2018-03-13T13:59:00Z`. |
| source-server | mydemoserver | Geri yükleme kaynağı olarak kullanılacak sunucunun adı veya kimliği. |

Bir sunucu zaman içinde önceki bir noktaya geri yüklediğinizde, yeni bir sunucu oluşturulur. Özgün sunucuda ve veritabanlarında belirtilen zaman noktasından yeni sunucuya kopyalanır.

Konum ve fiyatlandırma katmanı değerleri geri yüklenen sunucu için özgün sunucu ile aynı kalır.

Geri yükleme işlemi tamamlandıktan sonra yeni sunucuyu bulun ve verilerin beklenen şekilde geri yüklendiğini doğrulayın.

## <a name="geo-restore"></a>Coğrafi geri yükleme

Coğrafi olarak yedekli yedekleme için sunucunuzu yapılandırdıysanız, bu var olan bir sunucuyu yedekten yeni bir sunucu oluşturulabilir. Bu yeni sunucu MariaDB için Azure veritabanı kullanılabilir herhangi bir bölgede oluşturulabilir.  

Coğrafi olarak yedekli bir yedeklemesini kullanarak bir sunucu oluşturmak için Azure CLI kullanmak `az mariadb server georestore` komutu.

> [!NOTE]
> Bir sunucu ilk oluşturulduğunda coğrafi geri yükleme için hemen kullanılabilir olmayabilir. Bu doldurulması gerekli meta veriler için birkaç saat sürebilir.
>

Coğrafi geri yükleme sunucusundaki Azure CLI komut isteminde aşağıdaki komutu girin:

```azurecli-interactive
az mariadb server georestore --resource-group myresourcegroup --name mydemoserver-georestored --source-server mydemoserver --location eastus --sku-name GP_Gen5_8
```

Bu komut, adı verilen yeni bir sunucu oluşturur. *demosunucum georestored* ait Doğu ABD *myresourcegroup*. Bu, genel amaçlı, 5. nesil server 8 sanal çekirdekli olur. Sunucu, coğrafi olarak yedekli yedeğini oluşturulan *demosunucum*, olduğu da kaynak grubunda *myresourcegroup*

Mevcut bir sunucudan farklı bir kaynak grubunda yeni sunucuya, ardından oluşturmak istiyorsanız `--source-server` uygun sunucu adını aşağıdaki örnekte olduğu gibi parametre:

```azurecli-interactive
az mariadb server georestore --resource-group newresourcegroup --name mydemoserver-georestored --source-server "/subscriptions/$<subscription ID>/resourceGroups/$<resource group ID>/providers/Microsoft.DBforMariaDB/servers/mydemoserver" --location eastus --sku-name GP_Gen5_8

```

`az mariadb server georestore` Komut takip eden parametreleri gerektiriyor:

| Ayar | Önerilen değer | Açıklama  |
| --- | --- | --- |
|resource-group| myresourcegroup | Kaynak grubunun adı için yeni sunucuya ait.|
|ad | mydemoserver-georestored | Yeni sunucunun adı. |
|source-server | mydemoserver | Coğrafi olarak yedekli yedekleri kullanılan mevcut sunucu adı. |
|location | eastus | Yeni sunucu konumu. |
|sku-name| GP_Gen5_8 | Bu parametre, fiyatlandırma katmanı, işlem oluşturma ve yeni sunucunun sanal çekirdek sayısını ayarlar. Genel amaçlı, 5. nesil server 8 sanal çekirdekli GP_Gen5_8 eşlenir.|

>[!Important]
>Yeni bir sunucu coğrafi geri yükleme ile oluştururken, kaynak sunucuyla aynı depolama boyutuna ve fiyatlandırma katmanını devralır. Bu değerler, oluşturma sırasında değiştirilemez. Yeni Sunucu oluşturulduktan sonra depolama boyutu en çok ölçeklendirilebilir.

Geri yükleme işlemi tamamlandıktan sonra yeni sunucuyu bulun ve verilerin beklenen şekilde geri yüklendiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet hakkında daha fazla bilgi [yedeklemeleri](concepts-backup.md).
- Daha fazla bilgi edinin [iş sürekliliği](concepts-business-continuity.md) seçenekleri.
