---
title: İçeri ve dışarı aktarma Azure veritabanı için MySQL
description: Bu makalede Azure veritabanı veritabanlarında MySQL için MySQL çalışma ekranı gibi araçları kullanarak vermek ve almak için yaygın yolları açıklanmaktadır.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 6a4d5fd2bb649b2a0b0d23f4f752442f854e5098
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264910"
---
# <a name="migrate-your-mysql-database-by-using-import-and-export"></a>İçeri aktarma kullanarak MySQL veritabanınızı geçirin ve dışarı aktarma
Bu makalede, iki ortak yaklaşımlar açıklanmaktadır içeri aktarma ve verileri MySQL çalışma ekranı kullanarak MySQL sunucusu için bir Azure veritabanı dışarı aktarma. 

## <a name="before-you-begin"></a>Başlamadan önce
Nasıl yapılır bu kılavuzu adım için gerekir:
- MySQL sunucusu için izleyerek, Azure veritabanı [Azure portalını kullanarak MySQL sunucusu için bir Azure veritabanı oluşturma](quickstart-create-mysql-server-database-using-azure-portal.md).
- MySQL çalışma ekranı [indirilen](https://dev.mysql.com/downloads/workbench/), veya içeri ve dışarı aktarmak için başka bir MySQL araç.

## <a name="use-common-tools"></a>Ortak araçlarını kullanma
Uzaktan bağlanmak ve almak veya MySQL için Azure veritabanına veri vermek için MySQL çalışma ekranı, kurbağa veya Navicat gibi ortak araçlarını kullanın. 

MySQL için Azure veritabanına bağlanmak için bir Internet bağlantısı olan istemci makinenizde gibi araçlar kullanın. Bir SSL şifreli bağlantı açıklandığı gibi en iyi güvenlik uygulamalarını kullanın [yapılandırma SSL bağlantısı MySQL için Azure veritabanında](concepts-ssl-connection-security.md).

Alma taşıyın ve herhangi bir özel bulut konuma Azure veritabanı için MySQL geçirilirken dışarı aktarma dosyaları gerekmez. 

## <a name="create-a-database-on-the-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure veritabanı bir veritabanı oluşturun
Verileri geçirmek istediğiniz MySQL sunucusu için Azure veritabanı üzerinde boş bir veritabanı oluşturun. Veritabanını oluşturmak için MySQL çalışma ekranı, kurbağa veya Navicat gibi bir araç kullanın. Veritabanı Dökümü alınan verileri içeren veritabanı ile aynı ada sahip olabilir veya farklı bir adla bir veritabanı oluşturabilirsiniz.

Bağlantı için bağlantı bilgilerini bulun **genel bakış** MySQL için Azure veritabanınızın.

![Azure Portalı'nda bağlantı bilgilerini Bul](./media/concepts-migrate-import-export/1_server-overview-name-login.png)

Bağlantı bilgilerini MySQL çalışma ekranına ekleyin.

![MySQL çalışma ekranı bağlantı dizesi](./media/concepts-migrate-import-export/2_setup-new-connection.png)

## <a name="determine-when-to-use-import-and-export-techniques-instead-of-a-dump-and-restore"></a>İçe aktarma işlemi kullanın ve teknikleri bir döküm yerine vermek ve geri yüklemek ne zaman belirleme
MySQL araçlarını almak ve veritabanları aşağıdaki senaryolarda Azure MySQL veritabanına vermek için kullanın. Diğer senaryolarda kullanımından yararlanabilir [dökümü ve geri yükleme](concepts-migrate-dump-restore.md) yerine yaklaşımını. 

- Varolan bir MySQL veritabanından Azure MySQL veritabanına aktarmak için birkaç tablo seçmeli olarak seçmeniz gerektiğinde, içe aktarma işlemi kullanın ve teknik dışarı aktarmak en iyisidir.  Bunu yaparak, zaman ve kaynak kaydetmek için geçiş gereksiz tüm tablolardan atlayabilirsiniz. Örneğin, `--include-tables` veya `--exclude-tables` anahtarı ile [mysqlpump](https://dev.mysql.com/doc/refman/5.7/en/mysqlpump.html#option_mysqlpump_include-tables) ve `--tables` anahtarı ile [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#option_mysqldump_tables).
- Açıkça tablolar dışındaki veritabanı nesnelerini taşırken, bu nesneler oluşturun. Kısıtlamalar (birincil anahtar, yabancı anahtar, dizinler), görünümleri, işlevleri, yordamlar, tetikleyiciler ve geçirmek istediğiniz herhangi bir veritabanı nesnesini içerir.
- Bir MySQL veritabanı dışında dış veri kaynaklarından veri geçişini, düz dosyalarını oluşturmak ve bunları kullanarak içeri [mysqlimport](https://dev.mysql.com/doc/refman/5.7/en/mysqlimport.html).

Azure veritabanına veri MySQL için yüklemekte olduğunuz zaman veritabanındaki tüm tabloların InnoDB depolama altyapısı kullandığınızdan emin olun. Azure veritabanı için MySQL yalnızca InnoDB depolama altyapısı destekler, bu nedenle alternatif depolama altyapılarını desteklemiyor. Alternatif depolama altyapılarını tablolarınızı ihtiyacınız varsa bunları Azure veritabanı geçiş işleminden önce InnoDB altyapısı biçimi için MySQL kullanmak için dönüştürmek emin olun. 

MyISAM altyapısını kullanan bir WordPress veya web uygulaması varsa, örneğin, ilk tabloları InnoDB tablolara veri geçirerek dönüştürün. Ardından Azure veritabanı için MySQL geri yükleyin. Yan tümcesini kullanın `ENGINE=INNODB` tablo oluşturma için altyapısı ayarlayın ve ardından geçişten önce uyumlu tabloya veri aktarmak için. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```

## <a name="performance-recommendations-for-import-and-export"></a>İçeri ve dışarı aktarma için performans önerileri
-   Kümelenmiş dizinler ve birincil anahtarlar veri yüklemeden önce oluşturun. Birincil anahtar sırayla veri yükleyin. 
-   Gecikme veri kadar sonra ikincil dizinlerin oluşturulmasını yüklenir. Tüm İkincil dizinler yüklemeden sonra oluşturun. 
-   Yüklemeden önce yabancı anahtar kısıtlamaları devre dışı bırakın. Yabancı anahtar denetimleri devre dışı bırakma önemli ölçüde performans artışı sağlar. Kısıtlamaları etkinleştirin ve tutarlılığını sağlamak için yükleme işleminden sonra verileri doğrulayın.
-   Paralel veri yükleyin. Kaynak sınırına neden, ve kaynakları Azure portalında kullanılabilir ölçümleri kullanarak izlemek çok fazla paralellik kaçının. 
-   Uygun olduğunda bölümlenmiş tabloları kullanın.

## <a name="import-and-export-by-using-mysql-workbench"></a>İçeri ve dışarı aktarma MySQL çalışma ekranı kullanarak
MySQL çalışma ekranı verileri içeri ve dışarı aktarmak iki yolu vardır. Her farklı bir amaca hizmet eder. 

### <a name="table-data-export-and-import-wizards-from-the-object-browsers-context-menu"></a>Tablo verisi nesne tarayıcının bağlam menüsünden sihirbazları alma ve verme
![Nesne tarayıcının bağlam menüsünde MySQL çalışma ekranı sihirbazları](./media/concepts-migrate-import-export/p1.png)

Tablo verisi için sihirbazları alma desteği ve CSV ve JSON dosyaları kullanılarak verme işlemleri. Ayırıcılar, sütun seçimi ve kodlama seçimi gibi çeşitli yapılandırma seçenekleri içerirler. Yerel veya uzaktan bağlı MySQL sunucuları karşı her bir sihirbazın gerçekleştirebilirsiniz. Alma eylemi tablo, sütun ve tür eşlemesi içerir. 

Bir tablo sağ tıklayarak, bu sihirbazlar nesne tarayıcının bağlam menüsünden erişebilirsiniz. Ya da seçin **tablo verileri dışarı Aktarma Sihirbazı** veya **tablo veri içeri aktarma Sihirbazı'nı**. 

#### <a name="table-data-export-wizard"></a>Tablo verileri dışarı Aktarma Sihirbazı
Aşağıdaki örnek tablo bir CSV dosyasına dışarı aktarır: 
1. Dışa aktarılacak veritabanı tablosunun sağ tıklayın. 
2. Seçin **tablo verileri dışarı Aktarma Sihirbazı'nı**. Verilmesi, satır uzaklığı (varsa) ve (varsa) Say sütunları seçin. 
3. Üzerinde **seçin verileri dışarı aktarma** sayfasında, **sonraki**. Dosya yolunu seçin, CSV veya JSON dosya türü. Ayrıca satır ayırıcı dizeler ve alan ayırıcı kapsayan yöntemi seçin. 
4. Üzerinde **Select çıkış dosyasının konumu** sayfasında, **sonraki**. 
5. Üzerinde **dışarı veri** sayfasında, **sonraki**.

#### <a name="table-data-import-wizard"></a>Tablo verileri İçeri Aktarma Sihirbazı
Aşağıdaki örnek tablo bir CSV dosyasından içeri aktarır:
1. İçeri aktarılacak veritabanı tablosunun sağ tıklayın. 
2. Göz atın ve alınması ve ardından CSV dosyası seçmeniz **sonraki**. 
3. (Yeni veya var olan), hedef tablo seçin ve seçin veya temizleyin **içe aktarma işleminden önce Truncate table** onay kutusu. **İleri**’ye tıklayın.
4. Select kodlama ve içe aktarılması ve ardından sütunları **sonraki**. 
5. Üzerinde **veri içeri aktarma** sayfasında, **sonraki**. Sihirbaz, buna göre verileri içeri aktarır.

### <a name="sql-data-export-and-import-wizards-from-the-navigator-pane"></a>SQL veri Gezgini bölmesinden sihirbazları alma ve verme
MySQL çalışma ekranından oluşturulan veya mysqldump komuttan üretilen SQL içeri veya dışarı aktarmak için bir Sihirbazı'nı kullanın. Bu sihirbaz erişim **Gezgini** bölmesinde veya seçerek **Server** ana menüden. Ardından **verilerini dışa aktarma** veya **veri alma**. 

#### <a name="data-export"></a>Verileri dışarı aktarma
![Gezgin bölmesini kullanarak MySQL çalışma ekranı verileri dışarı aktarma](./media/concepts-migrate-import-export/p2.png)

Kullanabileceğiniz **verilerini dışa aktarma** sekmesini MySQL verilerinizi vermek için. 
1. Dışarı aktarma, isteğe bağlı olarak her şemadan belirli şema nesneleri/tabloları seçin ve dışa aktarma oluşturmak istediğiniz her şema seçin. Yapılandırma seçenekleri proje klasörünü veya kendi içinde bulunan SQL dosyasına dışa aktarma dahil, saklı yordamları ve olayları dökümü veya tablo verileri atlayabilirsiniz. 
 
   Alternatif olarak, kullanın **bir sonuç kümesi verme** belirli bir sonuç CSV, JSON, HTML ve XML gibi başka bir biçime SQL düzenleyicisinde kümesi vermek için. 
3. Dışarı aktarmak için veritabanı nesneleri seçin ve ilgili seçenekleri yapılandırın.
4. Tıklatın **yenileme** geçerli nesnelerini yüklenemiyor.
5. İsteğe bağlı olarak, açık **Gelişmiş Seçenekler** dışa aktarma işlemi iyileştirmek için sekmesi. Örneğin, tablo kilitleri, kullanım Değiştir INSERT deyimleri ve teklif tanımlayıcıları backtick karakterlerle yerine ekleyin.
6. Tıklatın **Başlat verme** dışarı aktarma işlemini başlatmak için.


#### <a name="data-import"></a>Veri alma
![Yönetim Gezgini kullanarak MySQL çalışma ekranı veri alma](./media/concepts-migrate-import-export/p3.png)

Kullanabileceğiniz **veri alma** içeri aktarın veya geri yüklemek için sekmesinde verilen verileri veri dışarı aktarma işlemi ya da mysqldump komutu. 
1. Proje klasörünü veya kendi içinde bulunan SQL dosyasını seçin, içeri aktarabilir veya seçmek için şema seçin **yeni** yeni bir şema tanımlamak için. 
2. Tıklatın **Başlat alma** içeri aktarma işlemini başlatmak için.

## <a name="next-steps"></a>Sonraki adımlar
- Başka bir geçiş yaklaşım okuma [dökümü ve geri yükleme, Azure veritabanı'nda MySQL için MySQL veritabanı kullanarak geçiş](concepts-migrate-dump-restore.md).
- Geçiş hakkında daha fazla bilgi için MySQL için Azure veritabanı veritabanlarını görmek [veritabanı Geçiş Kılavuzu](http://aka.ms/datamigration). 
