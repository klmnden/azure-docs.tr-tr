---
title: "İçe aktarma kullanarak bir veritabanını geçirme ve Azure veritabanı'nda PostgreSQL için dışarı aktarma"
description: "Açıklar nasıl bir komut dosyasına bir PostgreSQL veritabanına ayıklayın ve bu dosyayı hedef veritabanından veri aktarın."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 8726badde2214a0904336f5bc73310114bcf9e91
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Dışarı aktarma kullanarak PostgreSQL veritabanınızı geçirin ve içeri aktarma
Kullanabileceğiniz [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) bir komut dosyasına bir PostgreSQL veritabanına ayıklamak için ve [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) bu dosyayı hedef veritabanından veri almak için.

## <a name="prerequisites"></a>Önkoşullar
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

## <a name="import-the-data-on-target-azure-database-for-postgresql"></a>PostgreSQL için hedef Azure veritabanı üzerindeki verileri alma
Psql komut satırı ve--dbname parametre kullanabilirsiniz (-d) sql dosyasındaki PostgreSQL sunucu ve yük verileri için Azure veritabanına veri almak için.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
Bu örnek psql yardımcı programı ve adlı bir komut dosyası kullanır **testdb.sql** önceki adımdaki verileri veritabanına **mypgsqldb** hedef sunucudaki  **mydemoserver.postgres.Database.Azure.com**.
```bash
psql --file=testdb.sql --host=mydemoserver.database.windows.net --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb
```

## <a name="next-steps"></a>Sonraki adımlar
- Bir PostgreSQL veritabanı dökümü ve geri yükleme kullanarak geçirmek için bkz [döküm ve geri yükleme kullanarak PostgreSQL veritabanınızı geçirin](howto-migrate-using-dump-and-restore.md)
