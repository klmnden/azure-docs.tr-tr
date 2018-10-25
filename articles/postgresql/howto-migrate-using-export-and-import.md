---
title: İçeri Aktar'ı kullanarak bir veritabanını geçirme ve PostgreSQL için Azure veritabanı'nda dışarı aktarma
description: Açıklayan nasıl bir komut dosyasına bir PostgreSQL veritabanı ayıklayın ve bu dosyayı hedef veritabanından veri aktarın.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 5f6a83e33ea443839059b267dfb8043ae48af039
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49987078"
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Dışarı aktarma hizmetini kullanarak PostgreSQL veritabanınızı geçirme ve içeri aktarma
Kullanabileceğiniz [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) bir komut dosyasına bir PostgreSQL veritabanı ayıklanacak ve [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) bu dosya hedef veritabanından veri almak için.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- Bir [PostgreSQL sunucusu için Azure veritabanı](quickstart-create-server-database-portal.md) erişimi ve veritabanı altındaki izin vermek için güvenlik duvarı kuralları ile.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) komut satırı yardımcı programının yüklü olması
- [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) komut satırı yardımcı programının yüklü olması

PostgreSQL veritabanınızın içeri ve dışarı aktarmak için aşağıdaki adımları izleyin.

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Yüklenen verileri içeren pg_dump kullanarak bir komut dosyası oluşturma
Sql komut dosyası için bir VM içinde mevcut ortamınızda aşağıdaki komutu çalıştırın ya da var olan PostgreSQL veritabanı şirket içi dışarı aktarma için:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Örneğin, yerel bir sunucuya ve adlı bir veritabanı varsa **testdb** da:
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı hedefte veri alma
Psql komut satırını ve--dbname parametresi kullanabilirsiniz (-d) sql dosyası PostgreSQL sunucusu ve yük verileri için Azure veritabanı'na veri almak için.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
Bu örnekte psql yardımcı programı ve adlı bir komut dosyası **testdb.sql** verileri veritabanına aktarmak için önceki adımdaki **mypgsqldb** hedef sunucudaki  **demosunucum.postgres.Database.Azure.com**.
```bash
psql --file=testdb.sql --host=mydemoserver.database.windows.net --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb
```

## <a name="next-steps"></a>Sonraki adımlar
- Döküm ve geri yükleme kullanarak bir PostgreSQL veritabanına geçirmek için bkz [döküm ve geri yükleme kullanarak PostgreSQL veritabanınızı geçirme](howto-migrate-using-dump-and-restore.md).
- Geçiş hakkında daha fazla bilgi için bkz, PostgreSQL için Azure veritabanı veritabanına [veritabanı Geçiş Kılavuzu](https://aka.ms/datamigration). 
