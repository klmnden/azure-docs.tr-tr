---
title: MySQL için Azure veritabanı'nda dışarı ve içeri aktarma
description: Bu makalede, MySQL Workbench gibi araçları kullanarak MySQL için Azure veritabanı'nda veritabanlarını dışarı ve içeri aktarma yaygın yolları açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/01/2018
ms.openlocfilehash: fa72037c8f54271f5651667765c5d5e2e9c03619
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60838116"
---
# <a name="migrate-your-mysql-database-by-using-import-and-export"></a>Dışarı aktarma ve içeri aktarma'yı kullanarak MySQL veritabanınızı geçirme
Bu makalede iki genel yaklaşımları açıklanmaktadır verileri MySQL Workbench kullanarak MySQL için Azure veritabanı dışarı aktarma ve içeri aktarma. 

## <a name="before-you-begin"></a>Başlamadan önce
Bu nasıl yapılır kılavuzunda adımlamak için ihtiyacınız vardır:
- İzleyerek MySQL sunucusu için Azure veritabanı [Azure portalını kullanarak MySQL için Azure veritabanı oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md).
- MySQL Workbench [indirilen](https://dev.mysql.com/downloads/workbench/), içeri ve dışarı aktarma için veya başka bir MySQL araç.

## <a name="use-common-tools"></a>Ortak Araçlar kullanın
Uzaktan bağlanın ve içeri aktarma veya MySQL için Azure veritabanı'na veri dışarı MySQL Workbench, kurbağa veya Navicat gibi yaygın araçları kullanın. 

MySQL için Azure veritabanı'na bağlanmak üzere istemci makinenizde Internet bağlantısının bulunduğu gibi araçlar kullanın. Bir SSL şifreli bağlantı açıklandığı gibi en iyi güvenlik yöntemlerini kullanın [SSL bağlantısını yapılandırma MySQL için Azure veritabanı'nda](concepts-ssl-connection-security.md).

İçeri aktarma işleminiz taşıyın ve MySQL için Azure veritabanı'na geçiş sırasında herhangi bir özel bulutun konumuna dosyaları dışarı aktarma gerekmez. 

## <a name="create-a-database-on-the-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı bir veritabanı oluşturma
Verileri geçirmek istediğiniz MySQL sunucusu için Azure veritabanı, boş bir veritabanı oluşturun. Veritabanını oluşturmak için MySQL Workbench, kurbağa veya Navicat gibi bir araç kullanın. Veritabanı Dökümü alınan verileri içeren veritabanı ile aynı ada sahip olabilir veya farklı bir adla bir veritabanı oluşturabilirsiniz.

Bağlanmak için bağlantı bilgilerini bulun **genel bakış** , MySQL için Azure veritabanı.

![Azure portalında bağlantı bilgilerini Bul](./media/concepts-migrate-import-export/1_server-overview-name-login.png)

MySQL Workbench için bağlantı bilgilerini ekleyin.

![MySQL Workbench bağlantı dizesi](./media/concepts-migrate-import-export/2_setup-new-connection.png)

## <a name="determine-when-to-use-import-and-export-techniques-instead-of-a-dump-and-restore"></a>İçeri aktarma ve dışarı aktarın teknikler yerine bir döküm ve geri yükleme ne zaman belirleme
İçeri aktarma ve veritabanları aşağıdaki senaryolarda Azure MySQL veritabanına dışarı aktarmak için MySQL araçlarını kullanın. Diğer senaryolarda kullanımından yararlanabilir [döküm ve geri yükleme](concepts-migrate-dump-restore.md) yerine yaklaşımını. 

- Mevcut bir MySQL veritabanındaki verileri Azure MySQL veritabanına aktarmak için birkaç tablo seçerek gerektiğinde, alma ve verme tekniği en iyisidir.  Bunu yaptığınızda, gereksiz tüm tabloları geçiş zamandan ve kaynaklardan tasarruf atlayabilirsiniz. Örneğin, `--include-tables` veya `--exclude-tables` anahtarı ile [mysqlpump](https://dev.mysql.com/doc/refman/5.7/en/mysqlpump.html#option_mysqlpump_include-tables) ve `--tables` anahtarı ile [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#option_mysqldump_tables).
- Tablolar dışında veritabanı nesneleri taşıma, açıkça bu nesneleri oluşturun. Kısıtlamalar (birincil anahtar, yabancı anahtar, dizinleri), görünümleri, İşlevler, yordamlar, tetikleyiciler ve geçirmek istediğiniz diğer veritabanı nesnelerini içerir.
- Bir MySQL veritabanı dışındaki dış veri kaynaklarından veri geçiş yapıyorsanız, düz dosyaları oluşturmak ve bunları kullanarak içeri [mysqlimport](https://dev.mysql.com/doc/refman/5.7/en/mysqlimport.html).

MySQL için Azure veritabanı'na veri yüklüyorsunuz, veritabanındaki tüm tabloları Innodb depolama altyapısı kullandığınızdan emin olun. MySQL için azure veritabanı yalnızca Innodb depolama altyapısı desteklediğinden, bunu alternatif depolama altyapılarını desteklemiyor. Tablolarınızı alternatif depolama altyapılarını gerektiriyorsa, MySQL için Azure veritabanı geçiş işleminden önce Innodb altyapısı biçimi kullanmak için bunları dönüştürmeniz emin olun. 

MyISAM altyapısını kullanan bir WordPress veya web uygulamanız varsa, örneğin, ilk tabloları Innodb tablolarına veri geçirerek dönüştürün. Sonra MySQL için Azure veritabanı'na geri yükleyin. Yan tümcesini kullanın `ENGINE=INNODB` bir tablo oluşturmak için altyapı ayarlama ve sonra geçiş işleminden önce uyumlu tabloya veri aktarımı için. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```

## <a name="performance-recommendations-for-import-and-export"></a>İçeri ve dışarı aktarma için performans önerisi
-   Verileri yüklemeden önce Kümelenmiş dizinler ve birincil anahtar oluşturun. Birincil anahtar sırada verileri yükleyin. 
-   Gecikme kadar veri sonra ikincil dizinlerin oluşturulmasını yüklenir. Yüklemeden sonra tüm ikincil dizinleri oluşturun. 
-   Yüklemeden önce yabancı anahtar kısıtlamalarını devre dışı bırakın. Yabancı anahtar denetimleri devre dışı bırakma, önemli ölçüde performans kazanımı sağlar. Kısıtlamaları etkinleştirin ve tutarlılığını sağlamak için yüklemeden sonra verileri doğrulayın.
-   Paralel veri yükleyin. Kaynak sınırına neden, ve Azure portalında mevcut olan ölçümler kullanarak kaynakları izlemek çok fazla paralellik kaçının. 
-   Uygun olduğunda bölümlenmiş tabloları kullanın.

## <a name="import-and-export-by-using-mysql-workbench"></a>MySQL Workbench kullanarak dışarı ve içeri aktarma
MySQL Workbench uygulamasında veri içeri ve dışarı aktarmak için iki yolu vardır. Her, farklı bir amaca hizmet eder. 

### <a name="table-data-export-and-import-wizards-from-the-object-browsers-context-menu"></a>Sihirbaz nesne tarayıcının bağlam menüsünden içeri ve dışarı tablo verileri
![Nesne tarayıcı bağlam menüsünde MySQL Workbench sihirbazları](./media/concepts-migrate-import-export/p1.png)

Sihirbazlar için tablo verilerini alma desteği ve CSV ve JSON dosyaları kullanılarak verme işlemleri. Bunlar, ayırıcı, sütun seçimini ve kodlama seçimi gibi birkaç yapılandırma seçeneklerini kapsar. Yerel veya uzaktan bağlı MySQL sunucuları karşı her sihirbazın gerçekleştirebilirsiniz. Tablo, sütun ve tür eşlemesi alma eylemi içerir. 

Tabloya sağ tıklayarak, bu sihirbazlar nesne tarayıcının bağlam menüsünden erişebilirsiniz. Ya da seçin **tablo verileri dışarı Aktarma Sihirbazı** veya **tablo verileri İçeri Aktarma Sihirbazı'nı**. 

#### <a name="table-data-export-wizard"></a>Tablo verileri dışarı Aktarma Sihirbazı
Aşağıdaki örnek tabloya bir CSV dosyasına dışarı aktarır: 
1. Dışa aktarılacak veritabanı tablosu sağ tıklayın. 
2. Seçin **tablo verileri dışarı Aktarma Sihirbazı'nı**. Verilmesi, satır uzaklığı (varsa) ve (varsa) sayısı için sütunları seçin. 
3. Üzerinde **seçin verileri dışarı aktarma** sayfasında **sonraki**. Dosya yolu seçin, CSV veya JSON dosya türü. Ayrıca, satır ayırıcı, dizeler ve gelen alan ayırıcısı kapsayan yöntemi seçin. 
4. Üzerinde **Select çıkış dosyasının konumu** sayfasında **sonraki**. 
5. Üzerinde **verileri dışarı aktar** sayfasında **sonraki**.

#### <a name="table-data-import-wizard"></a>Tablo verileri İçeri Aktarma Sihirbazı
Aşağıdaki örnek tabloya bir CSV dosyasından içeri aktarır:
1. İçeri aktarılacak bir veritabanı tablosuna sağ tıklayın. 
2. Göz atın ve içe aktarılması ve ardından için CSV dosyası seçin **sonraki**. 
3. Hedef Tablo (yeni veya var olan) seçin ve seçin veya temizleyin **Truncate tablo içeri aktarmadan önce** onay kutusu. **İleri**’ye tıklayın.
4. Kodlama Seç ve sütunların içeri aktarılması ve ardından **sonraki**. 
5. Üzerinde **verileri içeri aktarma** sayfasında **sonraki**. Sihirbaz, buna göre verileri içeri aktarır.

### <a name="sql-data-export-and-import-wizards-from-the-navigator-pane"></a>SQL veri sihirbazları için Gezgin bölmesindeki alma ve verme
MySQL Workbench'ten oluşturulan veya mysqldump komuttan oluşturulan SQL içeri veya dışarı aktarmak için bir sihirbaz kullanın. Bu sihirbaz erişim **Gezgin** bölmesinde seçerek veya **sunucu** ana menüden. Ardından **verileri dışarı aktarma** veya **veri içeri aktarma**. 

#### <a name="data-export"></a>Verileri dışarı aktarma
![Gezinti bölmesini kullanarak MySQL Workbench verilerini dışarı aktarma](./media/concepts-migrate-import-export/p2.png)

Kullanabileceğiniz **verileri dışarı aktarma** MySQL verilerinizi dışarı aktarmak için sekmesinde. 
1. Dışarı aktarma, isteğe bağlı olarak her şemasından belirli şema nesneleri/tabloları seçebilir ve dışarı aktarma oluşturmak istediğiniz her bir şema seçin. Yapılandırma seçenekleri içeren bir proje klasörü veya kendi içinde SQL dosyası dışarı aktarma, saklı yordamları ve olayları dökümü veya tablo verilerini atlayın. 
 
   Alternatif olarak, **bir sonuç kümesi dışarı** CSV, JSON, HTML ve XML gibi başka bir biçimde SQL Düzenleyicisi kümesindeki belirli bir sonuç dışarı aktarmak için. 
3. Dışarı aktarmak için veritabanı nesneleri seçin ve ilgili seçenekleri yapılandırın.
4. Tıklayın **Yenile** geçerli nesneleri yüklenemedi.
5. İsteğe bağlı olarak, açık **Gelişmiş Seçenekler** dışarı aktarma işlemi iyileştirmek için sekmesinde. Örneğin, tablo kilitleri, kullanım Değiştir INSERT deyimleri ve teklif tanımlayıcıları vurgulamasını belirtir karakterlerle yerine ekleyin.
6. Tıklayın **Başlat dışarı** dışarı aktarma işlemini başlatmak için.


#### <a name="data-import"></a>Veri alma
![Yönetim Gezgini kullanarak MySQL Workbench veri alma](./media/concepts-migrate-import-export/p3.png)

Kullanabileceğiniz **veri alma** içeri aktarın veya geri yüklemek için sekmesinde aktarılan verileri veri dışarı aktarma işlemi veya mysqldump komutu. 
1. Şemayı içeri aktarabilir veya kendi içinde SQL dosyası ve proje klasörü seçin, **yeni** yeni bir şema tanımlamak için. 
2. Tıklayın **Başlat alma** içeri aktarma işlemini başlatmak için.

## <a name="next-steps"></a>Sonraki adımlar
- Başka bir geçiş yaklaşımı okuma [MySQL veritabanı ile dökümü ve MySQL için Azure veritabanı'nda geri geçirme](concepts-migrate-dump-restore.md).
- Geçiş hakkında daha fazla bilgi için bkz. veritabanları, MySQL için Azure veritabanı [veritabanı Geçiş Kılavuzu](https://aka.ms/datamigration). 
