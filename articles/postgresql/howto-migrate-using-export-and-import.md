---
title: "İçe aktarma kullanarak bir veritabanını geçirme ve Azure veritabanı'nda PostgreSQL için dışarı aktarma | Microsoft Docs"
description: "Açıklar nasıl bir komut dosyasına bir PostgreSQL veritabanına ayıklayın ve bu dosyayı hedef veritabanından veri aktarın."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 5c3a642940bbaf766b87c74522a97b145632291f
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Dışarı aktarma kullanarak PostgreSQL veritabanınızı geçirin ve içeri aktarma
Kullanabileceğiniz [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) bir komut dosyasına bir PostgreSQL veritabanına ayıklamak için ve [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) bu dosyayı hedef veritabanından veri almak için.

## <a name="prerequisites"></a>Ön koşullar
Nasıl yapılır bu kılavuzu adım için gerekir:
- Bir [PostgreSQL sunucu için Azure veritabanı](quickstart-create-server-database-portal.md) erişimini ve veritabanı altındaki izin veren güvenlik duvarı kuralları ile.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) komut satırı yardımcı programının yüklü olması
- [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) komut satırı yardımcı programının yüklü olması

PostgreSQL veritabanına içeri ve dışarı aktarmak için aşağıdaki adımları izleyin.

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Yüklenmesi için gerekli verileri içeren pg_dump kullanarak bir komut dosyası oluşturma
Sql komut dosyası için bir VM içinde varolan ortamınızda aşağıdaki komutu çalıştırın veya varolan PostgreSQL veritabanı şirket içi dışa aktarmak için:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Örneğin, yerel bir sunucuya ve adlı bir veritabanı varsa **testdb** da:
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postrgesql"></a>PostrgeSQL için hedef Azure veritabanı üzerindeki verileri alma
Psql komut satırı ve -d, verileri Azure oluşturduğumuz PostrgeSQL için veritabanına ve veri sql dosyadan yüklemek için--dbname parametresini kullanabilirsiniz.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
Bu örnek psql yardımcı programı ve adlı bir komut dosyası kullanır **testdb.sql** önceki adımdaki verileri veritabanına **mypgsqldb** hedef sunucudaki  **mypgserver 20170401.postgres.database.azure.com**.
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a>Sonraki adımlar
- Bir PostgreSQL veritabanı dökümü ve geri yükleme kullanarak geçirmek için bkz [döküm ve geri yükleme kullanarak PostgreSQL veritabanınızı geçirin](howto-migrate-using-dump-and-restore.md)
