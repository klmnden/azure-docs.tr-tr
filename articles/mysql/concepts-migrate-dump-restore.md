---
title: Döküm kullanarak MySQL veritabanınızı geçirme ve MySQL için Azure veritabanı'nda geri yükleme
description: Bu makalede PHPMyAdmin mysqldump ve MySQL Workbench gibi araçları kullanarak MySQL için Azure veritabanı veritabanlarını geri iki yaygın yolları açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/02/2018
ms.openlocfilehash: e79c83ecb17c4dcd11f7ccbecded59e7d1d13dfd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60525808"
---
# <a name="migrate-your-mysql-database-to-azure-database-for-mysql-using-dump-and-restore"></a>MySQL veritabanınızı, döküm ve geri yükleme kullanarak MySQL için Azure veritabanı'na geçirme
Bu makalede, MySQL için Azure veritabanı veritabanlarını geri iki yaygın yolları açıklanmaktadır.
- Döküm ve geri yükleme komut satırından (mysqldump kullanarak) 
- Döküm ve PHPMyAdmin kullanarak geri yükleme 

## <a name="before-you-begin"></a>Başlamadan önce
Bu nasıl yapılır kılavuzunda adımlamak için sahip olmanız gerekir:
- [MySQL server - Azure portalı için Azure veritabanı oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md)
- [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) komut satırı yardımcı programı bir makinede yüklü.
- MySQL Workbench [MySQL Workbench'i indir](https://dev.mysql.com/downloads/workbench/), kurbağa, Navicat veya diğer üçüncü taraf MySQL aracından dökümü ve komutlarını geri yükleme.

## <a name="use-common-tools"></a>Ortak Araçlar kullanın
Uzaktan bağlanmak ve verileri, MySQL için Azure veritabanı'na geri yüklemek için genel yardımcı programları ve MySQL Workbench, mysqldump, kurbağa veya Navicat gibi araçları kullanın. MySQL için Azure veritabanı'na bağlanmak üzere istemci makinenizde internet bağlantısının bulunduğu gibi araçlar kullanın. Bir SSL şifreli bağlantı için en iyi güvenlik uygulamalarını kullanın, ayrıca bkz: [SSL bağlantısını yapılandırma MySQL için Azure veritabanı'nda](concepts-ssl-connection-security.md). MySQL için Azure veritabanı'na geçirirken döküm dosyaları herhangi bir özel bulutun konumuna taşımak gerekmez. 

## <a name="common-uses-for-dump-and-restore"></a>Döküm ve geri yükleme için yaygın kullanımları
Birçok yaygın senaryoları Azure MySQL veritabanı mysqldump ve dump ve load veritabanlarına mysqlpump gibi MySQL yardımcı programını kullanabilirsiniz. Diğer senaryolarda kullanabilirsiniz [içeri ve dışarı aktarma](concepts-migrate-import-export.md) yerine yaklaşımını.

- Tüm veritabanını geçirirken veritabanı kullanmak dökümünü yapar. Bu öneri, büyük miktarda MySQL veri taşırken ya da Canlı siteler veya uygulamalar için hizmet kesintilerini en aza indirmek istediğiniz zaman barındırır. 
-  MySQL için Azure veritabanı'na veri yükleme zaman veritabanındaki tüm tabloları Innodb depolama altyapısı kullandığınızdan emin olun. MySQL için Azure veritabanı, yalnızca Innodb depolama altyapısını destekler ve alternatif depolama altyapılarını desteklemez. Tablolarınızı diğer depolama altyapıları ile yapılandırıldıysa, MySQL için Azure veritabanı geçiş işleminden önce Innodb altyapısı biçimine dönüştürün.
   WordPress veya MyISAM tabloları kullanarak Webapp'i varsa, örneğin, önce bu tablolarda MySQL için Azure veritabanı'na geri yüklemeden önce Innodb biçime geçiş yaparak dönüştürün. Yan tümcesini kullanın `ENGINE=InnoDB` yeni bir tablo oluştururken kullanılan altyapısının ayarlamak için ardından veri geri yüklemeden önce uyumlu tabloya aktarın. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- Uyumluluk sorunları önlemek için MySQL aynı sürümü kaynak ve hedef sistemlerde veritabanları çıkarırken kullanılan emin olun. Mevcut MySQL sunucunuzu sürümü 5.7 ise, örneğin, daha sonra Azure veritabanı'na için MySQL 5.7 sürümünü çalıştırmak üzere yapılandırılmış geçirmeniz gerekir. `mysql_upgrade` Komutu bir MySQL sunucusu için Azure veritabanı'nda çalışmaz ve desteklenmez. MySQL sürümleri arasında yükseltme gerekiyorsa, ilk döküm veya alt sürüm veritabanınızı MySQL daha yüksek bir sürümü kendi ortamınızda verin. Ardından çalıştırın `mysql_upgrade`, MySQL için Azure veritabanı'na geçişi denemeden önce.

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

## <a name="create-a-backup-file-from-the-command-line-using-mysqldump"></a>Bir yedekleme dosyası, komut satırından oluşturmak mysqldump kullanma
Yerel şirket içi sunucusunda veya bir sanal makinede mevcut bir MySQL veritabanını yedeklemek için aşağıdaki komutu çalıştırın: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Sağlamak için Parametreler şunlardır:
- [uname] Veritabanı kullanıcı adı 
- [başarılı] Veritabanınızın parolası (-p ile parola arasında boşluk olmadığından olmasına dikkat edin) 
- [dbname] Veritabanınızın adı 
- [backupfile.sql] veritabanı yedekleme için dosya adı 
- [--iyileştirilmiş] Mysqldump seçeneği 

Örneğin, MySQL sunucunuz 'testuser' kullanıcı adı ve parola için bir dosya testdb_backup.sql ' testdb' adlı bir veritabanı yedeklemek için aşağıdaki komutu kullanın. Komut yedekler `testdb` adlı bir dosya veritabanına `testdb_backup.sql`, veritabanını yeniden oluşturmak için gereken tüm SQL deyimlerini içerir. 

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

## <a name="create-a-database-on-the-target-azure-database-for-mysql-server"></a>' % S'hedef Azure veritabanını MySQL sunucusu için bir veritabanı oluşturma
' % S'hedef Azure veritabanını MySQL sunucusuna, verileri geçirmek istediğiniz boş bir veritabanı oluşturun. Veritabanını oluşturmak için MySQL Workbench, kurbağa veya Navicat gibi bir araç kullanın. Veritabanı olan veritabanı Dökümü alınan veriler içeriyor veya farklı bir adla bir veritabanı oluşturabileceğiniz gibi aynı ada sahip olabilir.

Bağlanmak için bağlantı bilgilerini bulun **genel bakış** , MySQL için Azure veritabanı.

![Azure portalında bağlantı bilgilerini Bul](./media/concepts-migrate-dump-restore/1_server-overview-name-login.png)

Bağlantı bilgilerini, MySQL Workbench uygulamasına ekleyin.

![MySQL Workbench bağlantı dizesi](./media/concepts-migrate-dump-restore/2_setup-new-connection.png)


## <a name="restore-your-mysql-database-using-command-line-or-mysql-workbench"></a>Komut satırı kullanarak MySQL veritabanı veya MySQL Workbench geri yükleme
Hedef veritabanı oluşturulduktan sonra yeni oluşturulan veritabanına döküm dosyasından özel içine verileri geri yüklemek için mysql komut veya MySQL Workbench kullanabilirsiniz.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
Bu örnekte, verileri ' % s'hedef Azure veritabanını MySQL sunucusu için yeni oluşturulan veritabanına geri yükleyin.
```bash
$ mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>PHPMyAdmin kullanarak dışarı aktarma
Dışarı aktarmak için zaten yerel ortamınızda yüklü olabilir ortak aracı phpMyAdmin kullanabilirsiniz. MySQL veritabanınızı PHPMyAdmin kullanarak dışarı aktarmak için:
1. PhpMyAdmin açın.
2. Veritabanınızı seçin. Sol taraftaki listenin veritabanı adına tıklayın. 
3. Tıklayın **dışarı** bağlantı. Veritabanı dökümünü görüntülemek için yeni bir sayfa görüntülenir.
4. Dışarı aktarma alanı **Tümünü Seç** veritabanınızdaki tablolar seçmek için bağlantı. 
5. SQL Seçenekler bölümünde, uygun seçenekleri'ni tıklatın. 
6. Tıklayın **dosyayı farklı Kaydet** seçeneği ve seçeneğini ve ardından ilgili sıkıştırma **Git** düğmesi. Dosyayı yerel olarak kaydetmek isteyip istemediğinizi soran bir iletişim kutusu görüntülenmelidir.

## <a name="import-using-phpmyadmin"></a>PHPMyAdmin kullanarak içeri aktarma
Veritabanı içeri aktarma, dışarı aktarma için benzerdir. Aşağıdaki eylemleri gerçekleştirin:
1. PhpMyAdmin açın. 
2. PhpMyAdmin Kurulum sayfasında tıklatın **Ekle** Azure veritabanınızı MySQL sunucusuna eklemek için. Oturum açma bilgileri ve bağlantı ayrıntılarını sağlayın.
3. Uygun şekilde adlandırılmış bir veritabanı oluşturun ve ekranın sol tarafında seçin. Var olan veritabanını yeniden yazmak için veritabanının adına tıklayın, tablo adlarının yanındaki onay kutularını seçip **bırak** var olan tabloları silinemedi. 
4. Tıklayın **SQL** burada SQL komutları yazın, ve SQL dosyanızı karşıya yükleme sayfasını göstermek için bağlantı. 
5. Kullanım **Gözat** düğmesi veritabanı dosyası bulunamadı. 
6. Tıklayın **Git** yedekleme dışarı aktarma, SQL komutları yürütme ve veritabanınızı yeniden oluşturma düğmesi.

## <a name="next-steps"></a>Sonraki adımlar
- [MySQL için Azure veritabanı uygulamaları bağlama](./howto-connection-string.md).
- Geçiş hakkında daha fazla bilgi için bkz. veritabanları, MySQL için Azure veritabanı [veritabanı Geçiş Kılavuzu](https://aka.ms/datamigration).
