<properties
   pageTitle="Azure SQL Data Warehouse Nedir? | Microsoft Azure"
   description="İlişkisel ve ilişkisel olmayan petabaytlarca veriyi işleyebilen, kurumsal sınıf bir veritabanıdır. Saniyeler içinde büyütmenizi, küçültmenizi ve duraklatmanızı sağlayan, sektörün ilk bulut veri ambarıdır."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/01/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# Azure SQL Data Warehouse Nedir?

Azure SQL Data Warehouse, hem ilişkisel hem de ilişkisel olmayan çok geniş hacimlerdeki verileri işleyebilen, bulut tabanlı bir genişletme veritabanıdır. Yüksek düzeyde paralel işleme (MPP) mimarimizin üzerine kurulu olan SQL Data Warehouse, kuruluşunuzun iş yükünü işleyebilir. 

SQL Data Warehouse:

- Kanıtlanmış SQL Server ilişkisel veritabanımızla Azure bulut ölçeğini genişletme işlevlerimizi birleştirir. İşlemleri saniyeler içinde yükseltebilir, azaltabilir, duraklatabilir veya sürdürebilirsiniz.  Bu sayede ihtiyacınız olduğunda CPU'nun ölçeğini genişleterek ve yoğun olmayan zamanlarda kullanımı azaltarak maliyet tasarrufu yapabilirsiniz.
- Azure platformumuzdan yararlanır. Dağıtılması kolaydır ve bakımı sorunsuz şekilde yapılır. Aynı zamanda otomatik yedeklemeler sayesinde hatalara tamamen dayanıklıdır. 
- SQL Server ekosistemini tamamlar.  Alışık olduğunuz SQL Server T-SQL ve araçlarla geliştirme yapabilirsiniz.

SQL Data Warehouse'un temel özellikleri hakkında daha fazla bilgi almak için okumaya devam edin.

## İyileştirilmiş

### Yüksek Düzeyde Paralel İşleme (MPP) mimarisi

SQL Data Warehouse, dünyanın en büyük şirket için veri ambarlarından bazılarını çalıştırmak için tasarlanmış olan Microsoft'un yüksek düzeyde paralel işleme (MPP) mimarisini kullanır.

Şu anda, MPP mimarimiz verilerinizi 60 adet hiçbir şey paylaşılmayan depolama ve işleme birimine yaymaktadır. Veriler Premium Storage içinde Azure Storage Bloblarında depolanır ve sorgu yürütmesi için İşlem düğümlerine bağlanır. Bu mimariyle birlikte, karmaşık T-SQL sorgularını çalıştırmak için böl ve yönet yaklaşımını uygulayabiliriz. İşleme sırasında, Kontrol düğümü sorguyu ayrıştırır ve ardından her İşlem düğümü kendine ait paralel veri bölümünü "yönetir". 

MPP mimarimizi ve Azure depolama işlevlerimizi birleştiren SQL Data Warehouse şunları yapabilir:

- İşlemden bağımsız olarak depolamayı büyütür veya küçültür.
- Verileri taşımadan işlemi büyütür veya küçültür. 
- İşlem kapasitesini duraklatırken verilerin bozulmadan kalmasını sağlar.
- İşlem kapasitesini anında sürdürür.

Mimari aşağıda ayrıntılı olarak açıklanmıştır. 

![SQL Data Warehouse Mimarisi][1]


- **Denetim düğümü:** Denetim düğümü sistemi "denetler". Tüm uygulamalarla ve bağlantılarla etkileşim kuran ön uçtur. SQL Data Warehouse'da Kontrol düğümü SQL Database ile güçlendirilmiştir ve buna yapılan bağlantının görünümü ve hissi aynıdır. Geri planda ise Kontrol düğümü dağıtılmış verilerinizde paralel sorgular çalıştırmak için gerekli olan tüm veri hareketlerini ve hesaplamaları koordine eder. SQL Data Warehouse'a bir TSQL sorgusu gönderdiğiniz zaman, Kontrol düğümü bu sorguyu her paralel İşlem düğümünde çalışacak ayrı sorgulara dönüştürür.

- **İşlem Düğümleri:** İşlem düğümleri, SQL Data Warehouse'un arkasındaki güç olarak hizmet verir. Bunlar, sorgu adımlarınızı işleyen ve verilerinizi yöneten SQL Database'lerdir. Veri eklediğiniz zaman, SQL Data Warehouse İşlem düğümlerinizi kullanarak satırları dağıtır. İşlem düğümleri aynı zamanda verilerinizde paralel sorguları çalıştıran çalışanlardır. İşlemeden sonra, sonuçları yeniden Kontrol düğümüne geçirirler. Kontrol düğümü sorguyu tamamlamak için sonuçları toplar ve son sonucu döndürür.


- **Storage:** Verileriniz Azure Storage Bloblarında depolanır. İşlem düğümleri verilerinizle etkileşim kurduklarında, doğrudan blob depolamaya ve blob depolamadan yazma ve okuma işlemlerini gerçekleştirirler. Azure depolama saydam ve sınırsız şekilde genişlediğinden, SQL Data Warehouse da aynısını yapabilir. İşlem ve depolama bağımsız olduğundan, SQL Data Warehouse işlemi ölçeklendirmeden ayrı olarak depolamayı otomatik şekilde ölçeklendirebilir ve tam tersini yapabilir.  Azure Storage aynı zamanda hatalara tamamen dayanıklıdır ve yedekleme ve geri yükleme işlemini kolaylaştırır.
   

- **Veri Taşıma Hizmeti:** Veri Taşıma Hizmeti (DMS), düğümler arasında verileri taşımak için kullandığımız teknolojimizdir. DMS, İşlem düğümlerinin birleşimler ve toplamalar için ihtiyaç duydukları verilere erişmelerini sağlar. DMS bir Azure hizmeti değildir. Tüm düğümlerde SQL Database'in yanında çalışan bir Windows hizmetidir. DMS arka planda çalıştığı için doğrudan DMS ile etkileşim kurmazsınız. Bununla birlikte sorgu planlarına baktığınız zaman planların bazı DMS işlemlerini içerdiğini fark edersiniz, bunun nedeni her paralel sorguda veri taşımanın bir biçimde veya formda çalışmasının gerekmesidir.


### İyileştirilmiş sorgu performansı

Böl ve yönet stratejisinin yanı sıra, MPP yaklaşımı veri ambarına özgü çeşitli performans iyileştirmeleriyle desteklenir; bu iyileştirmeler arasında aşağıdakiler bulunur:

- Tüm verileri kapsayan karmaşık istatistikler kümesi ve bir dağıtılmış sorgu iyileştiricisi. Veri boyutu ve dağıtımı bilgilerini kullanan hizmet, belirli dağıtılmış sorgu işlemlerinin maliyetini değerlendirerek sorguları iyileştirebilir.

- Sorguyu gerçekleştirmek için gerekli olan bilgi işlem kaynakları arasında verilerin etkin şekilde taşınmasını sağlayan, veri taşıma işlemine tümleştirilmiş gelişmiş algoritmalar ve teknikler. Bu veri taşıma işlemleri yerleşiktir ve Veri Taşıma Hizmeti'nde yapılan tüm iyileştirmeler otomatik olarak gerçekleşir.

- Varsayılan olarak kümelenmiş columnstore dizinleri. SQL Data Warehouse, sütun tabanlı depolamayı kullanarak geleneksel satır yönelimli depolamaya göre 5 kata kadar sıkıştırma kazancı ve 10 kata kadar sorgu performansı kazancı sağlar. Çok sayıda satırı taraması gereken analitik sorguları, columnstore dizinlerinde çok iyi sonuçlar verir. 

## Ölçeklenebilir

SQL Data Warehouse mimarisinin sunduğu ayrılmış depolama ve işlem, depolamanın ve işlemin bağımsız olarak ölçeklendirilmesini sağlar. SQL Database'in hızlı ve basit dağıtım yapısı, ek işlemlerin anında kullanılabilir olmasını sağlar. Azure Storage Blobları bunun tamamlanmasını sağlar. Blobları kullanmak bize kararlı ve çoğaltılmış depolama sağlamanın yanı sıra, düşük maliyetle zahmetsiz genişlemenin altyapısını da sunar.  Bu bulut ölçekli depolama ve Azure işlem birleşimini kullanan SQL Data Warehouse, ihtiyacınız olduğu kadar ve ihtiyacınız olduğu zaman sorgu performansı depolama alanı için ödeme yapmanıza olanak tanır. İşlem miktarını değiştirmek Azure portalında bir kaydırıcıyı sola veya sağa hareket ettirmek kadar basittir, aynı zamanda T-SQL ve PowerShell kullanılarak zamanlanabilir.

Depolamadan bağımsız olarak işlem miktarını tamamen kontrol etme becerisinin yanı sıra, SQL Data Warehouse veri ambarınızı tamamen duraklatmanıza da olanak tanır. Depolamanız yerinde tutulurken, tüm işlemler Azure'ın ana havuzuna serbest bırakılarak anında paradan tasarruf etmeniz sağlanır. Gerekli olduğu zaman, işlemi sürdürmeniz ve verilerinizi ve işleminizi iş yükü için kullanılabilir duruma getirmeniz yeterlidir.

SQL Data Warehouse'daki işlem kullanımı, SQL Data Warehouse Birimleri (DWU'lar) ile ölçülür. DWU'lar veri ambarınızın sahip olduğu temel alınan gücü ölçer ve herhangi bir zamanda ambarınızla ilgili olarak standart bir performans miktarına sahip olduğunuzdan emin olmanızı sağlar.  Özel olarak, DWU'ları şunları sağlamak için kullanırız:

- Temel alınan donanım veya yazılım hakkında endişelenmeden, veri ambarınızı etkili şekilde ölçeklendirebilirsiniz.

- Veri ambarınızın boyutunu değiştirmeden önce bir DWU düzeyinde gördüğünüz performansı anlayabilirsiniz.

- Sizin örneğinizdeki temel alınan donanım veya yazılım, iş yükü performansınızı etkilemeden değişebilir veya taşınabilir

- Hizmetin temel alınan mimarisinde, iş yükünüzün performansını etkilemeden ayarlamalar yapabiliriz.

- SQL Data Warehouse'da performansı hızla geliştirirken, bunu sistemi eşit şekilde etkileyecek ve ölçeklenebilir bir şekilde yaptığımızdan emin olabiliriz.

### Data Warehouse Birimleri

Özel olarak Data Warehouse Birimleri'ni, veri ambarı iş yükü performansı ile son derece bağıntılı bulduğumuz üç hassas ölçüm olarak ele alıyoruz. Genel uygunluk durumumuz için bu temel iş yükü ölçümlerinin veri ambarınıza yönelik seçtiğiniz DWU'larla doğrusal olarak ölçeklendirilmesini hedefliyoruz.

**Tarama/Toplama:** Bu iş yükü ölçümü, çok sayıda satırı tarayan ve ardından karmaşık bir toplama işlemi gerçekleştiren standart bir veri ambarı sorgusu alır. Bu, GÇ ve CPU yoğunluklu bir işlemdir.

**Yük:** Bu ölçüm, hizmete veri alma becerisini ölçer. Yükler, PolyBase'in bir Azure Storage Blobundan temsili bir veri kümesi yüklemesiyle tamamlanır. Bu ölçüm, hizmetin Ağ ve CPU yönlerine stres testi uygulamak için tasarlanmıştır.

**CREATE TABLE AS SELECT (CTAS):** CTAS, bir tablonun kopyasını oluşturma becerisini ölçer. Bu da verilerin depolama alanından okunmasını, gerecin tüm düğümlerine dağıtılmasını ve yeniden depolama alanına yazılmasını içerir. CPU ve Ağ yoğunluklu bir işlemdir.

### Ne zaman ölçeklendirme yapılmalıdır?

Genel olarak, DWU'ların basit olmasını istiyoruz. Daha hızlı sonuçlar almanız gerektiğinde, daha yüksek performans elde etmek için DWU'larınızı artırın.  Daha düşük bir işlem gücüne gereksinim duyduğunuzda, DWU'larınızı azaltarak yalnızca ihtiyacınız olduğu ölçüde ödeme yapın. Şu durumlarda DWU'larınızın sayısını artırmayı düşünebilirsiniz:

- Akşamları ve hafta sonları gibi sorgu çalıştırmanızın gerekli olmadığı zamanlarda işlem kaynaklarını duraklatarak, çalışan tüm sorguları iptal edin ve veri ambarınız için ayrılmış olan tüm DWU'ları kaldırın.

- Çok miktarda verinin yüklendiği veya dönüştürüldüğü bir işlemi gerçekleştirirken, verilerinizin daha hızlı kullanılabilir olmasını sağlamak için ölçeklemeyi artırmak isteyebilirsiniz.

- İdeal DWU değerinizin ne olduğunu anlamak için verilerinizi yükledikten sonra ölçeği artırmayı veya azaltmayı ve birkaç sorgu çalıştırmayı deneyin. Ölçeklendirme hızla gerçekleştiği için bir saatten daha fazla yürütme yapmadan çeşitli performans düzeylerini deneyebilirsiniz.

> [AZURE.NOTE] SQL Data Warehouse'un mimarisi nedeniyle, daha düşük veri hacimlerinde beklenen performans özelliklerini görmeyebilirsiniz.  Performans avantajlarının ne düzeyde olduğunu doğru şekilde ölçebilmek için 1 TB'lik veya 1 TB'nin üzerindeki veri hacimleriyle başlamanızı öneririz.

## Tümleşik

SQL Data Warehouse, SQL Server'ın kanıtlanmış ilişkisel veritabanı altyapısını temel alır ve bir kurumsal veri ambarından beklediğiniz birçok özelliği kapsar. Transact-SQL'i tanıyorsanız bildiklerinizi SQL Data Warehouse'a aktarmanız son derece kolaydır. İleri düzeyde veya kullanmaya yeni başlıyor olabilirsiniz, belgelerin genelinde sağlanan örnekler başlangıçta size yardımcı olur. Genel olarak, SQL Data Warehouse'un dil öğelerini şu şekilde oluşturduğumuzu kabul edebilirsiniz:

- SQL Data Warehouse, birçok işlem için SQL Server'ın Transact-SQL (TSQL) söz dizimini kullanır ve depolanan yordamlar, kullanıcı tanımlı işlevler, tablo bölümleme, dizinler ve harmanlamalar gibi çok sayıda geleneksel SQL yapısını destekler.

- SQL Data Warehouse kümelenmiş columnstore dizinleri, PolyBase tümleştirme ve Veri Denetimi (Tehdit Değerlendirmesi ile tamamlanan) dahil olmak üzere, en son teknoloji ürünü çeşitli SQL Server özelliklerini de içerir.

- SQL Data Warehouse halen geliştirilme aşamasındadır, bu nedenle veri ambarı iş yüklerinde daha nadir kullanılan veya SQL Server için daha yeni olan bazı TSQL dil öğeleri şu anda kullanılamayabilir. Bu konuyla ilgili daha fazla bilgi için Geçiş belgelerimize bakın.

SQL Server, SQL Data Warehouse, SQL Database ve Analiz Platformu Sistemi arasındaki ortak özellikler ve Transact-SQL sayesinde, veri ihtiyaçlarınıza uygun olan bir çözümü geliştirebilirsiniz. Performans, güvenlik ve ölçeklendirme gereksinimlerine göre verilerinizi saklamayı ve ardından verileri gereken şekilde farklı sistemler arasında aktarmayı seçebilirsiniz.

SQL Server'ın TSQL yüzey alanını benimsemenin yanı sıra, SQL Data Warehouse aynı zamanda SQL Server kullanıcılarının alışık olabilecekleri birçok araçla da tümleşir. Özel olarak SQL Data Warehouse ile tümleştirmek üzere aşağıdakileri de içeren birkaç araç kategorisine odaklandık:

**Geleneksel SQL Server Araçları:** SQL Data Warehouse'da SQL Server Analysis Services, Integration Services ve Reporting Services ile tam tümleştirme yapılabilir.

**Bulut Tabanlı Araçlar:** SQL Data Warehouse; Azure'daki bir dizi yeni araçla birlikte kullanılabilir ve Azure Data Factory, Stream Analytics, Machine Learning ve Power BI ile kapsamlı şekilde tümleştirilebilir.

**Üçüncü Taraf Araçları:** Çok sayıda üçüncü taraf aracı sağlayıcısına ait araç, SQL Data Warehouse ile sertifikalı tümleştirmeye sahiptir. Tam listeye bakın.

## Karma

Kullanıcılar, SQL Data Warehouse'u PolyBase ile birlikte kullanarak verileri ekosistemleri içinde taşıma konusunda benzersiz bir beceriye sahip olur ve ilişkisel olmayan ve şirket içi veri kaynakları ile gelişmiş karma senaryolar kurma becerisini ortaya çıkarırlar.

Polybase'in kullanımı kolaydır ve farklı kaynaklardaki verilerinizi alışık olduğunuz T-SQL komutlarıyla kullanmanızı sağlar. Polybase, Azure blob depolamada tutulan ilişkisel olmayan verileri olağan bir tablo gibi sorgulamanıza olanak tanır. Polybase'i kullanarak ilişkisel olmayan verileri sorgulayabilir veya ilişkisel olmayan verileri SQL Data Warehouse'a içeri aktarabilirsiniz.

- PolyBase ilişkisel olmayan verilere erişmek için dış tabloları kullanır. Tablo tanımları SQL Data Warehouse'da depolanır ve normal ilişkisel verilere erişirken kullandığınız araçlarla ve SQL yoluyla tablo tanımlarına erişilebilir.

- Polybase, tümleştirme sırasında bağımsız şekilde hareket eder. Desteklediği tüm kaynaklar için aynı özellikleri ve işlevleri kullanıma sunar. Polybase tarafından okunan veriler, sınırlandırılmış dosyalar veya ORC dosyaları dahil olmak üzere çeşitli biçimlerde olabilir.

- PolyBase bir HD Insight kümesinin depolama alanı olarak da kullanılan blob depolamaya erişmek için kullanılabilir, bu sayede ilişkisel ve ilişkisel olmayan araçları kullanarak söz konusu verilere gelişmiş bir erişim sağlamış olursunuz.

## Sonraki adımlar

SQL Data Warehouse hakkında artık kısmen bilgi sahibi olduğunuza göre, [veri ambarı iş yükü], bir SQL Data Warehouse [sağlama] ve [örnek veriler yükleme] hakkında bilgi alın.  Alternatif olarak, aşağıdaki diğer SQL Data Warehouse Kaynakları'na göz atın.  

- [Bloglar] 
- [Özellik İstekleri]
- [Videolar]
- [CAT Ekibi Blogları]
- [Destek Bileti Oluşturun]
- [MSDN Forumu]
- [Stack Overflow Forumu]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Destek Bileti Oluşturun]: ./sql-data-warehouse-get-started-create-support-ticket.md
[veri ambarı iş yükü]: ./sql-data-warehouse-overview-workload.md
[örnek veriler yükleme]: ./sql-data-warehouse-get-started-manually-load-samples.md
[sağlama]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other Web references-->
[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[CAT Ekibi Blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Özellik İstekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN Forumu]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow Forumu]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse



<!----HONumber=Jun16_HO2-->


