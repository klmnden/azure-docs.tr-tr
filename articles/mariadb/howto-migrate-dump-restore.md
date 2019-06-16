---
title: Döküm kullanarak MariaDB veritabanınızı geçirme ve MariaDB için Azure veritabanı'nda geri yükleme
description: Bu makalede PHPMyAdmin mysqldump ve MySQL Workbench gibi araçları kullanarak, MariaDB için Azure veritabanı veritabanlarını geri iki yaygın yolları açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: bcb76fcbba02bf53b48cc462e3dad8f264db02ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60745969"
---
# <a name="migrate-your-mariadb-database-to-azure-database-for-mariadb-using-dump-and-restore"></a>MariaDB veritabanı, döküm ve geri yükleme kullanarak MariaDB için Azure veritabanı'na geçirme
Bu makalede, MariaDB için Azure veritabanı veritabanlarını geri iki yaygın yolları açıklanmaktadır.
- Döküm ve geri yükleme komut satırından (mysqldump kullanarak) 
- Döküm ve PHPMyAdmin kullanarak geri yükleme

## <a name="before-you-begin"></a>Başlamadan önce
Bu nasıl yapılır kılavuzunda adımlamak için sahip olmanız gerekir:
- [MariaDB sunucu - Azure portalı için Azure veritabanı oluşturma](quickstart-create-mariadb-server-database-using-azure-portal.md)
- [mysqldump](https://mariadb.com/kb/en/library/mysqldump/) komut satırı yardımcı programı bir makinede yüklü.
- MySQL Workbench [MySQL Workbench'i indir](https://dev.mysql.com/downloads/workbench/), kurbağa, Navicat veya diğer üçüncü taraf MySQL aracından dökümü ve komutlarını geri yükleme.

## <a name="use-common-tools"></a>Ortak Araçlar kullanın
Uzaktan bağlanmak ve verileri MariaDB için Azure veritabanı'na geri yüklemek için genel yardımcı programları ve MySQL Workbench, mysqldump, kurbağa veya Navicat gibi araçları kullanın. MariaDB için Azure veritabanı'na bağlanmak için bir internet bağlantısı olan istemci makinenizde gibi araçlar kullanın. Bir SSL şifreli bağlantı için en iyi güvenlik uygulamalarını kullanın, ayrıca bkz: [SSL bağlantısını yapılandırma MariaDB için Azure veritabanı'nda](concepts-ssl-connection-security.md). MariaDB için Azure veritabanı'na geçirirken döküm dosyaları herhangi bir özel bulutun konumuna taşımak gerekmez. 

## <a name="common-uses-for-dump-and-restore"></a>Döküm ve geri yükleme için yaygın kullanımları
MariaDB sunucu çeşitli ortak senaryolar için Azure veritabanı'na MySQL yardımcı programları mysqldump ve dump ve load veritabanlarına mysqlpump gibi kullanabilir. 

<!--In other scenarios, you may use the [Import and Export](howto-migrate-import-export.md) approach instead.-->

- Tüm veritabanını geçirirken veritabanı kullanmak dökümünü yapar. Bu öneri, büyük miktarda veri taşırken ya da Canlı siteler veya uygulamalar için hizmet kesintilerini en aza indirmek istediğiniz zaman barındırır. 
-  MariaDB için Azure veritabanı'na veri yükleme zaman veritabanındaki tüm tabloları Innodb depolama altyapısı kullandığınızdan emin olun. MariaDB için Azure veritabanı, yalnızca Innodb depolama altyapısını destekler ve alternatif depolama altyapılarını desteklemez. Tablolarınızı diğer depolama altyapıları ile yapılandırıldıysa, MariaDB için Azure veritabanı geçiş işleminden önce Innodb altyapısı biçimine dönüştürün.
   WordPress veya MyISAM tabloları kullanarak Webapp'i varsa, örneğin, önce bu tablolarda MariaDB için Azure veritabanı'na geri yüklemeden önce Innodb biçime geçiş yaparak dönüştürün. Yan tümcesini kullanın `ENGINE=InnoDB` yeni bir tablo oluştururken kullanılan altyapısının ayarlamak için ardından veri geri yüklemeden önce uyumlu tabloya aktarın. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- Uyumluluk sorunları önlemek için MariaDB sürümüyle aynı sürümü kaynak ve hedef sistemlerde veritabanları çıkarırken kullanılan emin olun. Mevcut MariaDB sunucunuzu sürümü 10.2 ise, örneğin, daha sonra Azure veritabanı'na 10.2 sürümünü çalıştırmak üzere yapılandırılmış MariaDB için geçirmeniz gerekir. `mysql_upgrade` Komut MariaDB server için Azure veritabanı'nda çalışmaz ve desteklenmez. MariaDB sürümleri arasında yükseltme gerekiyorsa, ilk döküm veya alt veritabanı sürümünü daha yüksek bir sürümü MariaDB kendi ortamınızda verin. Ardından çalıştırın `mysql_upgrade`, MariaDB için Azure veritabanı'na geçişi denemeden önce.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar
Performansı iyileştirmek için bu ölçütlerin bildirimi büyük veritabanları çıkarırken uygulayın:
-   Kullanım `exclude-triggers` veritabanları çıkarırken mysqldump seçeneği. Tetikleyici tetikleme veri geri yükleme sırasında tetikleyici komutları önlemek için döküm dosyalarını hariç tutun. 
-   Kullanım `single-transaction` REPEATABLE READ işlem yalıtım modu ayarlamak için seçeneği ve bir başlangıç işlem SQL deyimi veri dökme önce sunucuya gönderir. Tek bir işlem içinde birçok tabloları dökme bazı ek depolama alanı geri yükleme sırasında tüketilen neden olur. `single-transaction` Seçeneği ve `lock-tables` seçeneği karşılıklı olarak birbirini dışlar kilit tabloları örtük olarak kabul edilebilmesi için işlem bekleyen neden olduğu. Döküm büyük tablolar için birleştirerek `single-transaction` seçeneğini `quick` seçeneği. 
-   Kullanım `extended-insert` birkaç değer listesi içeren birden çok satır içi söz dizimi. Bu daha küçük bir döküm dosyasında sonuçları ve ekler dosya yeniden yüklendiğinde hızlandırır.
-  Kullanım `order-by-primary` veritabanları, böylece verileri birincil anahtarı sırasına komut dosyası çıkarırken mysqldump seçeneği.
-   Kullanım `disable-keys` veri çıkarırken mysqldump yük önce yabancı anahtar kısıtlamalarını devre dışı bırakma seçeneği. Yabancı anahtar denetimleri devre dışı bırakılması performans artışı sağlar. Kısıtlamaları etkinleştirin ve tutarlılığını sağlamak için yüklemeden sonra verileri doğrulayın.
-   Uygun olduğunda bölümlenmiş tabloları kullanın.
-   Paralel veri yükleyin. Kaynak sınırına neden, ve Azure portalında mevcut olan ölçümler kullanarak kaynakları izlemek çok fazla paralellik kaçının. 
-   Kullanım `defer-table-indexes` veritabanları, böylece dizin oluşturma tabloları veri yüklendikten sonra gerçekleşir çıkarırken mysqlpump seçeneği.
-   Yedekleme dosyalarını Azure bir blob/deposuna kopyalamak ve buradan Internet üzerinden geri yükleme işlemi daha çok daha hızlı olması gereken, geri yükleme gerçekleştirin.

## <a name="create-a-backup-file"></a>Bir yedekleme dosyası oluşturma
Mevcut bir MariaDB veritabanı yerel şirket içi sunucuda veya bir sanal makine yedeklemek için mysqldump kullanarak şu komutu çalıştırın: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Sağlamak için Parametreler şunlardır:
- [uname] Veritabanı kullanıcı adı 
- [başarılı] Veritabanınızın parolası (-p ile parola arasında boşluk olmadığından olmasına dikkat edin) 
- [dbname] Veritabanınızın adı 
- [backupfile.sql] veritabanı yedekleme için dosya adı 
- [--iyileştirilmiş] Mysqldump seçeneği 

Örneğin, 'testuser' kullanıcı adı ve parola için bir dosya testdb_backup.sql ile MariaDB sunucunuzda ' testdb' adlı bir veritabanı yedeklemek için aşağıdaki komutu kullanın. Komut yedekler `testdb` adlı bir dosya veritabanına `testdb_backup.sql`, veritabanını yeniden oluşturmak için gereken tüm SQL deyimlerini içerir. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
Veritabanı yedekleme için belirli tablolar seçmek için tablo adlarının boşluklarla ayırarak listeleyin. Örneğin, 'testdb' yalnızca tablo1 ve tablo2 tablolarının tabloları için bu örnek izleyin: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```
Aynı anda birden fazla veritabanını yedeklemek için kullanın veritabanına geçin ve boşluklarla ayırarak veritabanı adları listesi. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```
Sunucudaki tüm veritabanları için aynı anda yedeklemek için kullanmalısınız tüm veritabanlarını seçeneği.
```bash
$ mysqldump -u root -p --all-databases > alldb_backup.sql 
```

## <a name="create-a-database-on-the-target-server"></a>Hedef sunucuda bir veritabanı oluşturun
' % S'hedef Azure veritabanını, verileri geçirmek istediğiniz MariaDB sunucusu için boş bir veritabanı oluşturun. Veritabanını oluşturmak için MySQL Workbench, kurbağa veya Navicat gibi bir araç kullanın. Veritabanı olan veritabanı Dökümü alınan veriler içeriyor veya farklı bir adla bir veritabanı oluşturabileceğiniz gibi aynı ada sahip olabilir.

Bağlanmak için bağlantı bilgilerini bulun **genel bakış** MariaDB için Azure veritabanı'nın.

![Azure portalında bağlantı bilgilerini Bul](./media/howto-migrate-dump-restore/1_server-overview-name-login.png)

Bağlantı bilgilerini, MySQL Workbench uygulamasına ekleyin.

![MySQL Workbench bağlantı dizesi](./media/howto-migrate-dump-restore/2_setup-new-connection.png)

## <a name="restore-your-mariadb-database"></a>MariaDB veritabanı geri yükleme
Hedef veritabanı oluşturulduktan sonra yeni oluşturulan veritabanına döküm dosyasından özel içine verileri geri yüklemek için mysql komut veya MySQL Workbench kullanabilirsiniz.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
Bu örnekte, verileri yeni oluşturulan veritabanına MariaDB sunucusu için Azure veritabanı hedefte geri yükleyin.
```bash
$ mysql -h mydemoserver.mariadb.database.azure.com -u myadmin@mydemoserver -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>PHPMyAdmin kullanarak dışarı aktarma
Dışarı aktarmak için zaten yerel ortamınızda yüklü olabilir ortak aracı phpMyAdmin kullanabilirsiniz. MariaDB veritabanınızı PHPMyAdmin kullanarak dışarı aktarmak için:
1. PhpMyAdmin açın.
2. Veritabanınızı seçin. Sol taraftaki listenin veritabanı adına tıklayın. 
3. Tıklayın **dışarı** bağlantı. Veritabanı dökümünü görüntülemek için yeni bir sayfa görüntülenir.
4. Dışarı aktarma alanı **Tümünü Seç** veritabanınızdaki tablolar seçmek için bağlantı. 
5. SQL Seçenekler bölümünde, uygun seçenekleri'ni tıklatın. 
6. Tıklayın **dosyayı farklı Kaydet** seçeneği ve seçeneğini ve ardından ilgili sıkıştırma **Git** düğmesi. Dosyayı yerel olarak kaydetmek isteyip istemediğinizi soran bir iletişim kutusu görüntülenmelidir.

## <a name="import-using-phpmyadmin"></a>PHPMyAdmin kullanarak içeri aktarma
Veritabanı içeri aktarma, dışarı aktarma için benzerdir. Aşağıdaki eylemleri gerçekleştirin:
1. PhpMyAdmin açın. 
2. PhpMyAdmin Kurulum sayfasında tıklatın **Ekle** MariaDB için Azure veritabanı sunucunuza eklemek için. Oturum açma bilgileri ve bağlantı ayrıntılarını sağlayın.
3. Uygun şekilde adlandırılmış bir veritabanı oluşturun ve ekranın sol tarafında seçin. Var olan veritabanını yeniden yazmak için veritabanının adına tıklayın, tablo adlarının yanındaki onay kutularını seçip **bırak** var olan tabloları silinemedi. 
4. Tıklayın **SQL** burada SQL komutları yazın, ve SQL dosyanızı karşıya yükleme sayfasını göstermek için bağlantı. 
5. Kullanım **Gözat** düğmesi veritabanı dosyası bulunamadı. 
6. Tıklayın **Git** yedekleme dışarı aktarma, SQL komutları yürütme ve veritabanınızı yeniden oluşturma düğmesi.

## <a name="next-steps"></a>Sonraki adımlar
- [MariaDB için Azure veritabanı uygulamaları bağlama](./howto-connection-string.md).
 
<!--
- For more information about migrating databases to Azure Database for MariaDB, see the [Database Migration Guide](https://aka.ms/datamigration).
-->
