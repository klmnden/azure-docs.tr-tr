<properties
   pageTitle="Veri ambarı iş yükü"
   description="SQL Data Warehouse'un esnekliği sayesinde, bir veri ambarı birimi (DWU) kaydırıcı ölçeğini kullanarak işlem gücünü büyütebilir, küçültebilir veya duraklatabilirsiniz. Bu makalede veri ambarı ölçümleri ve bunların DWU'larla olan ilişkisi açıklanmaktadır. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="barbkess;mausher;jrj;sonyama"/>


# Veri ambarı iş yükü
Veri ambarı iş yükü, bir veri ambarında gerçekleşen tüm işlemleri niteler. Veri ambarına verilerin yüklenmesi, veri ambarıyla ilgili analiz ve raporlama yapılması, veri ambarındaki verilerin yönetilmesi ve veri ambarından verilerin dışarı aktarılması sürecinin tamamı veri ambarı iş yükünün kapsamındadır. Bu bileşenlerin derinliği ve genişliği genellikle veri ambarının olgunluk seviyesi ile orantılıdır.


## Veri ambarı ortamında yeni misiniz?
Veri ambarı, bir veya daha fazla veri kaynağından yüklenen verilerin bir koleksiyonudur ve raporlama ile veri analizi gibi iş zekası görevlerinin gerçekleştirilmesinde kullanılır.

Veri ambarları, çok sayıda satırı ve geniş veri aralıklarını tarayan sorgular tarafından belirlenir ve analiz ile raporlama amaçları için görece büyük sonuçlar döndürebilir. Veri ambarları aynı zamanda görece büyük veri yüklerine karşı küçük işlem düzeyindeki eklemeler/güncelleştirmeler/silmelere göre belirlenir.

- Veriler çok sayıda satırı veya geniş veri aralıklarını taraması gereken sorguları iyileştiren bir şekilde depolandığı zaman veri ambarı en iyi şekilde çalışır. Bu tarama türü, veriler satırlar yerine sütunlara göre depolandığı ve arandığı zaman en iyi şekilde çalışır.

>[AZURE.NOTE] Sütun depolamayı kullanan bellek içi columnstore dizini, raporlama ve analiz sorguları için geleneksel ikili ağaçlara göre 10 kata kadar sıkıştırma kazancı ve 100 kata kadar sorgu performansı kazancı sağlar. Veri ambarlarında geniş verilerin depolanması ve taranmasında columnstore dizinlerini standart olarak kabul ediyoruz.

- Bir veri ambarı, çevrimiçi işlem gerçekleştirme (OLTP) için iyileştirilen bir sistemden farklı gereksinimlere sahiptir. OLTP sisteminde birçok ekleme, güncelleştirme ve silme işlemi gerçekleşir. Bu işlemlerde tablodaki belirli satırlar aranır. Tablo aramaları, veriler satır temelinde depolandığında en iyi şekilde çalışır. Veriler ikili ağaç veya btree araması olarak adlandırılan böl ve yönet yaklaşımı kullanılarak sıralanabilir ve hızla aranabilir.


## Veri yükleme
Veri yükleme, veri ambarı iş yükünün büyük bir parçasıdır. İşletmeler genellikle müşteriler ticari işlemler oluştururken gün boyunca değişiklikleri izleyen meşgul OLTP sistemlerine sahiptir. Genellikle gece bakım süresi içinde olmak üzere işlemler düzenli aralıklarla veri ambarına taşınır veya kopyalanır. Veriler veri ambarına ulaştığı zaman, çözümleyiciler veriler üzerinde analiz gerçekleştirebilir ve iş kararları alabilirler.

- Geleneksel olarak, bu işleme Ayıklama, Dönüştürme ve Yükleme yani ETL adı verilir. Verilerin veri ambarındaki diğer verilerle uyumlu olması için genellikle dönüştürülmeleri gerekir. İşletmeler eskiden işlemleri gerçekleştirmek için adanmış ETL sunucuları kullanırdı. Artık son derece hızlı olan yüksek düzeyde paralel işleme sayesinde, önce verileri SQL Data Warehouse'a yükleyebilir ve ardından dönüştürmeleri gerçekleştirebilirsiniz. Bu işlem Ayıklama, Yükleme ve Dönüştürme (ELT) olarak adlandırılır ve veri ambarı iş yükleri için yeni bir standart haline gelmektedir.

> [AZURE.NOTE] SQL Server CTP2 ile artık bir OLTP tablosunda analizleri gerçek zamanlı olarak gerçekleştirebilirsiniz. Bu olanak verilerin depolanması ve analiz edilmesi için bir veri ambarı ihtiyacını ortadan kaldırmaz ancak gerçek zamanlı olarak analiz gerçekleştirilmesi için bir yol sağlar.

### Raporlama ve analiz sorguları
Raporlama ve analiz sorguları genellikle bir dizi ölçüte göre küçük, orta ve büyük olarak sınıflandırılır ancak sınıflandırma çoğunlukla süreye göre yapılır. Çoğu veri ambarında, hızlı çalışan ve uzun süre çalışan sorgulardan oluşan karma bir iş yükü bulunur. Her durumda, bu karışımın ve sıklığının belirlenmesi önemlidir (saatlik, günlük, ay sonu, üç aylık dönem bitişi vb.). Eşzamanlılık ile bağlı olan karma sorgu iş yükünün bir veri ambarı için doğru kapasite planlaması sağlayacağının anlaşılması önemlidir.

- Özellikle veri ambarına kapasite eklemek için uzun bir sağlama süresine ihtiyacınız olduğunda, karma bir sorgu iş yükü için kapasite planlaması karmaşık bir görev olabilir. Depolama ve işlem kapasitesi bağımsız olarak boyutlandırıldığından ve işlem kapasitesini herhangi bir zamanda büyütebileceğiniz ve küçültebileceğiniz için, SQL Data Warehouse kapasite planlaması aciliyetini ortadan kaldırır.

### Veri yönetimi
Verilerin yönetilmesi önemlidir, özellikle de yakın gelecekte diskinizdeki boş alanın tükenebileceğini biliyorsanız. Veri ambarları genellikle verileri anlamlı aralıklara böler, bunlar da bir tabloda bölümler olarak depolanır. Tüm SQL Server tabanlı ürünler, bölümleri tabloya ve tablodan dışarı taşımanıza olanak tanır. Bu bölüm değiştirme olanağı sayesinde, eski verileri daha düşük maliyetli bir depolama alanına taşıyabilir ve en son verileri çevrimiçi depolama alanında tutabilirsiniz.

- Columnstore dizinleri bölümlenmiş tabloları destekler. Columnstore dizinlerinde bölümlenmiş tablolar, veri yönetimi ve arşivleme için kullanılır. Satır temelinde depolanan tablolarda, bölümler sorgu performansında daha büyük bir role sahiptir.  

- PolyBase verilerin yönetiminde önemli bir rol oynar. PolyBase'i kullanarak, eski verileri Hadoop veya Azure blob depolamada arşivleme seçeneğine sahip olursunuz.  Bu, veriler halen çevrimiçi olduğundan çok sayıda seçeneğe sahip olmanızı sağlar.  Verilerin Hadoop'tan alınması daha uzun sürebilir ancak alma süresinin sağladığı kazanç depolama maliyetinden üstün olabilir.

### Verileri dışarı aktarma
Verileri raporlar ve analiz için kullanılabilir duruma getirmenin bir yolu, veri ambarındaki verileri rapor ve analiz çalıştırmak için adanmış sunuculara göndermektir. Bu sunuculara veri reyonları adı verilir. Örneğin, rapor verilerini önceden işleyebilir ve ardından veri ambarından bu verileri tüm dünyadaki birçok sunucuya dışarı aktararak müşterilerin ve çözümleyicilerin geniş çaplı kullanımına sunabilirsiniz.

- Rapor oluşturmak için her gece salt okunur raporlama sunucularını günlük verilerin anlık görüntüleri ile doldurabilirsiniz. Bu sayede müşteriler için daha yüksek bir bant genişliği sağlanırken veri ambarının işlem kaynağı ihtiyaçları azaltılmış olur. Güvenlik açısından, veri reyonları veri ambarına erişimi olan kullanıcıların sayısını azaltmanızı sağlar.
- Analiz için veri ambarında bir çözümleme küpü oluşturup veri ambarında analiz çalıştırabilir veya verileri önceden işleyip ek analizler yapmak için analiz sunucusuna dışarı aktarabilirsiniz.

## Sonraki adımlar
Veri ambarınızı geliştirmeye başlamak için bkz. [geliştirmeye genel bakış][].

## Kitaplar
Karthik Ramachandran, Istvan Szededi ve Richard L. Saltzer tarafından hazırlanan [Big Data Warehousing](https://www.manning.com/books/big-data-warehousing) (Manning Publications) (İngilizce). [Bölüm 1](https://manning-content.s3.amazonaws.com/download/e/3d94acd-9512-46c8-b0b0-8c9c3c6a303b/BDW_MEAP_ch1.pdf)

<!--Image references-->

<!--Article references-->
[geliştirmeye genel bakış]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other web references-->



<!----HONumber=Jun16_HO2-->


