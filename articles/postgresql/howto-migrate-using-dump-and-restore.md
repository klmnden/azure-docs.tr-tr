---
title: "Döküm ve Azure veritabanında PostgreSQL için geri yükleme | Microsoft Docs"
description: "Bir PostgreSQL veritabanı dökümü dosyasına ayıklayın ve Azure veritabanındaki pg_dump tarafından PostgreSQL için oluşturulan bir arşiv dosyasına PostgreSQL veritabanını geri açıklar."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: 28727117dbd37f9c595b488639a632b4c7404496
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Döküm ve geri yükleme kullanarak PostgreSQL veritabanınızı geçirin
Kullanabileceğiniz [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) PostgreSQL veritabanına dökümü dosyasına ayıklamak için ve [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) pg_dump tarafından oluşturulan bir arşiv dosyadan PostgreSQL veritabanını geri yüklemek için.

## <a name="prerequisites"></a>Ön koşullar
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
pg_restore -v –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
Bu örnekte, döküm dosyadan verileri geri yüklemek **testdb.dump** veritabanına **mypgsqldb** hedef sunucu üzerindeki **mypgserver 20170401.postgres.database.azure.com**.
```bash
pg_restore -v --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Sonraki adımlar
- Dışarı ve içeri aktarma kullanarak bir PostgreSQL veritabanı geçirmek için bkz [verme kullanarak PostgreSQL veritabanınızı geçirin ve içeri aktarma](howto-migrate-using-export-and-import.md)