---
title: Azure SQL Data Warehouse Nedir? | Microsoft Belgeleri
description: "İlişkisel ve ilişkisel olmayan petabaytlarca veriyi işleyebilen, kurumsal sınıf bir veritabanıdır. Saniyeler içinde büyütmenizi, küçültmenizi ve duraklatmanızı sağlayan, sektörün ilk bulut veri ambarıdır."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: bjhubbard
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: jrj;mausher;kevin;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 6241eb0e7ea091dffcb0ae770f8d89f24a19eb67
ms.openlocfilehash: ff2f688d42924edb1596cb2db474a58748f2b44c


---
# <a name="what-is-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse Nedir?
Azure SQL Veri Ambarı, hem ilişkisel hem de ilişkisel olmayan çok geniş hacimlerdeki verileri işleyebilen, bulut tabanlı bir genişletme veritabanıdır. Yüksek düzeyde paralel işleme (MPP) mimarimizin üzerine kurulu olan SQL Data Warehouse, kuruluşunuzun iş yükünü işleyebilir.

SQL Data Warehouse:

* SQL Server ilişkisel veritabanıyla Azure bulut ölçeğini genişletme işlevlerini birleştirir. İşlemleri saniyeler veya dakikalar içinde yükseltebilir, azaltabilir, duraklatabilir veya sürdürebilirsiniz. İhtiyacınız olduğunda CPU'nun ölçeğini genişleterek ve yoğun olmayan zamanlarda kullanımı azaltarak maliyet tasarrufu yapabilirsiniz.
* Azure platformundan yararlanır. Dağıtılması kolaydır, bakımı sorunsuzdur ve otomatik yedeklemeler sayesinde hataya tamamen dayanıklıdır.
* SQL Server ekosistemini tamamlar. Alışık olduğunuz SQL Server Transact-SQL (T-SQL) ve araçlarla geliştirme yapabilirsiniz.

Bu makalede SQL Veri Ambarı’nın temel özellikleri açıklanmaktadır.

## <a name="massively-parallel-processing-architecture"></a>Yüksek düzeyde paralel işleme mimarisi
SQL Veri Ambarı, yüksek düzeyde paralel işleme (MPP) ile dağıtılmış bir veritabanı sistemidir. Birden fazla düğümde verileri ve işleme özelliğini bölerek, SQL Veri Ambarı tek bir sistemin sağlayabileceğinden çok büyük ölçeklenebilirlik sunabilir.  Arka planda, SQL Veri Ambarı verilerinizi hiçbir şey paylaşılmayan çok sayıda depolama ve işleme birimleri arasında yayar. Veriler Premium yerel olarak yedekli depolama alanına depolanır ve sorgu yürütmesi için işlem düğümlerine bağlanır. Bu mimari ile SQL Veri Ambarı, çalışan yükler ve karmaşık sorgular karşısında bir "böl ve yönet" yaklaşımını benimser. İstekler Denetim düğümü tarafından alınır, iyileştirilir ve ardından çalışmalarını paralel olarak yürütmeleri için İşlem düğümlerine geçirilir.

MPP mimarisi ile Azure depolama işlevlerini birleştiren SQL Veri Ambarı şunları yapabilir:

* İşlemden bağımsız olarak depolamayı büyütür veya küçültür.
* Verileri taşımadan işlemi büyütür veya küçültür.
* İşlem kapasitesini duraklatırken verilerin bozulmadan kalmasını sağlar.
* İşlem kapasitesini anında sürdürür.

Aşağıdaki diyagramda mimari daha ayrıntılı olarak gösterilmiştir.

![SQL Data Warehouse Mimarisi][1]

**Denetim düğümü:** Denetim düğümü sorguları yönetir ve en iyi duruma getirir. Tüm uygulamalarla ve bağlantılarla etkileşim kuran ön uçtur. SQL Data Warehouse'da Kontrol düğümü SQL Database ile güçlendirilmiştir ve buna yapılan bağlantının görünümü ve hissi aynıdır. Geri planda ise Kontrol düğümü dağıtılmış verilerinizde paralel sorgular çalıştırmak için gerekli olan tüm veri hareketlerini ve hesaplamaları koordine eder. SQL Veri Ambarı’na bir T-SQL sorgusu gönderdiğiniz zaman, Kontrol düğümü bu sorguyu her paralel İşlem düğümünde çalışacak ayrı sorgulara dönüştürür.

**İşlem düğümleri:** İşlem düğümleri, SQL Veri Ambarı’nın arkasındaki güç olarak hizmet verir. Bunlar verilerinizi depolayan ve sorgunuzu işleyen SQL Veritabanlarıdır. Veri eklediğiniz zaman SQL Veri Ambarı satırları İşlem düğümlerinize dağıtır. İşlem düğümleri verilerinizde paralel sorguları çalıştıran çalışanlardır. İşlemeden sonra, sonuçları yeniden Kontrol düğümüne geçirirler. Kontrol düğümü sorguyu tamamlamak için sonuçları toplar ve son sonucu döndürür.

**Depolama:** Verileriniz Azure Blob depolama alanına depolanır. İşlem düğümleri verilerinizle etkileşim kurduklarında, doğrudan blob depolamaya ve blob depolamadan yazma ve okuma işlemlerini gerçekleştirirler. Azure depolama saydam ve büyük ölçüde genişlediğinden, SQL Veri Ambarı da aynısını yapabilir. İşlem ve depolama bağımsız olduğundan, SQL Data Warehouse işlemi ölçeklendirmeden ayrı olarak depolamayı otomatik şekilde ölçeklendirebilir ve tam tersini yapabilir. Azure Blob depolama aynı zamanda hatalara tamamen dayanıklıdır ve yedekleme ve geri yükleme işlemini kolaylaştırır.

**Veri Taşıma Hizmeti:** Veri Taşıma Hizmeti (DMS), düğümler arasında verileri taşır. DMS, İşlem düğümlerinin birleşimler ve toplamalar için ihtiyaç duydukları verilere erişmelerini sağlar. DMS bir Azure hizmeti değildir. Tüm düğümlerde SQL Database'in yanında çalışan bir Windows hizmetidir. DMS arka planda çalıştığı için doğrudan DMS ile etkileşim kurmazsınız. Bununla birlikte sorgu planlarına baktığınız zaman planların bazı DMS işlemlerini içerdiğini fark edersiniz, bunun nedeni her sorguyu paralel olarak çalıştırmak için veri taşımanın gerekli olmasıdır.

## <a name="optimized-for-data-warehouse-workloads"></a>Data warehouse iş yükleri için en iyi duruma getirilmiştir
MPP yaklaşımı veri ambarına özgü çeşitli performans iyileştirmeleriyle desteklenir; bu iyileştirmeler arasında aşağıdakiler bulunur:

* Tüm verileri kapsayan karmaşık istatistikler kümesi ve bir dağıtılmış sorgu iyileştiricisi. Veri boyutu ve dağıtımı bilgilerini kullanan hizmet, belirli dağıtılmış sorgu işlemlerinin maliyetini değerlendirerek sorguları iyileştirebilir.
* Sorguyu gerçekleştirmek için gerekli olan bilgi işlem kaynakları arasında verilerin etkin şekilde taşınmasını sağlayan, veri taşıma işlemine tümleştirilmiş gelişmiş algoritmalar ve teknikler. Bu veri taşıma işlemleri yerleşiktir ve Veri Taşıma Hizmeti'nde yapılan tüm iyileştirmeler otomatik olarak gerçekleşir.
* Varsayılan olarak kümelenmiş **columnstore** dizinleri. SQL Veri Ambarı, sütun tabanlı depolamayı kullanarak geleneksel satır yönelimli depolamaya göre ortalama 5 kat sıkıştırma kazancı ve 10 kat veya daha fazla sorgu performansı kazancı sağlar. Çok sayıda satırı taraması gereken analitik sorguları, columnstore dizinlerinde çok iyi sonuçlar verir.

## <a name="predictable-and-scalable-performance"></a>Tahmin edilebilir ve ölçeklenebilir performans
SQL Veri Ambarı, depolama ve işlemi birbirinden ayırarak her birini bağımsız olarak ölçeklendirmeye imkan tanır. SQL Veri Ambarı çok kısa sürede başka işlem kaynakları eklemek üzere hızlı ve kolay bir biçimde ölçeklendirilebilir. Azure Blob depolamanın kullanılması bunun tamamlanmasını sağlar. Blob’lar kararlı ve çoğaltılmış depolama sağlamanın yanı sıra, düşük maliyetle zahmetsiz genişlemenin altyapısını da sunar. Bu bulut ölçekli depolama ve Azure işlem birleşimini kullanan SQL Veri Ambarı, sorgu performansı ve depolama alanı için ödeme yapmanıza olanak tanır. İşlem miktarını değiştirmek Azure portalında bir kaydırıcıyı sola veya sağa hareket ettirmek kadar basittir ya da aynı zamanda T-SQL ve PowerShell kullanılarak zamanlanabilir.

Depolamadan bağımsız olarak işlem miktarını tamamen kontrol etme becerisinin yanı sıra, SQL Veri Ambarı veri ambarınızı tamamen duraklatmanıza da olanak tanır, yanı gerekli olmadığında işlem ücreti ödemeniz gerekmez. Depolamanız yerinde tutulurken, tüm işlemler Azure'un ana havuzuna bırakılarak paradan tasarruf etmeniz sağlanır. Gerekli olduğu zaman, işlemi sürdürmeniz ve verilerinizi ve işleminizi iş yükü için kullanılabilir duruma getirmeniz yeterlidir.

## <a name="data-warehouse-units"></a>Data Warehouse Birimleri
Kaynakların SQL Veri Ambarı’na ayrılması Data Warehouse Birimlerinde (DWU) ölçülür. DWU’lar CPU, bellek, IOPS gibi SQL Veri Ambarı’na ayrılmış temel alınan kaynakların bir ölçümüdür. DWU sayısı artırıldığında kaynaklar ve performans da artar. DWU’lar özellikle aşağıdakilere yardımcı olur:

* Temel alınan donanım veya yazılım hakkında endişelenmeden, veri ambarınızı kolayca ölçeklendirebilirsiniz.
* Veri ambarınızın boyutunu değiştirmeden önce bir DWU düzeyindeki performans artışını tahmin edebilirsiniz.
* Örneğinizdeki temel alınan donanım veya yazılım, iş yükü performansınızı etkilemeden değişebilir veya taşınabilir.
* Microsoft, hizmetin temel alınan mimarisinde iş yükünüzün performansını etkilemeden ayarlamalar yapabilir.
* Microsoft, sistemi eşit şekilde etkileyecek ve ölçeklenebilir bir şekilde SQL Veri Ambarı’nda performansı hızla geliştirebilir.

Data Warehouse Birimleri, veri ambarı iş yükü performansı ile son derece bağıntılı olan üç hassas ölçüm sağlar. Aşağıdaki temel iş yükü ölçümlerinin veri ambarınıza yönelik seçtiğiniz DWU'larla doğrusal olarak ölçeklendirilmesi hedeflenmektedir.

**Tarama/Toplama:** Bu iş yükü ölçümü, çok sayıda satırı tarayan ve ardından karmaşık bir toplama işlemi gerçekleştiren standart bir veri ambarı sorgusu alır. Bu, G/Ç ve CPU yoğunluklu bir işlemdir.

**Yük:** Bu ölçüm, hizmete veri alma becerisini ölçer. Yükler, PolyBase'in Azure Blob depolama alanındaki temsili bir veri kümesi yüklemesiyle tamamlanır. Bu ölçüm, hizmetin Ağ ve CPU yönlerine stres testi uygulamak için tasarlanmıştır.

**Create Table As Select (CTAS):** CTAS bir tabloyu kopyalama becerisini ölçer. Bu da verilerin depolama alanından okunmasını, gerecin tüm düğümlerine dağıtılmasını ve yeniden depolama alanına yazılmasını içerir. CPU, G/Ç ve ağ yoğunluklu bir işlemdir.

## <a name="pause-and-scale-on-demand"></a>İsteğe bağlı duraklatma ve ölçeklendirme
Daha hızlı sonuçlar almanız gerektiğinde, daha yüksek performans elde etmek için DWU'larınızı artırın. Daha düşük bir işlem gücüne gereksinim duyduğunuzda, DWU'larınızı azaltarak yalnızca ihtiyacınız olduğu ölçüde ödeme yapın. Aşağıdaki senaryolarda DWU’ları değiştirmeyi düşünebilirsiniz:

* Sorgu çalıştırmanız gerekli olmadığında, belki de akşamları veya hafta sonları, sorgularınızı susturun. Ardından gerekli olmadığında DWU için ödeme yapmaktan kaçınmak üzere işlem kaynaklarınızı duraklatın.
* Sisteminiz düşük talebe sahip olduğunda DWU’yu küçük bir boyuta indirmeyi deneyin. Verilere erişmeye devam edebilirsiniz, ancak önemli bir maliyet tasarrufu elde edersiniz.
* Çok miktarda verinin yüklendiği veya dönüştürüldüğü bir işlemi gerçekleştirirken, verilerinizin daha hızlı kullanılabilir olmasını sağlamak için ölçeklemeyi artırmak isteyebilirsiniz.

İdeal DWU değerinizin ne olduğunu anlamak için verilerinizi yükledikten sonra ölçeği artırmayı veya azaltmayı ve birkaç sorgu çalıştırmayı deneyin. Ölçeklendirme hızla gerçekleştiği için bir saat veya daha kısa bir sürede çeşitli performans düzeylerini deneyebilirsiniz.  SQL Veri Ambarı büyük miktarlarda verileri işlemek için tasarlanmıştır ve özellikle sunduğumuz büyük ölçeklerde ölçeklendirmeye yönelik gerçek kapasitesini görmek için yaklaşık 1 TB veya daha fazla bir büyük veri kümesi kullanmak istersiniz.

## <a name="built-on-sql-server"></a>Yerleşik SQL Server
SQL Veri Ambarı, SQL Server ilişkisel veritabanı altyapısını temel alır ve bir kurumsal veri ambarından beklediğiniz birçok özelliği kapsar. T-SQL'i tanıyorsanız bildiklerinizi SQL Veri Ambarı’na aktarmanız son derece kolaydır. İleri düzeyde veya kullanmaya yeni başlıyor olabilirsiniz, belgelerin genelinde sağlanan örnekler başlangıçta size yardımcı olur. Genel olarak, SQL Data Warehouse'un dil öğelerini şu şekilde oluşturduğumuzu kabul edebilirsiniz:

* SQL Veri Ambarı birçok işlem için T-SQL söz dizimini kullanır. Ayrıca depolanan yordamlar, kullanıcı tanımlı işlevler, tablo bölümleme, dizinler ve harmanlamalar gibi çok sayıda geleneksel SQL yapısını destekler.
* SQL Veri Ambarı kümelenmiş **columnstore** dizinleri, PolyBase tümleştirme ve veri denetimi (tehdit değerlendirmesi ile tamamlanan) dahil olmak üzere daha yeni SQL Server özelliklerini de içerir.
* Veri ambarı iş yükleri için daha az yaygın olan ya da SQL Server için yeni olan bazı T-SQL dil öğeleri şu anda kullanılamayabilir. Daha fazla bilgi için bkz. [Geçiş belgeleri][Migration documentation].

SQL Server, SQL Data Warehouse, SQL Database ve Analiz Platformu Sistemi arasındaki ortak özellikler ve Transact-SQL sayesinde, veri ihtiyaçlarınıza uygun olan bir çözümü geliştirebilirsiniz. Performans, güvenlik ve ölçeklendirme gereksinimlerine göre verilerinizi saklamayı ve ardından verileri gereken şekilde farklı sistemler arasında aktarmayı seçebilirsiniz.

## <a name="data-protection"></a>Veri koruma
SQL Veri Ambarı tüm verileri Azure Premium yerel olarak yedekli depolama alanında depolar. Yerelleştirilmiş hata olasılıklarına karşın saydam veri koruma olanağı sağlamak amacıyla yerel veri merkezinde verilerin birden çok zaman uyumlu kopyası saklanır. Ayrıca, SQL Veri Ambarı Azure Depolama Anlık Görüntüleri kullanarak etkin (duraklatılmamış) veritabanlarınızı otomatik olarak yedekler. Yedekleme ve geri yükleme işleminin nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [Yedekleme ve geri yüklemeye genel bakış][Backup and restore overview].

## <a name="integrated-with-microsoft-tools"></a>Microsoft araçları ile tümleşiktir
SQL Veri Ambarı ayrıca SQL Server kullanıcıların tanıyor olabileceği çok sayıda araçla tümleştirilir. Bunlar:

**Geleneksel SQL Server araçları:** SQL Veri Ambarı SQL Server Analysis Services, Integration Services ve Reporting Services ile tam olarak tümleşiktir.

**Bulut tabanlı araçlar:** SQL Veri Ambarı; Data Factory, Akış Analizi , Machine Learning ve Power BI dahil olmak üzere Azure'daki bir dizi yeni araçla birlikte kullanılabilir. Daha kapsamlı bir liste için bkz. [Tümleşik araçlara genel bakış][Integrated tools overview].

**Üçüncü taraf araçları:** Çok sayıda üçüncü taraf aracı sağlayıcısına ait araç, SQL Veri Ambarı ile sertifikalı tümleştirmeye sahiptir. Tam bir liste için bkz. [SQL Veri Ambarı çözüm ortakları][SQL Data Warehouse solution partners].

## <a name="hybrid-data-sources-scenarios"></a>Karma veri kaynakları senaryoları
Kullanıcılar, SQL Veri Ambarı’nı PolyBase ile birlikte kullanarak verileri ekosistemleri içinde taşıma konusunda benzersiz bir beceriye sahip olur ve ilişkisel olmayan ve şirket içi veri kaynakları ile gelişmiş karma senaryolar kurma becerisini ortaya çıkarırlar.

Polybase farklı kaynaklardaki verilerinizi alışık olduğunuz T-SQL komutlarıyla kullanmanızı sağlar. Polybase, Azure Blob depolamada tutulan ilişkisel olmayan verileri olağan bir tablo gibi sorgulamanıza olanak tanır. Polybase'i kullanarak ilişkisel olmayan verileri sorgulayabilir veya ilişkisel olmayan verileri SQL Veri Ambarı’na içeri aktarabilirsiniz.

* PolyBase ilişkisel olmayan verilere erişmek için dış tabloları kullanır. Tablo tanımları SQL Veri Ambarı’nda depolanır ve normal ilişkisel verilere erişirken kullandığınız araçlarla ve SQL yoluyla tablo tanımlarına erişebilirsiniz.
* Polybase, tümleştirme sırasında bağımsız şekilde hareket eder. Desteklediği tüm kaynaklar için aynı özellikleri ve işlevleri kullanıma sunar. Polybase tarafından okunan veriler, sınırlandırılmış dosyalar veya ORC dosyaları dahil olmak üzere çeşitli biçimlerde olabilir.
* PolyBase aynı zamanda bir HDInsight kümesi için depolama alanı olarak kullanılan blob depolama alanına erişmek için kullanılabilir. Bu özellik, ilişkisel ve ilişkisel olmayan araçlarla aynı verilere erişmenizi sağlar.

## <a name="sla"></a>SLA
SQL Veri Ambarı, Microsoft Online Services SLA’nın parçası olarak ürün düzeyinde hizmet düzeyi sözleşmesi (SLA) sağlar. Daha fazla bilgi için [SQL Veri Ambarı için SLA][SLA for SQL Data Warehouse] bölümünü ziyaret edin. Diğer tüm ürünlerle ilgili SLA bilgileri için [Hizmet Düzeyi Sözleşmeleri] Azure sayfasını ziyaret edebilir veya [Toplu Lisanslama][Volume Licensing] sayfasından bunları indirebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
SQL Veri Ambarı hakkında biraz bilgi sahibi olduğunuza göre hızlıca [SQL Veri Ambarı oluşturma][create a SQL Data Warehouse] ve [örnek verileri yükleme][load sample data] hakkında bilgi edinin. Azure'da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü][Azure glossary] yararlı bulabilirsiniz. Alternatif olarak, aşağıdaki diğer SQL Data Warehouse Kaynakları'na göz atın.  

* [Müşteri başarı hikayeleri]
* [Bloglar]
* [Özellik istekleri]
* [Videolar]
* [Müşteri Danışma Ekibi blogları]
* [Destek bileti oluşturma]
* [MSDN forumu]
* [Stack Overflow forumu]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Destek bileti oluşturma]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Müşteri başarı hikayeleri]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Müşteri Danışma Ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN forumu]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow forumu]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Hizmet Düzeyi Sözleşmeleri]: https://azure.microsoft.com/en-us/support/legal/sla/



<!--HONumber=Jan17_HO3-->


