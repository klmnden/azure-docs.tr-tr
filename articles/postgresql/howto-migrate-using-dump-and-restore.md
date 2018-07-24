---
title: Döküm ve PostgreSQL için Azure veritabanı'nda geri yükleme
description: Bir PostgreSQL veritabanı dökümü dosyasına ayıklama ve PostgreSQL için Azure veritabanı'nda pg_dump oluşturan bir dosya geri yükleme işlemini açıklar.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/19/2018
ms.openlocfilehash: 94d196ceecc0b63b9f0b0fe94f71363dc2086c30
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213659"
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Döküm ve geri yükleme kullanarak PostgreSQL veritabanınızı geçirme
Kullanabileceğiniz [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) bir döküm dosyası bir PostgreSQL veritabanı ayıklanacak ve [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) PostgreSQL veritabanı pg_dump tarafından oluşturulan bir arşiv dosyasını geri.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- Bir [PostgreSQL sunucusu için Azure veritabanı](quickstart-create-server-database-portal.md) erişimi ve veritabanı altındaki izin vermek için güvenlik duvarı kuralları ile.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) ve [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) yüklü komut satırı yardımcı programları

Döküm ve PostgreSQL veritabanınızın geri yüklemek için aşağıdaki adımları izleyin:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Yüklenen verileri içeren pg_dump kullanarak bir döküm dosyası oluşturma
Bir mevcut PostgreSQL veritabanı şirket içinde veya bir sanal makine yedeklemek için aşağıdaki komutu çalıştırın:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Örneğin, yerel bir sunucuya ve adlı bir veritabanı varsa **testdb** da
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

> [!IMPORTANT]
> Yedekleme dosyalarını Azure bir blob/deposuna kopyalamak ve buradan Internet üzerinden geri yükleme işlemi daha çok daha hızlı olması gereken, geri yükleme gerçekleştirin.
> 

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Verileri pg_restore kullanarak PostrgeSQL için hedef Azure veritabanını geri yükleyin
Hedef veritabanı oluşturulduktan sonra pg_restore komut ve -d, döküm dosyasını hedef veritabanına veri geri yükleme için--dbname parametresini kullanabilirsiniz.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Dahil olmak üzere tüm nesneleri oluşturulan--username belirtilen kullanıcı tarafından ait olduğu geri yükleme işlemi sırasında no-owner parametresi neden olur. Daha fazla bilgi için üzerinde resmi PostgreSQL belgelerine bakın. [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

> [!NOTE]
> PostgreSQL sunucunuz, SSL bağlantıları gerektiriyorsa (üzerinde PostgreSQL sunucuları için Azure veritabanı'nda varsayılan olarak), bir ortam değişkenini ayarlamak `PGSSLMODE=require` böylece pg_restore aracı SSL ile bağlanır. SSL okuma hatası  `FATAL:  SSL connection is required. Please specify SSL options and retry.`
>
> Komutu Windows komut satırında çalıştırın `SET PGSSLMODE=require` pg_restore komutu çalıştırmadan önce. Linux veya Bash komut çalıştırma `export PGSSLMODE=require` pg_restore komutu çalıştırmadan önce.
>

Bu örnekte, döküm dosyadan verileri geri **testdb.dump** veritabanına **mypgsqldb** hedef sunucuda **demosunucum.postgres.Database.Azure.com**. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Sonraki adımlar
- Dışarı ve içeri aktarma kullanarak bir PostgreSQL veritabanına geçirmek için bkz [dışarı aktarma hizmetini kullanarak PostgreSQL veritabanınızı geçirme ve içeri aktarma](howto-migrate-using-export-and-import.md).
- Geçiş hakkında daha fazla bilgi için bkz, PostgreSQL için Azure veritabanı veritabanına [veritabanı Geçiş Kılavuzu](http://aka.ms/datamigration).
