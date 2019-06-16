---
title: Döküm ve - tek bir sunucu PostgreSQL için Azure veritabanı'nda geri yükleme
description: Bir PostgreSQL veritabanı dökümü dosyasına ayıklama ve - tek bir sunucu PostgreSQL için Azure veritabanı'nda pg_dump oluşturan bir dosya geri yükleme işlemini açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: aa9485ec8fcabdc0276e0598bd3e19f04d70dfa1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65066978"
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


## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Verileri pg_restore kullanarak PostrgeSQL için hedef Azure veritabanını geri yükleyin
Hedef veritabanı oluşturduktan sonra döküm dosyasını hedef veritabanına veri geri yüklemek için pg_restore komut -d,--dbname parametresini kullanabilirsiniz.
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

## <a name="optimizing-the-migration-process"></a>Geçiş işlemi en iyi duruma getirme

Var olan PostgreSQL veritabanınızın PostgreSQL hizmeti için Azure veritabanı'na geçirmek için bir kaynak üzerinde veritabanını yedekleme ve Azure'da geri yoludur. Geçişi tamamlamak için gereken süreyi en aza indirmek için aşağıdaki parametreleri kullanarak yedeklemeyi göz önünde bulundurun ve komutları geri yükleyin.

> [!NOTE]
> Ayrıntılı sözdizimi için makale bilgi [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) ve [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).
>

### <a name="for-the-backup"></a>Yedekleme için
- Geri yüklemeyi hızlandırmak için paralel gerçekleştirebilmeniz -Fc anahtarıyla yedekleyin. Örneğin:

    ```
    pg_dump -h MySourceServerName -U MySourceUserName -Fc -d MySourceDatabaseName > Z:\Data\Backups\MyDatabaseBackup.dump
    ```

### <a name="for-the-restore"></a>Geri yüklemek için
- PostgreSQL sunucusu için geçiş yaptığınız ve ağ gecikme süresini azaltmak için o VM'den pg_restore yapmak için Azure veritabanı ile aynı bölgede bir Azure VM yedekleme dosyasını gitme öneririz. Ayrıca VM yaratılırken öneririz [accelerated networking](../virtual-network/create-vm-accelerated-networking-powershell.md) etkin.

- Varsayılan olarak yapılması gerekir, ancak veri ekleme sonra create INDEX deyimi doğrulamak için döküm dosyasını açın. Böyle değilse, veri eklendikten sonra create INDEX deyimi taşıyın.

- Anahtarlar geri yükleme -Fc ve -j *#* geri paralel hale getirmek için. *#* hedef sunucuda çekirdek sayısıdır. İle deneyebilirsiniz *#* yönelik etkisini öğrenmek için iki kez hedef sunucu çekirdek sayısı için ayarlayın. Örneğin:

    ```
    pg_restore -h MyTargetServer.postgres.database.azure.com -U MyAzurePostgreSQLUserName -Fc -j 4 -d MyTargetDatabase Z:\Data\Backups\MyDatabaseBackup.dump
    ```

- Komut ekleyerek döküm dosyasını düzenleyebilirsiniz *synchronous_commıt ayarlayın; =* başında ve komut *synchronous_commıt ayarlayın; =* sonunda. Uygulamaları verileri değiştirmeden önce sonunda, açma değil, sonraki veri kaybına neden olabilir.

- Hedefte veritabanı Azure PostgreSQL sunucusu için geri yüklemeden önce aşağıdakileri göz önünde bulundurun:
    - Geçiş sırasında bu İstatistikler gerekmediğinden sorgu performansı, izlemeyi devre dışı. Pg_stat_statements.track pg_qs.query_capture_mode ve pgms_wait_sampling.query_capture_mode NONE olarak ayarlayarak bunu yapabilirsiniz.

    - Yüksek işlem ve gibi 32 sanal çekirdek bellek için iyileştirilmiş, yüksek bellek sku geçişi hızlandırmak için kullanın. Geri yükleme tamamlandıktan sonra tercih edilen sku'nuz aşağı kolayca ölçeklendirebilirsiniz. Daha yüksek sku, daha fazla paralellik karşılık gelen artırarak elde edebileceğiniz `-j` pg_restore komut parametresi. 

    - Hedef sunucuda daha yüksek IOPS geri yükleme performansını geliştirebilirsiniz. Sunucunun depolama boyutunu artırarak daha yüksek IOPS sağlayabilirsiniz. Bu ayar, ters çevrilebilir değildir, ancak daha yüksek IOPS gerçek iş yükünüzü gelecekte yararlı olup olmadığını göz önünde bulundurun.

Test ve üretim ortamında kullanmadan önce bu komutları bir sınama ortamında doğrulamak unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
- Dışarı ve içeri aktarma kullanarak bir PostgreSQL veritabanına geçirmek için bkz [dışarı aktarma hizmetini kullanarak PostgreSQL veritabanınızı geçirme ve içeri aktarma](howto-migrate-using-export-and-import.md).
- Geçiş hakkında daha fazla bilgi için bkz, PostgreSQL için Azure veritabanı veritabanına [veritabanı Geçiş Kılavuzu](https://aka.ms/datamigration).
