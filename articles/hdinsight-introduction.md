<properties urlDisplayName="HDInsight Introduction" pageTitle="HDInsight içerisindeki Hadoop'a Giriş | Azure" metaKeywords="" description="Learn how Azure HDInsight uses Apache Hadoop clusters in the cloud, to provide a software framework to manage, analyze, and report on big data." metaCanonical="" services="hdinsight" documentationCenter="" title="Introduction to Hadoop in HDInsight" authors="bradsev" solutions="" manager="paulettm" editor="cgronlun" />

<tags ms.service="hdinsight" ms.workload="big-data" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="01/01/1900" ms.author="bradsev"></tags>

# HDInsight içerisindeki Hadoop'a Giriş

## Genel Bakış

Azure HDInsight, bulutta Apache&#x99; Hadoop(r) kümelerini dağıtan ve sağlayan, büyük verileri yönetmek, çözümlemek ve raporlamak üzere tasarlanmış bir yazılım çerçevesi sağlayan bir hizmettir.

### Büyük veriler


Veriler sürekli artan hacimlerde, artan yüksek hızlarda ve artan çeşitlilikteki yapılandırılmamış biçimlerde ve değişken semantik bağlamlarda toplandığını belirtmek üzere "büyük veri" olarak tarif edilir. Büyük verilerin toplanması tek başına bir kuruluş için değer sağlamaz. Büyük verilerin işlem yapılabilir bilgiler veya anlayış halinde değer sağlaması için doğru soruların sorulması ve sorunlarla ilgili verilerin toplanması yeterli değildir; veriler erişilebilir olmalı, temizlenmeli, çözümlenmeli ve çoğu zaman şu anda karma olarak bilinen bir bakış açısı ve bağlam oluşturan diğer çeşitli kaynaklardan verilerle birlikte faydalı bir şekilde sunulmalıdır.

### Apache Hadoop
Apache Hadoop, büyük veri yönetimini ve çözümlemesini kolaylaştıran bir yazılım çerçevesidir. Apache Hadoop çekirdeği, Hadoop Dağıtılmış Dosya Sistemi (HDFS) ile güvenilir veri depolaması ve buna paralel olarak bu dağıtılmış sistemde depolanmış verileri işleyip çözümlemeye yönelik basit bir MapReduce programlama modeli sağlar. HDFS, böyle yüksek oranda dağıtılmış sistemleri dağıtırken ortaya çıkan donanım hatası sorunlarını çözmek üzere veri çoğaltmayı kullanır.

### MapReduce ve YARN

Yapılandırılmamış verilerin çeşitli kaynaklardan çözümlenmesiyle ilgili karmaşıklığı azaltmak amacıyla MapReduce programlama modeli, eşleme ve indirgeme işlemleri için kapanışı sağlama alan bir çekirdek özeti sağlar. MapReduce programlama modeli, tüm işlerini anahtar-değer çiftlerinden oluşan veri kümeleri üzerinden hesaplamalar olarak görüntüler. Bu nedenle hem giriş hem de çıkış dosyaları, yalnızca anahtar-değer çiftlerinden oluşan veri kümelerini içermelidir. Bu kısıtlamanın birincil çözümü, MapReduce işlerinin sonuç olarak birleştirilebilir olmasıdır.

Hadoop ile ilgili Pig ve Hive gibi diğer projeler, HDFS ve MapReduce çerçevesi üzerinde oluşturulur. Bunlar gibi projeler, bir kümeyi yönetmek için doğrudan MapReduce programlarıyla çalışmaktan daha basit bir yol sağlamak için kullanılır. Örneğin Pig, kümedeki MapReduce programlarına derlenen Pig Latin adlı bir yordam dilini kullanarak program yazmanızı sağlar. Ayrıca veri akışını yönetmek için akıcı denetimler sağlar. Hive, daha sonra HiveQL adlı bildirimsel bir dilde SQL benzeri deyimler kullanarak sorgulanabilecek bir kümede depolanmış dosyalardaki veriler için bir tablo özeti sağlayan veri ambarı altyapısıdır.

### HDInsight
Azure HDInsight, Apache Hadoop'u bulutta hizmet olarak kullanılabilir hale getirir. HDFS/MapReduce yazılım çerçevesini ve Pig, Hive ve Oozie gibi ilgili projeleri daha basit, daha ölçeklenebilir ve uygun maliyetli bir ortamda kullanılabilir hale getirir.

Hizmetin kullanılabilirliğini artırmak amacıyla HDInsight tarafından dağıtılan Hadoop kümelerine ikinci bir baş düğümü eklenmiştir. Hadoop kümelerinin standart uygulamalarında genellikle tek bir baş düğümü bulunur. HDInsight, ikinci bir baş düğümü ekleyerek bu tek hata noktasını ortadan kaldırır. Müşteri çok büyük baş düğümüne sahip kümeler sağlamadıkça yeni HA küme yapılandırmasına geçiş, kümenin fiyatını değiştirmez.

HDInsight'ın sağladığı başlıca verimliliklerden biri, verileri nasıl yönetip depoladığıyla ilgilidir. HDInsight, varsayılan dosya sistemi olarak Azure Blob depolama birimini kullanır. Blob depolama ve HDFS, sırasıyla veri depolama ve bu verilerin üzerindeki hesaplamalar için iyileştirilmiş ayrı dosya sistemleridir.

- Azure Blob depolama birimi, HDInsight kullanılarak işlenecek veriler için yüksek oranda ölçeklenebilir ve kullanılabilir, düşük maliyetli, uzun vadeli ve paylaşılabilir bir depolama seçeneği sağlar.
- HDInsight tarafından HDFS üzerinde dağıtılan Hadoop kümeleri, veriler üzerinde MapReduce hesaplama görevlerini çalıştırmak üzere iyileştirilmiştir.

HDInsight kümeleri, MapReduce görevlerini yürütmek için Azure'da hesaplama düğümlerine dağıtılır ve bu görevler tamamlandıktan sonra kullanıcılar tarafından bırakılabilir. Hesaplamalar tamamlandıktan sonra verilerin HDFS kümelerinde tutulması, bu verileri depolamanın pahalı bir yoludur. Blob depolama birimi, sağlam ve genel amaçlı bir Azure depolama çözümüdür. Bu nedenle, verilerin Blob depolama biriminde depolanması, hesaplama için kullanılan kümelerin, kullanıcı verileri kaybedilmeden güvenli bir şekilde silinmesini sağlar. Ancak, Blob depolama birimi yalnızca düşük maliyetli bir çözüm değildir: Hadoop ekosistemindeki tüm bileşenlerin varsayılan olarak yönettiği veriler üzerinde doğrudan çalışmasını sağlayarak, müşterilere sorunsuz bir deneyim sunan, tam özellikli bir HDFS dosya sistemi arabirimidir.

HDInsight, Hadoop işlerini yapılandırmak, çalıştırmak ve son işlemden geçirmek için Azure PowerShell kullanır. HDInsight, ayrıca bir Azure SQL veritabanından HDFS'ye verileri içe aktarmak veya verileri Azure SQL veritabanından HDFS'ye dışa aktarmak için kullanılabilen bir Sqoop bağlayıcısı sağlar.

HDInsight, ayrıca YARN'ı kullanılabilir hale getirmiştir. Hadoop kümelerinde verileri işlemek için klasik Apache Hadoop MapReduce çerçevesinin yerine geçen yeni, genel amaçlı, dağıtılmış bir uygulama yönetimi çerçevesidir. Hadoop işletim sistemi olarak etkili bir biçimde hizmet verir ve Hadoop'u toplu işlem için tek kullanımlık veri platformundan toplu, etkileşimli, çevrimiçi ve akışa dayalı işlem sağlayan çok kullanımlık bir platforma taşır. Bu yeni yönetim çerçevesi kapasite garantileri, tarafsızlık ve hizmet düzeyi sözleşmeleri gibi ölçütlere göre ölçeklenebilirlik ve küme kullanımını iyileştirir.

Excel için Microsoft Power Query; Azure HDInsight'tan veya herhangi bir HDFS'den Excel'e veri aktarmak için kullanılabilir. Bu eklenti, veri bulmayı ve çok çeşitli veri kaynaklarına erişimi kolaylaştırarak Excel'deki self servis BI deneyimini geliştirir. Power Query'e ek olarak Excel, SQL Server Analysis Services ve Reporting Services gibi iş zekası (BI) araçlarını tümleştirmeye ve uçtan uca veri çözümlemeyi kolaylaştırmaya yönelik Microsoft Hive ODBC Sürücüsü mevcuttur.

### Anahat
Bu konuda, HDInsight tarafından desteklenen Hadoop ekosistemi, HDInsight için ana kullanım senaryoları ve diğer kaynaklarla ilgili bir kılavuz sağlanmaktadır. Kılavuz aşağıdaki bölümleri içerir:

 * <a href="#Ecosystem">HDInsight Üzerindeki Hadoop Ekosistemi</a>: HDInsight; Pig, Hive, Sqoop, Oozie ve Ambari uygulamalarını sağlar ve Power Query veya Microsoft Hive ODBC Sürücüsü'nü kullanarak Excel, SQL Server Analysis Services ve Reporting Services gibi Blob depolama birimi/HDFS ve MapReduce çerçevesiyle tümleştirilmiş diğer BI araçlarını destekler. Bu bölümde Hadoop ekosistemindeki bu programların hangi işleri gerçekleştirmek üzere tasarlandığı açıklanmaktadır.

 * <a href="#Scenarios">HDInsight için büyük veri senaryoları</a>: Bu bölümde şu soru ele alınmaktadır: HDInsight hangi tür işler için uygun bir teknolojidir?

 * <a href="#Resources">HDInsight Kaynakları</a>: Bu bölümde ek bilgi için ilgili kaynakların nerede bulunacağı gösterilmektedir.


<h2 id="ecosystem"> <a name="Ecosystem">Azure üzerindeki Hadoop ekosistemi </a></h2>

### Giriş

HDInsight, büyük verileri işlemek için Microsoft'un bulut tabanlı çözümünü uygulayan bir çerçeve sağlar. Bu federasyon ekosistemi, büyük miktarlarda verileri yönetip çözümlerken MapReduce programlama modelinin paralel işleme özelliklerinden faydalanır. HDInsight ile kullanılabilen ve Apache ile uyumlu Hadoop teknolojileri, bu bölümde madde madde verilmiş ve kısaca açıklanmıştır.

HDInsight, veri işleme ve depolama özelliklerini tümleştirmek için Hive ve Pig uygulamalarını sağlar.  Microsoft'un büyük veri çözümü; SQL Server Analysis Services, Reporting Services, PowerPivot ve Excel gibi Microsoft BI araçlarıyla tümleştirilir. Bu durum, HDInsight tarafından Blob depolama biriminde depolanıp yönetilen veriler üzerinde sorunsuz bir BI gerçekleştirmenizi sağlar. 

Apache ile uyumlu diğer teknolojiler ve Hadoop ekosisteminin parçası olup Hadoop kümelerinin üzerinde oluşturulmuş kardeş teknolojiler de indirilerek HDInsight ile birlikte kullanılabilir. Bunlar HDFS'yi ilişkisel veri depolarıyla tümleştiren Sqoop gibi açık kaynak teknolojilerini içerir. 

### Pig	

Pig, Hadoop kümelerinde büyük verileri işlemeye yönelik yüksek düzeyli bir platformdur. Pig, büyük veri kümelerine sorgu yazmayı ve bir konsoldan program çalıştıran yürütme ortamını destekleyen Pig Latin adlı bir veri akışı dilinden oluşur. Pig Latin programları kapaklar altında bir MapReduce program serisine dönüştürülen veri kümesi dönüşüm serisinden oluşur. Pig Latin özetleri, MapReduce'tan daha zengin veri yapıları sağlar ve SQL'in İlişkisel Veritabanı Yönetim Sistemleri (RDBMS) için gösterdiği performansı Hadoop için gösterir. Pig Latin tam olarak genişletilebilir. Java, Python, Ruby, C# veya JavaScript dilinde yazılmış Kullanıcı Tanımlı İşlevler (UDF'ler), çözümlemeyi birleştirirken her bir işlem yolu aşamasını özelleştirmek için çağrılabilir. Daha fazla bilgi için, bkz. [Apache Pig'e Hoş Geldiniz!](http://pig.apache.org/)

### Hive	

Hive bir HDFS'de depolanmış verileri yöneten, dağıtılmış bir veri ambarıdır. Hadoop sorgu alt yapısıdır. Hive, SQL benzeri bir arabirim ve ilişkisel veri modeli sağlayan güçlü SQL becerilerine sahip analiz uzmanlarına yöneliktir. Hive; bir SQL diyalekti olan HiveQL adlı bir dil kullanır. Aynı zamanda Hive, Pig gibi MapReduce üzerinde oluşturulmuş bir özettir ve çalıştırıldığında sorguları bir dizi MapReduce işine çevirir. Hive senaryoları kavram olarak RDBMS senaryolarına daha yakındır ve bu nedenle daha fazla yapılandırılmış veriyle kullanıma uygundur. Yapılandırılmamış veriler için Pig daha iyi bir seçimdir. Daha fazla bilgi için, bkz. [Apache Hive'e Hoş Geldiniz!](http://hive.apache.org/)

### Sqoop		

Sqoop; Hadoop ile SQL gibi ilişkisel veritabanları veya diğer yapılandırılmış veri depoları arasında toplu verileri mümkün olduğunca verimli bir şekilde aktaran bir araçtır. Harici yapılandırılmış veri depolarını HDFS'ye veya Hive gibi ilgili sistemlere aktarmak için Sqoop kullanın. Sqoop, ayrıca Hadoop'tan verileri ayıklayabilir ve ayıklanan verileri harici ilişkisel veritabanlarına, kurumsal veri ambarlarına veya yapılandırılmış diğer veri ambarı türlerine aktarabilir. Daha fazla bilgi için, bkz. [Apache Sqoop](http://sqoop.apache.org/) Web sitesi.

### Oozie

Apache Oozie, Hadoop işlerini yöneten bir iş akışı/eşgüdüm sistemidir. Hadoop yığını ile tümleştirilir ve Apache MapReduce, Apache Pig, Apache Hive ve Apache Sqoop için Hadoop işlerini destekler. Ayrıca, Java programları veya kabuk betikleri gibi belirli bir sisteme özel işleri planlamak için kullanılabilir.

### Ambari

Apache Ambari; Apache Hadoop kümelerini sağlamaya, yönetmeye ve izlemeye yöneliktir. Hadoop karmaşıklığını gizleyen ve kümelerin çalışmasını basitleştiren kullanımı kolay bir operatör araçları koleksiyonu ve sağlam bir API kümesi içerir. API'ler hakkında daha fazla bilgi için, bkz. [Ambari API başvurusu](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md). HDInsight şu anda yalnızca Ambari izleme özelliğini desteklemektedir. Ambari API sürümü 1.0; HDInsight küme sürümü 2.1 ve 3.0 tarafından desteklenir. Ambari hakkında daha fazla bilgi için, bkz. [Apache Ambari](http://ambari.apache.org/) Web sitesi.


### Microsoft Avro Kitaplığı

Microsoft Avro Kitaplığı, Microsoft.NET ortamı için Apache Avro veri serileştirme sistemini uygular. Apache Avro, serileştirme için sıkıştırılmış bir ikili veri değişim biçimi sağlar. Dillerin birlikte çalışabilirliğini sağlayan dil agnostik şemasını tanımlamak üzere [JSON](http://www.json.org) kullanır. Bir dilde serileştirilmiş veriler başka bir dilde kullanılabilir. Şu anda C, C++, C#, Java, PHP, Python ve Ruby desteklenmektedir. Biçim hakkında ayrıntılı bilgiler [Apache Avro Belirtimi](http://avro.apache.org/docs/current/spec.html) içinde bulunabilir. Microsoft Avro Kitaplığı'nın geçerli sürümü bu belirtimin Uzak Yordam Çağrıları (RPC) bölümünü desteklemez.

Apache Avro serileştirme biçimi Azure HDInsight ve diğer Apache Hadoop ortamlarında yaygın olarak kullanılır. Avro bir Hadoop MapReduce işindeki karmaşık veri yapılarını temsil etmenin kolay bir yolunu sağlar. Avro dosyalarının biçimi, dağıtılmış MapReduce programlama modelini destekleyecek şekilde tasarlanmıştır. Dağıtımı sağlayan temel özellik, dosya içinde herhangi bir noktanın aranabilmesi ve belirli bir bloktan itibaren okumaya başlanabilmesi bakımından dosyaların "bölünebilir" olmasıdır. Daha fazla bilgi için, bkz. [Microsoft Avro Kitaplığı ile verileri serileştirme](../hdinsight-dotnet-avro-serialization/).

### İş zekası araçları ve bağlayıcıları

Excel, PowerPivot, SQL Server Analysis Services ve Reporting Services gibi bilinen iş zekası (BI) araçları, Power Query eklentisini veya Microsoft Hive ODBC Sürücüsünü kullanarak HDInsight ile tümleştirilmiş verileri alır, çözümler ve raporlar.

 * Excel için Microsoft Power Query; [Microsoft Yükleme Merkezi](http://go.microsoft.com/fwlink/?LinkID=286689)'nden indirilebilir.

 * Microsoft Hive ODBC Sürücüsü bu [Yükleme Sitesi](http://go.microsoft.com/fwlink/?LinkID=286698)'nden indirilebilir.

 * Analysis Services hakkında bilgi için, bkz. [SQL Server 2012 Analysis Services](http://www.microsoft.com/sqlserver/en/us/solutions-technologies/business-intelligence/SQL-Server-2012-analysis-services.aspx).	

 * Reporting Services hakkında bilgi için, bkz. [SQL Server 2012 Reporting](http://www.microsoft.com/tr-tr/sqlserver/solutions-technologies/business-intelligence/reporting.aspx).	


<h2> <a name="Scenarios"></a>HDInsight için büyük veri senaryoları</h2>
HDInsight için kullanım örneği sağlayan örnek bir senaryo, Azure düğümlerinde depolanan tüm yapılandırılmamış veri kümesi üzerinde toplu olarak yapılan ve sık güncelleştirme gerektirmeyen geçici bir analizdir.

Bu koşullar iş, bilim ve yönetim alanlarında çok çeşitli etkinlikler için geçerlidir. Bunlar, örneğin perakende sektöründe tedarik zincirlerini, finansta şüpheli alım-satım modellerini, kamu sektörü için talep modellerini ve bir dizi ortam algılayıcıdan hava ve su kalitesini ya da metropol bölgelerindeki suç eğilimlerini içerebilir.

HDInsight (ve genel olarak Hadoop teknolojileri), yazıldıktan sonra sık güncelleştirme gerektirmeyen ve genellikle tam bir çözümleme yapmak için sıklıkla okunan, günlüğe veya arşive kaydedilmiş büyük miktarlarda verileri işlemek için çok uygundur. Bu senaryo, daha az miktarda veri (petabayt yerine gigabayt) gerektiren ve tam veri kümesindeki belirli veri noktaları için sürekli olarak güncelleştirilmesi veya sorgulanması gereken bir RDBMS tarafından daha uygun bir şekilde işlenen verileri tamamlar. RDBMS, sabit bir şemaya göre düzenlenip depolanan yapılandırılmış verilerle en iyi şekilde çalışır. MapReduce, işleme sırasında yapılandırılmamış verileri yorumlayabildiği için önceden tanımlanmış bir şema olmadan yapılandırılmamış verilerle iyi bir şekilde çalışır.

<h2> <a name="Resources"></a>Sonraki adımlar: HDInsight Kaynakları</h2>
**Microsoft: HDInsight**	

* [HDInsight Belgeleri](http://go.microsoft.com/fwlink/?LinkID=285601): Makale, video ve diğer kaynakların bağlantılarını içeren Azure HDInsight belge sayfası.

* [HDInsight Sürüm Notları](http://azure.microsoft.com/tr-tr/documentation/articles/hdinsight-release-notes/): En son sürümlere ilişkin notlar.

* [Azure HDInsight'ı Kullanmaya Başlama][hdinsight-get-started]: HDInsight kullanımıyla ilgili hızlı başlangıç sağlayan bir öğretici.

* [HDInsight örneklerini çalıştırma][hdinsight-samples]: HDInsight ile birlikte gönderilen örneklerin nasıl çalıştırılacağıyla ilgili bir öğretici.

* [Büyük veriler ve Azure](http://azure.microsoft.com/tr-tr/solutions/big-data/): Azure ile ne oluşturabileceğinizi araştıran büyük veri senaryoları.	

* [Azure HDInsight SDK'sı](http://msdnstage.redmond.corp.microsoft.com/tr-tr/library/dn479185.aspx): HDinsight SDK'sı için başvuru belgeleri.

**Microsoft: Windows ve SQL Veritabanı**	

* [Azure giriş sayfası](http://azure.microsoft.com/tr-tr/): Uygulama oluşturmaya başlamak için ihtiyaç duyduğunuz senaryolar, ücretsiz deneme kaydı, geliştirme araçları ve belgeler.
		
* [Azure SQL Veritabanı](http://msdn.microsoft.com/tr-tr/library/windowsazure/ee336279.aspx): SQL Veritabanı için MSDN belgeleri.
	
* [SQL Veritabanı için Yönetim Portalı](http://msdn.microsoft.com/tr-tr/library/windowsazure/gg442309.aspx): Bulutta SQL Veritabanını yönetmeye yönelik, hafif ve kullanımı kolay bir veritabanı yönetim aracı.

* [SQL Veritabanı için Adventure Works](http://msftdbprodsamples.codeplex.com/releases/view/37304): SQL Veritabanı örnek veritabanı için indirme sayfası.	

**Microsoft: İş zekası**		

* [Excel'i Power Query ile HDInsight'a bağlama][hdinsight-power-query]: Excel için Microsoft Power Query kullanarak Excel'in HDInsight kümenizle ilişkili verileri depolayan Azure depolama hesabına nasıl bağlanacağını öğrenin. 

* [Microsoft Hive ODBC Sürücüsü][hdinsight-ODBC] ile HDInsight'ı Excel'e Bağlama: Azure HDInsight'tan Microsoft Hive ODBC Sürücüsü ile verilerin nasıl içeri aktarılacağını öğrenin.

* [Microsoft BI PowerPivot](http://www.microsoft.com/tr-tr/bi/PowerPivot.aspx): Güçlü veri karma ve keşif aracını indirin ve hakkında bilgi alın.
			
* [SQL Server 2012 Analysis Services](http://www.microsoft.com/sqlserver/en/us/solutions-technologies/business-intelligence/SQL-Server-2012-analysis-services.aspx): SQL Server 2012'nin bir değerlendirme kopyasını indirin ve işleme dönüştürülebilir fikirler sunan kapsamlı, kurumsal ölçekte analitik çözümlerin nasıl oluşturulacağını öğrenin.	

* [SQL Server 2012 Reporting](http://www.microsoft.com/sqlserver/en/us/solutions-technologies/business-intelligence/SQL-Server-2012-reporting-services.aspx): SQL Server 2012'nin bir değerlendirme kopyasını indirin ve şirket genelinde gerçek zamanlı karar almayı sağlayan kapsamlı, yüksek oranda ölçeklenebilir çözümleri nasıl oluşturacağınızı öğrenin. 
	
**Apache Hadoop**:			

* [Apache Hadoop](http://hadoop.apache.org/): Büyük veri kümelerinin bilgisayar kümelerinde dağıtılmış işlemesine izin veren bir çerçeve olan Apache Hadoop yazılım kitaplığı hakkında daha fazla bilgi alın.	

* [HDFS](http://hadoop.apache.org/docs/r0.18.1/hdfs_design.html): Hadoop uygulamaları tarafından kullanılan birincil depolama sistemi Hadoop Dağıtılmış Dosya Sistemi (HDFS) hakkında daha fazla bilgi alın.		

* [MapReduce](http://mapreduce.org/): Büyük miktarda verileri büyük hesaplama düğümü kümelerine paralel olarak hızlıca işleyen Hadoop uygulamalarını yazmaya yönelik programlama çerçevesi hakkında daha fazla bilgi alın.	

[hdinsight-ODBC]: ../hdinsight-connect-excel-hive-ODBC-driver/
[hdinsight-power-query]: ../hdinsight-connect-excel-power-query/
[hdinsight-get-started]: ../hdinsight-get-started/
[hdinsight-samples]: ../hdinsight-run-samples/

[zookeeper]: http://zookeeper.apache.org/ 
