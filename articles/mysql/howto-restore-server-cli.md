---
title: Yedekleme ve MySQL için Azure veritabanı bir sunucuya geri yükleme
description: Yedekleme ve Azure CLI kullanarak Azure veritabanındaki bir sunucu için MySQL geri yükleme hakkında bilgi edinin.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 04/01/2018
ms.openlocfilehash: bd4ebbec4506824f00d09a09369ebbeaf9458c19
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="how-to-back-up-and-restore-a-server-in-azure-database-for-mysql-using-the-azure-cli"></a>Yedekleme ve Azure CLI kullanarak MySQL için Azure veritabanı bir sunucuya geri yükleme

## <a name="backup-happens-automatically"></a>Yedekleme otomatik olarak gerçekleşir
Azure veritabanı için MySQL sunucuları düzenli aralıklarla geri yükleme özelliklerini etkinleştirmek için yedeklenir. Bu özelliği kullanarak, sunucu ve tüm veritabanları için bir önceki noktası zaman içinde yeni bir sunucu üzerinde geri yükleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu tamamlamak için gerekir:
- Bir [MySQL server ve veritabanı için Azure veritabanı](quickstart-create-mysql-server-database-using-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

 

> [!IMPORTANT]
> Nasıl yapılır bu kılavuz, Azure CLI Sürüm 2.0 veya üstü kullanmanızı gerektirir. Azure CLI komut isteminde sürümünü onaylamak için girin `az --version`. Yüklemek veya yükseltmek için bkz: [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="add-the-extension"></a>Uzantıyı ekleme
Aşağıdaki komutu kullanarak güncelleştirilmiş MySQL için Azure Veritabanı yönetim uzantısını ekleyin:
```azurecli-interactive
az extension add --name rdbms
``` 

Doğru uzantı sürümünü yüklediğinizden emin olun. 
```azurecli-interactive
az extension list
```

Dönüş JSON'u aşağıdakileri içermelidir: 
```json
{
    "extensionType": "whl",
    "name": "rdbms",
    "version": "0.0.5"
}
```

Sürüm 0.0.5 döndürülmezse, aşağıdakini çalıştırarak uzantıyı güncelleştirin: 
```azurecli-interactive
az extension update --name rdbms
```


## <a name="set-backup-configuration"></a>Yedekleme kümesi yapılandırması

Sunucu oluşturma sırasında yerel olarak yedekli yedekleri veya coğrafi olarak yedekli yedekleme için sunucunuzu yapılandırma arasında seçim yapın. 

> [!NOTE]
> Bir sunucu oluşturulduğunda, sahip, artıklık tür coğrafi olarak yedekli vs yerel olarak yedekli olamaz değiştirdi.
>

Bir sunucu üzerinden oluşturulurken `az mysql server create` komutu, `--geo-redundant-backup` parametresi yedekleme artıklığı seçeneği karar verir. Varsa `Enabled`, coğrafi olarak yedekli yedeklemeleri alınır. Veya `Disabled` yerel olarak yedekli yedeklemeleri alınır. 

Yedekleme Bekletme dönemi parametresiyle ayarlanır `--backup-retention`. 

Oluşturma sırasında bu değerleri ayarlama hakkında daha fazla bilgi için bkz: [Azure veritabanı için MySQL server CLI Quickstart](quickstart-create-mysql-server-database-using-azure-cli.md).

Bir sunucu yedekleme saklama süresi gibi değiştirilebilir:

```azurecli-interactive
az mysql server update --name mydemoserver --resource-group myresourcegroup --backup-retention 10
```

Önceki örnekte mydemoserver yedekleme saklama süresi 10 gün olarak değiştirir.

Ne kadar geri yedeklemelerin kullanılabilir dayalı olduğundan zaman içinde nokta geri alınabilir zaman Yedekleme bekletme süresini yönetir. Zaman içinde nokta geri yükleme bir sonraki bölümde daha ayrıntılı açıklanmıştır.

## <a name="server-point-in-time-restore"></a>Sunucu zaman içinde nokta geri yükleme
Sunucu zaman içinde önceki bir noktaya geri yükleyebilirsiniz. Yeni bir sunucuya geri yüklenen veriler kopyalanır ve var olan sunucu olduğu gibi bırakılır. Örneğin, bir tablo bugün öğlen yanlışlıkla kesilirse zamana öğlen hemen önce geri yükleyebilirsiniz. Ardından, sunucunun geri yüklenen kopyadan eksik tablo ve veri alabilirsiniz. 

Sunucuyu geri yüklemek için Azure CLI kullanma [az mysql server geri](/cli/azure/mysql/server#az_mysql_server_restore) komutu.

### <a name="run-the-restore-command"></a>Restore komutu Çalıştır

Azure CLI komut isteminde, sunucuyu geri yüklemek için aşağıdaki komutu girin:

```azurecli-interactive
az mysql server restore --resource-group myresourcegroup --name mydemoserver-restored --restore-point-in-time 2018-03-13T13:59:00Z --source-server mydemoserver
```

`az mysql server restore` Komutu aşağıdaki parametreleri gerektirir:
| Ayar | Önerilen değer | Açıklama  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Kaynak sunucunun bulunduğu kaynak grubu.  |
| ad | mydemoserver-restored | Geri yükleme komutu tarafından oluşturulan yeni sunucunun adı. |
| restore-point-in-time | 2018-03-13T13:59:00Z | Geri yüklemek için zaman içinde bir nokta seçin. Bu tarih ve saat, kaynak sunucunun yedekleme saklama dönemi içinde olmalıdır. ISO8601 tarih ve saat biçimini kullanın. Örneğin, kendi yerel saat dilimi gibi kullanabilir `2018-03-13T05:59:00-08:00`. Örneğin, UTC Zulu dili biçimini kullanabilirsiniz `2018-03-13T13:59:00Z`. |
| source-server | mydemoserver | Geri yükleme kaynağı olarak kullanılacak sunucunun adı veya kimliği. |

Yeni bir sunucu, bir sunucu zaman içinde önceki bir noktaya geri yüklediğinizde oluşturulur. Özgün sunucunun ve zaman içinde belirtilen noktasından veritabanlarını yeni sunucuya kopyalanır.

Konum ve geri yüklenen sunucusunun fiyatlandırma katmanı değerleri özgün sunucusu ile aynı kalır. 

Geri yükleme işlemi tamamlandıktan sonra yeni sunucu bulun ve verileri beklendiği gibi geri yüklendiğini doğrulayın.

## <a name="geo-restore"></a>Coğrafi geri yükleme
Coğrafi olarak yedekli yedekleme için sunucunuzu yapılandırdıysanız, var olan sunucu yedekten yeni bir sunucu oluşturulabilir. Azure veritabanı için MySQL kullanılabilir herhangi bir bölgede bu yeni sunucu oluşturulabilir.  

Coğrafi olarak yedekli yedeklemeyi kullanarak bir sunucu oluşturmak için Azure CLI kullanma `az mysql server georestore` komutu.

> [!NOTE]
> Bir sunucu ilk oluşturulduğunda coğrafi geri yükleme için hemen kullanılabilir olmayabilir. Doldurulması gerekli meta veriler için birkaç saat sürebilir.
>

Coğrafi geri yükleme sunucu Azure CLI komut isteminde aşağıdaki komutu girin:

```azurecli-interactive
az mysql server georestore --resource-group myresourcegroup --name mydemoserver-georestored --source-server mydemoserver --location eastus --sku-name GP_Gen4_8 
```
Bu komut adlı yeni bir sunucu oluşturur *mydemoserver georestored* ait Doğu ABD *myresourcegroup*. Genel amaçlı, 8 vCores Gen 4 sunucusuyla olan. Sunucu coğrafi olarak yedekli yedekten oluşturulmuş *mydemoserver*, olduğu Ayrıca kaynak grubunda *myresourcegroup*

Farklı bir kaynak grubu mevcut sunucudan yeni sunucuda, ardından oluşturmak istiyorsanız `--source-server` nitelemek aşağıdaki örnekteki gibi sunucu adını parametre:

```azurecli-interactive
az mysql server georestore --resource-group newresourcegroup --name mydemoserver-georestored --source-server "/subscriptions/$<subscription ID>/resourceGroups/$<resource group ID>/providers/Microsoft.DBforMySQL/servers/mydemoserver" --location eastus --sku-name GP_Gen4_8

```

`az mysql server georestore` Komutu requies aşağıdaki parametreleri:
| Ayar | Önerilen değer | Açıklama  |
| --- | --- | --- |
|resource-group| myresourcegroup | Yeni Sunucu ait olacağı kaynak grubunun adı.|
|ad | mydemoserver georestored | Yeni sunucunun adıdır. |
|source-server | mydemoserver | Coğrafi olarak yedekli yedekleri kullanılan mevcut sunucunun adıdır. |
|location | eastus | Yeni sunucu konumu. |
|SKU adı| GP_Gen4_8 | Bu parametre, fiyatlandırma katmanı, işlem oluşturma ve yeni sunucunun vCores sayısını ayarlar. GP_Gen4_8 genel amaçlı, 8 vCores Gen 4 sunucusuyla eşleştirir.|


>[!Important]
>Yeni bir sunucu coğrafi geri yükleme ile oluştururken, kaynak sunucu ile aynı depolama boyutu ve fiyatlandırma katmanı devralır. Bu değerler, oluşturma sırasında değiştirilemez. Yeni Sunucu oluşturulduktan sonra depolama boyutu ölçeklendirilebilir.

Geri yükleme işlemi tamamlandıktan sonra yeni sunucu bulun ve verileri beklendiği gibi geri yüklendiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetin hakkında daha fazla bilgi [yedeklemeleri](concepts-backup.md).
- Daha fazla bilgi edinmek [iş sürekliliği](concepts-business-continuity.md) seçenekleri.
