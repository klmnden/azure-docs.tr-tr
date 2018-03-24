---
title: Döküm kullanarak MySQL veritabanınızı geçirin ve Azure veritabanı'nda MySQL için geri yükleme
description: Bu makalede yedeklemek ve mysqldump, MySQL çalışma ekranı ve PHPMyAdmin gibi araçları kullanarak MySQL için Azure veritabanınızdaki veritabanlarını geri yüklemek için iki genel yolu açıklanmaktadır.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 03/20/2018
ms.openlocfilehash: ef35ee881923c69d41b79fd6cb8464c695c614f9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="migrate-your-mysql-database-to-azure-database-for-mysql-using-dump-and-restore"></a>MySQL veritabanınız için MySQL döküm ve geri yükleme kullanarak Azure veritabanına geçirme
Bu makalede, yedeklemek ve MySQL için Azure veritabanınızdaki veritabanlarını geri yüklemek için iki genel yolu açıklanmaktadır.
- Döküm ve geri yükleme komut satırından (mysqldump kullanarak) 
- Döküm ve PHPMyAdmin kullanarak geri yükleme 

## <a name="before-you-begin"></a>Başlamadan önce
Nasıl yapılır bu kılavuzu adım için sahip olmanız gerekir:
- [MySQL server - Azure portalında Azure veritabanı oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md)
- [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) komut satırı yardımcı programı bir makinede yüklü.
- MySQL çalışma ekranı [çalışma ekranı MySQL karşıdan](https://dev.mysql.com/downloads/workbench/), kurbağa, Navicat veya diğer üçüncü taraf MySQL aracı dökümü ve geri yükleme komutları.

## <a name="use-common-tools"></a>Ortak araçlarını kullanma
Uzaktan bağlanma ve verileri için MySQL Azure veritabanına geri yüklemek için ortak yardımcı programları ve MySQL çalışma ekranı, mysqldump, kurbağa veya Navicat gibi araçları kullanın. MySQL için Azure veritabanına bağlanmak için bir Internet bağlantısı olan istemci makinenizde gibi araçlar kullanın. Bir SSL şifreli bağlantı için en iyi güvenlik uygulamalarını kullanın, ayrıca bkz. [yapılandırma SSL bağlantısı MySQL için Azure veritabanında](concepts-ssl-connection-security.md). Azure veritabanı için MySQL geçirilirken döküm dosyaları herhangi bir özel bulut konuma taşımak gerekmez. 

## <a name="common-uses-for-dump-and-restore"></a>Ortak kullanımlar döküm ve geri yükleme için
Bir Azure MySQL veritabanına MySQL yardımcı programları mysqldump ve döküm ve yük veritabanlarına mysqlpump gibi birkaç ortak senaryoda kullanabilir. Diğer senaryolarda kullanabilirsiniz [içeri ve dışarı aktarma](concepts-migrate-import-export.md) yerine yaklaşımını.

- Tüm veritabanını geçirilirken veritabanını kullan dökümünü yapar. Bu öneri, büyük miktarda MySQL veri geçerken ya da Canlı siteler veya uygulamalar için hizmet kesintisini en aza indirmek istediğiniz zaman barındırır. 
-  Veritabanındaki tüm tabloların InnoDB depolama altyapısı için MySQL Azure veritabanına verileri yüklenirken kullandığınızdan emin olun. Azure veritabanı için MySQL yalnızca InnoDB depolama altyapısını destekler ve bu nedenle alternatif depolama altyapılarını desteklemez. Tablolarınızı diğer depolama altyapılarını ile yapılandırılmışsa, MySQL için Azure veritabanı geçişten önce InnoDB altyapısı biçime dönüştürmek.
   WordPress veya MyISAM tabloları kullanarak WebApp varsa, örneğin, önce bu tablolar için MySQL Azure veritabanına geri yüklemeden önce InnoDB biçimine geçirerek dönüştürün. Yan tümcesini kullanın `ENGINE=InnoDB` yeni bir tablo oluştururken kullanılan altyapısının ayarlamak için ardından veri geri yüklemeyi önce uyumlu tabloya aktarın. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- Tüm uyumluluk sorunlarını önlemek için MySQL aynı sürümü kaynak ve hedef sistemlerde veritabanları çıkarırken kullanılan emin olun. Mevcut MySQL sunucunuzu sürüm 5.7 ise, örneğin, daha sonra Azure veritabanı için MySQL sürüm 5.7 çalışacak şekilde yapılandırılmış geçirmeniz gerekir. `mysql_upgrade` Komutu MySQL sunucusu için bir Azure veritabanındaki çalışmaz ve desteklenmez. MySQL sürümleri arasında yükseltme gerekiyorsa, ilk dökümü veya alt sürüm veritabanınızı MySQL daha yüksek bir sürüm kendi ortamınızda verin. Ardından çalıştırın `mysql_upgrade`, MySQL için bir Azure veritabanına geçiş işlemini denemeden önce.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar
Performansı iyileştirmek için bu noktalar duyuru büyük veritabanları çıkarırken alın:
-   Kullanım `exclude-triggers` veritabanları çıkarırken mysqldump seçeneği. Tetikleyiciler veri geri yükleme sırasında tetikleme tetikleyici komutları önlemek için döküm dosyalarını dışında bırakın. 
-   Kullanım `single-transaction` YİNELENEBİLİR OKUMAK için işlem yalıtım modunu ayarlamak için seçenek ve bir başlangıç işlem SQL deyimi veri dökme önce sunucuya gönderir. Tek bir işlem içinde çok sayıda tabloya dökme geri yükleme sırasında kullanılması bazı ek depolama alanı neden olur. `single-transaction` Seçeneği ve `lock-tables` seçeneği karşılıklı olarak birbirini dışlar kilit tabloları örtük olarak kabul edilebilmesi için işlemleri beklemedeki neden olduğundan. Büyük tabloları dökümü için birleştirme `single-transaction` seçeneğini `quick` seçeneği. 
-   Kullanım `extended-insert` birkaç değer listesi içeren birden çok satır sözdizimi. Bu, daha küçük bir dökümü dosyasına sonuçları ve dosya yeniden yüklendiğinde eklemeleri hızlandırır.
-  Kullanım `order-by-primary` veritabanları, böylece birincil anahtar sırayla verileri Script çıkarırken mysqldump seçeneği.
-   Kullanım `disable-keys` veri çıkarırken mysqldump seçeneğinde yük önce yabancı anahtar kısıtlamaları devre dışı bırakmak için. Yabancı anahtar denetimleri devre dışı bırakma performans artışı sağlar. Kısıtlamaları etkinleştirin ve tutarlılığını sağlamak için yükleme işleminden sonra verileri doğrulayın.
-   Uygun olduğunda bölümlenmiş tabloları kullanın.
-   Paralel veri yükleyin. Kaynak sınırına neden, ve Azure portalında kullanılabilir ölçümleri kullanarak kaynakları izlemek çok fazla paralellik kaçının. 
-   Kullanım `defer-table-indexes` veritabanları, böylece dizin oluşturma tabloları veriler yüklendikten sonra gerçekleşir çıkarırken mysqlpump seçeneği.

## <a name="create-a-backup-file-from-the-command-line-using-mysqldump"></a>Bir yedekleme dosyası komut satırından oluşturma mysqldump kullanma
Yerel şirket içi sunucuda veya bir sanal makinede mevcut bir MySQL veritabanını yedeklemek için aşağıdaki komutu çalıştırın: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Sağlamak için Parametreler şunlardır:
- [uname] Veritabanı kullanıcı adı 
- [geçiş] Veritabanınız için parola (-p parola arasındaki alan yok unutmayın) 
- [dbname] Veritabanı adı 
- [backupfile.sql] veritabanı yedeklemesi için dosya adı 
- [--opt] Mysqldump seçeneği 

Örneğin, MySQL sunucunuzu 'testuser' kullanıcı adı ve dosya testdb_backup.sql için bir parola ile ' testdb' adlı bir veritabanı yedeklemek için aşağıdaki komutu kullanın. Komut yedekler `testdb` adlı bir dosya veritabanına `testdb_backup.sql`, veritabanını yeniden oluşturmak için gerekli olan SQL deyimlerini içerir. 

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
Veritabanınızı yedeklemek için belirli tabloları seçmek için boşluklarla ayırarak tablo adları listesi. Örneğin, yalnızca tablo1 ve tablo2 tablolarının tabloları 'testdb' yedeklemek için bu örnek izleyin: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```

Aynı anda birden fazla veritabanını yedeklemek için kullanın veritabanı geçin ve boşluklarla ayırarak veritabanı adları listesi. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```
Sunucudaki tüm veritabanları için aynı anda yedeklemek için kullanmanız tüm veritabanları seçeneği.
```bash
$ mysqldump -u root -p --all-databases > alldb_backup.sql 
```

## <a name="create-a-database-on-the-target-azure-database-for-mysql-server"></a>MySQL sunucusu için hedef Azure veritabanı bir veritabanı oluşturma
MySQL server verileri geçirmek istediğiniz hedef Azure veritabanı üzerinde boş bir veritabanı oluşturun. Veritabanını oluşturmak için MySQL çalışma ekranı, kurbağa veya Navicat gibi bir araç kullanın. Veritabanı olduğundan veritabanı Dökümü alınan veriler içeriyor veya farklı bir adla bir veritabanı oluşturabilirsiniz gibi aynı ada sahip olabilir.

Bağlantı için bağlantı bilgilerini bulun **genel bakış** MySQL için Azure veritabanınızın.

![Azure Portalı'nda bağlantı bilgilerini Bul](./media/concepts-migrate-dump-restore/1_server-overview-name-login.png)

MySQL çalışma ekranı içine bağlantı bilgilerini ekleyin.

![MySQL çalışma ekranı bağlantı dizesi](./media/concepts-migrate-dump-restore/2_setup-new-connection.png)


## <a name="restore-your-mysql-database-using-command-line-or-mysql-workbench"></a>Komut satırı kullanarak MySQL veritabanı veya MySQL çalışma ekranı geri yükleme
Hedef veritabanı oluşturduktan sonra yeni veritabanı dökümü dosyasından oluşturulan özel içine verileri geri yüklemek için mysql komut veya MySQL çalışma ekranı kullanabilirsiniz.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
Bu örnekte, hedefte Azure veritabanı MySQL sunucusu için yeni oluşturulan veritabanına verileri geri yükleyin.
```bash
$ mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p testdb < testdb_backup.sql
```

## <a name="export-using-phpmyadmin"></a>PHPMyAdmin kullanarak dışa aktarın
Dışarı aktarma için yerel olarak ortamınızda yüklediyseniz ortak aracı phpMyAdmin kullanabilirsiniz. MySQL veritabanınız PHPMyAdmin kullanarak dışarı aktarmak için:
- PhpMyAdmin açın.
- Veritabanınızı seçin. Sol taraftaki listesinde veritabanı adını tıklatın. 
- Tıklatın **verme** bağlantı. Veritabanı dökümünü görüntülemek için yeni bir sayfa görüntülenir.
- Dışa aktarma alanı **Tümünü Seç** bağlantı veritabanınızda tabloları seçin. 
- SQL Seçenekleri alanında, uygun seçenekleri'ni tıklatın. 
- ' I tıklatın **dosyası olarak kaydetmeniz** seçeneği ve karşılık gelen sıkıştırma seçeneğini ve ardından **Git** düğmesi. Dosyayı yerel olarak kaydetmek isteyip istemediğinizi soran bir iletişim kutusu görünür.

## <a name="import-using-phpmyadmin"></a>PHPMyAdmin kullanarak içe aktarma
Veritabanınızı içeri aktarma, dışarı aktarma için benzer. Aşağıdaki eylemleri gerçekleştirebilirsiniz:
- PhpMyAdmin açın. 
- PhpMyAdmin Kurulum sayfasında **Ekle** MySQL sunucusu için Azure veritabanı eklemek için. Bağlantı ayrıntıları ve oturum açma bilgileri sağlayın.
- Uygun adlandırılmış bir veritabanı oluşturun ve ekranın sol tarafta seçin. Var olan veritabanını yeniden yazma için veritabanı adını tıklatın, tablo adlarının yanındaki onay kutularını seçin ve seçin **bırakma** varolan tablolardan silmek için. 
- Tıklatın **SQL** bağlantı buradan SQL komutları yazın, veya SQL dosyanızı karşıya yükleme sayfasını göster. 
- Kullanım **Gözat** veritabanı dosyasını bulmak için düğmesini. 
- Tıklatın **Git** düğmesine yedekleme verme, SQL komutları yürütün ve veritabanınızı yeniden oluşturun.

## <a name="next-steps"></a>Sonraki adımlar
[MySQL için uygulamaları Azure veritabanına bağlan](./howto-connection-string.md)
