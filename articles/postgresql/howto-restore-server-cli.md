---
title: "Yedekleme ve Azure veritabanındaki bir sunucu için PostgreSQL geri yükleme"
description: "Yedekleme ve Azure CLI kullanarak Azure veritabanındaki bir sunucu için PostgreSQL geri yükleme hakkında bilgi edinin."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 69dfde7e54a271caabc6d0909565165fb219c7f2
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="how-to-backup-and-restore-a-server-in-azure-database-for-postgresql-by-using-the-azure-cli"></a>Yedekleme ve Azure CLI kullanarak PostgreSQL için Azure veritabanı bir sunucuya geri yükleme

Azure veritabanı PostgreSQL için bir sunucu veritabanı 7'den 35 güne yayılmış önceki bir tarihe geri yüklemek için kullanın.

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu tamamlamak için gerekir:
- Bir [PostgreSQL sunucu ve veritabanı için Azure veritabanı](quickstart-create-server-database-azure-cli.md)

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


> [!IMPORTANT]
> Yükleyin ve Azure CLI yerel olarak kullanırsanız, bu yapılır Kılavuzu Azure CLI Sürüm 2.0 veya üstü kullanmanızı gerektirir. Azure CLI komut isteminde sürümünü onaylamak için girin `az --version`. Yüklemek veya yükseltmek için bkz: [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="backup-happens-automatically"></a>Yedekleme otomatik olarak gerçekleşir
Azure veritabanı için PostgreSQL kullandığınızda, veritabanı hizmeti yedekleme hizmetinin her 5 dakikada bir otomatik olarak yapar. 

Temel katman için yedeklemeler 7 gün için kullanılabilir. Standart katmanı için yedeklemeleri 35 gün için kullanılabilir. Daha fazla bilgi için bkz: [Azure veritabanı fiyatlandırma katmanlarına PostgreSQL için](concepts-pricing-tiers.md).

Bu otomatik yedekleme özelliği ile sunucu ve veritabanlarını önceki bir tarihe geri yükleyin veya zamanında gelin.

## <a name="restore-a-database-to-a-previous-point-in-time-by-using-the-azure-cli"></a>Bir veritabanı, Azure CLI kullanarak zaman içinde önceki bir noktaya geri
Sunucu, zaman içinde önceki bir noktaya geri yüklemenizi Azure veritabanı PostgreSQL için kullanın. Yeni bir sunucuya geri yüklenen veriler kopyalanır ve var olan sunucu olduğu gibi bırakılır. Örneğin, bir tablo bugün öğlen yanlışlıkla kesilirse zamana öğlen hemen önce geri yükleyebilirsiniz. Ardından, sunucunun geri yüklenen kopyadan eksik tablo ve veri alabilirsiniz. 

Sunucuyu geri yüklemek için Azure CLI kullanma [az postgres server geri](/cli/azure/postgres/server#az_postgres_server_restore) komutu.

### <a name="run-the-restore-command"></a>Restore komutu Çalıştır

Azure CLI komut isteminde, sunucuyu geri yüklemek için aşağıdaki komutu girin:

```azurecli-interactive
az postgres server restore --resource-group myresourcegroup --server mydemoserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mydemoserver
```

`az postgres server restore` Komutu aşağıdaki parametreleri gerektirir:
| Ayar | Önerilen değer | Açıklama  |
| --- | --- | --- |
| kaynak grubu |  myresourcegroup |  Kaynak sunucunun bulunduğu kaynak grubu.  |
| ad | mydemoserver-restored | Geri yükleme komutu tarafından oluşturulan yeni sunucunun adıdır. |
| zaman içinde geri yükleme noktası | 2017-04-13T13:59:00Z | Geri yüklemek için zaman içinde bir nokta seçin. Bu tarih ve saat kaynak sunucunun yedekleme saklama dönemi içinde olmalıdır. ISO8601 tarih ve saat biçimini kullanın. Örneğin, kendi yerel saat dilimi gibi kullanabilir `2017-04-13T05:59:00-08:00`. Örneğin, UTC Zulu dili biçimini kullanabilirsiniz `2017-04-13T13:59:00Z`. |
| source-server | mydemoserver | Adı veya geri yüklemek için kaynak sunucunun kimliği. |

Yeni bir sunucu, bir sunucu zaman içinde önceki bir noktaya geri yüklediğinizde oluşturulur. Özgün sunucunun ve zaman içinde belirtilen noktasından veritabanlarını yeni sunucuya kopyalanır.

Konum ve geri yüklenen sunucusunun fiyatlandırma katmanı değerleri özgün sunucusu ile aynı kalır. 

`az postgres server restore` Komut zaman uyumlu olur. Sunucunun geri yüklendikten sonra onu yeniden işlemi için farklı bir noktaya zamanında yinelemek için kullanabilirsiniz. 

Geri yükleme işlemi tamamlandıktan sonra yeni sunucu bulun ve verileri beklendiği gibi geri yüklendiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure veritabanı PostgreSQL için için bağlantı kitaplıkları](concepts-connection-libraries.md)
