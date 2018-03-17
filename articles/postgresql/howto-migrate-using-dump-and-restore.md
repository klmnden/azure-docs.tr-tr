---
title: "Döküm ve Azure veritabanında PostgreSQL için geri yükleme"
description: "Bir PostgreSQL veritabanı dökümü dosyasına ayıklayın ve Azure veritabanındaki pg_dump tarafından PostgreSQL için oluşturulan bir dosyadan geri açıklar."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: f74c60cb99ee5bae1af8e000ebbd21b41600638d
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Döküm ve geri yükleme kullanarak PostgreSQL veritabanınızı geçirin
Kullanabileceğiniz [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) PostgreSQL veritabanına dökümü dosyasına ayıklamak için ve [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) pg_dump tarafından oluşturulan bir arşiv dosyadan PostgreSQL veritabanını geri yüklemek için.

## <a name="prerequisites"></a>Önkoşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- Bir [PostgreSQL sunucu için Azure veritabanı](quickstart-create-server-database-portal.md) erişimini ve veritabanı altındaki izin veren güvenlik duvarı kuralları ile.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) ve [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) yüklü komut satırı yardımcı programları

Döküm ve PostgreSQL veritabanınızı geri yüklemek için aşağıdaki adımları izleyin:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Yüklenmesi için gerekli verileri içeren pg_dump kullanarak döküm dosyası oluşturma
Bir varolan PostgreSQL veritabanına şirket içinde veya bir VM yedeklemek için aşağıdaki komutu çalıştırın:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Örneğin, yerel bir sunucuya ve adlı bir veritabanı varsa **testdb** da
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Verileri hedef Azure veritabanı için PostrgeSQL pg_restore kullanarak geri yükleyin.
Hedef veritabanı oluşturduktan sonra pg_restore komut ve -d, hedef veritabanına döküm dosyadan verileri geri yüklemek için--dbname parametresini kullanabilirsiniz.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Tüm nesneleri oluşturma--kullanıcı adıyla belirtilen kullanıcı tarafından sahibi için geri yükleme sırasında yok-owner parametresi nedenler dahil. Daha fazla bilgi için üzerinde resmi PostgreSQL belgelerine bakın. [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

Bu örnekte, döküm dosyadan verileri geri yüklemek **testdb.dump** veritabanına **mypgsqldb** hedef sunucu üzerindeki **mydemoserver.postgres.database.azure.com**. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Sonraki adımlar
- Dışarı ve içeri aktarma kullanarak bir PostgreSQL veritabanı geçirmek için bkz [verme kullanarak PostgreSQL veritabanınızı geçirin ve içeri aktarma](howto-migrate-using-export-and-import.md)