#Veri Yönetimi ve İş Analizi

Bulut ortamında verilerin yönetilmesi ve çözümlenmesi diğer herhangi bir ortamda olduğu kadar önemlidir. Azure, bunu yapabilmeniz için ilişkisel olan ve ilişkisel olmayan verilerle çalışmaya yönelik bir dizi teknoloji sağlıyor. Bu makalede seçeneklerin her biri takdim edilmektedir. 

##İçindekiler Tablosu      

- [Blob Depolama](#blob)
- [Sanal Makinede DBMS Çalıştırma](#dbinvm)
- [SQL Veritabanı](#sqldb)
	- [SQL Veri Eşitleme](#datasync)
	- [Sanal Makineleri Kullanarak SQL Veri Raporlama](#datarpt)
- [Tablo Depolama](#tblstor)
- [Hadoop](#hadoop)

## <a name="blob"></a>Blob Depolama

"Blob" sözcüğü "İkili Büyük Nesne"nin kısaltması olup blob'un ne olduğunu tam anlamıyla açıklar: İkili bilgiler koleksiyonu. Blob'lar basit olmakla birlikte oldukça faydalıdır. [Şekil 1](#Fig1)'de Azure Blob Depolamanın temelleri gösterilmektedir.

<a name="Fig1"></a>![Diagram of Blobs][blobs]
 
**Şekil 1: Azure Blob Depolama ikili verileri (blob'lar) kapsayıcılar içinde depolar.**

Blob'ları kullanmak için önce bir Azure *depolama hesabı* oluşturursunuz. Bu işlemin bir parçası olarak, bu hesabı kullanarak oluşturduğunuz nesneleri depolayacağınız Azure veri merkezini belirtirsiniz. Nerede olursa olsun, oluşturduğunuz her bir blob depolama hesabınızdaki bir kapsayıcıya aittir. Bir blob'a erişmek için, uygulamalar şu biçimde bir URL sağlar:

http://<*StorageAccount*>.blob.core.windows.net/<*Container*>/<*BlobName*>

<*StorageAccount*> yeni bir depolama hesabı oluşturulduğunda atanan benzersiz bir tanımlayıcı olurken, <*Container*> ve <*BlobName*> belirli bir kapsayıcının ve bu kapsayıcı içindeki bir blob'un adlarıdır. 

Azure iki farklı türde blob sağlar. Seçenekler şunlardır:

- *Blok* blob'lar: Her biri 200 gigabayta kadar veri içerebilir. Adından da anlaşılacağı gibi, blok blob belirli bir sayıda blok halinde alt bölümlere ayrılmıştır. Blok blogu aktarırken bir hata olursa, tüm blob'u tekrar göndermek yerine yeniden iletim en son blok ile devam edebilir. Blok blob'lar depolamaya yönelik oldukça genel bir yaklaşımdır ve bugünlerde en yaygın olarak kullanılan blob türüdür.

- *Sayfa* blob'ları: Her biri bir terabayt gibi büyük bir boyutta olabilir. Sayfa blob'ları rastgele erişim için tasarlanmıştır ve bu nedenle her biri belirli sayıda sayfaya bölünür. Uygulamalar blob'da ayrı ayrı sayfaları rastgele serbestçe okuyabilir ve yazabilir. Azure Sanal Makineleri'nde, örneğin, oluşturduğunuz sanal makineler sayfa blob'larını hem OS diskleri hem de veri diskleri için kalıcı depolama olarak kullanır.

İster blok blob'ları ister sayfa blob'larını seçin, uygulamalar blob verilerine çeşitli yollarla erişim sağlayabilir. Seçenekler şunları kapsar:

- Bir RESTful (yani, HTTP tabanlı) erişim protokolü aracılığıyla doğrudan. Hem Azure uygulamaları hem de harici uygulamalar (şirket içinde çalışan uygulamalar dahil) bu seçeneği kullanabilir.
- Ham RESTful blob erişim protokolüne ek olarak daha geliştirici dostu bir arabirim sağlayan Azure Depolama İstemci kitaplığını kullanarak. Yine, hem Azure uygulamaları hem de harici uygulamalar bu kitaplığı kullanarak blob'lara erişebilir.
- Azure uygulamasının, sayfa blob'unu NTFS dosya sistemi içeren bir yerel sürücü gibi kabul etmesini sağlayan bir seçenek olarak Azure sürücülerini kullanarak. Uygulama, sayfa blob'unu standart dosya G/Ç kullanılarak erişim sağlanan sıradan bir Windows dosya sistemi olarak görür. Aslında okunanlar ve yazılanlar, Azure Sürücüsü'nü hayata geçiren temel sayfa blob'una gönderilir. 

Donanım arızalarına karşı koruma sağlaması ve kullanılabilirliği artırması için her blob Azure veri merkezinde üç bilgisayara çoğaltılır. Bir blob'a yazıldığında üç kopya da güncellenir ve böylece sonraki okumalarda tutarsız sonuçlar görülmez. Ayrıca, bir blob'un verilerinin aynı coğrafyada bulunan, ancak en az 800 km (500 mil) uzaktaki başka bir Azure veri merkezine kopyalanması gerektiğini de belirtebilirsiniz. *Coğrafi çoğaltma* olarak adlandırılan bu kopyalama işlemi, blob'da güncelleme yapıldıktan sonra birkaç dakika içinde gerçekleşir ve olağanüstü durum kurtarmada yararlıdır.

Blob'lardaki veriler, Azure *İçerik Teslim Ağı(CDN)* aracılığıyla da kullanıma sunulabilir. Blob verilerinin kopyalarının dünya çapında düzinelerce sunucuda önbelleğe alınmasıyla, CDN tekrar tekrar erişim sağlanan bilgilere erişimi hızlandırabilir. 

Blob'lar basit oldukları kadar, çoğu durumda da en doğru tercihtir. Depolama ve video ve ses akışları bariz örneklerdir; yedeklemeler ve diğer tür veri arşivleri de örnek olarak verilebilir. Geliştiriciler, istedikleri her tür yapılandırılmamış veriyi tutmak için de blob'ları kullanabilir. Elinizde ikili verileri depolamak ve bunlara erişmek için dolaysız bir yöntem bulunması şaşırtıcı derecede yararlı olabilir.


## <a name="dbinvm"></a>Sanal Makinede DBMS Çalıştırma

Günümüzde birçok uygulama bir tür veritabanı yönetim sistemine (DBMS) bağımlıdır. SQL Server gibi ilişkisel sistemler en sık kullanılan seçenektir; ancak genelde *NoSQL* teknolojileri olarak bilinen ilişkisel olmayan yaklaşımların popülerliği de her geçen gün artmaktadır. Bulut uygulamalarının bu veri yönetimi seçeneklerini kullanabilmesini sağlamak için, Azure Sanal Makineleri sanal makinede bir DBMS (ilişkisel veya NoSQL) çalıştırmanıza imkan tanır. [Şekil 2](#Fig2)'de bunun SQL Server ile nasıl göründüğü gösterilmektedir.

<a name="Fig2"></a>![Diagram of SQL Server in a Virtual Machine][SQLSvr-vm]
 
**Şekil 2: Azure Sanal Makineleri, blob'ların sağladığı süreklilik sayesinde sanal makinede DBMS çalıştırmaya olanak sağlar.**

Hem geliştiriciler hem de veritabanı yöneticileri açısından bu senaryo, aynı yazılımı kendi veri merkezlerinde çalıştırmaya çok benzer. Mesela burada gösterilen örnekte, SQL Server'ın kabiliyetlerinin neredeyse tümü kullanılabilir ve sisteme tam yönetimsel erişiminiz olur. Ayrıca, tıpkı yerel olarak çalışıyormuş gibi veritabanı sunucusunu yönetme sorumluluğunuz da şüphesiz vardır.

[Şekil 2](#Fig2)'de gösterildiği gibi, veritabanlarınız sunucunun çalıştığı sanal makinenin yerel diskinde depolanıyor görünür. Ancak arka planda aslında bu disklerin her biri Azure blob'una yazılır. (Kendi veri merkezinizde SAN kullanmaya benzer ve blob daha çok bir LUN gibi davranır.) Her Azure blob'unda olduğu gibi, içerdiği veriler bir veri merkezi dahilinde üç kez çoğaltılır ve sizin istemeniz durumunda aynı bölgedeki başka bir veri merkezine coğrafi olarak çoğaltılır. Daha iyi bir güvenilirlik için SQL Server veritabanı yansıtması gibi seçenekleri kullanmak da mümkündür. 

Sanal makinede SQL Server kullanmanın bir diğer yolu, veriler Azure'da hayat bulurken uygulama mantığının şirket içinde çalıştığı karma bir uygulama oluşturmaktır. Örneğin, birden fazla konumda ya da çeşitli mobil cihazlarda çalışan uygulamaların aynı verileri paylaşmasının gerektiği durumlarda bu yöntem mantığa uygun olabilir. Kuruluş, bulut veritabanı ile şirket içi mantık arasındaki iletişimi basitleştirmek için, Azure Sanal Ağını kullanmak suretiyle Azure veri merkezi ile kendi şirket içi veri merkezi arasında bir sanal özel ağ (VPN) bağlantısı oluşturabilir.


## <a name="sqldb"></a>SQL Veritabanı

Yapılandırılmış verilerin bulut ortamında yönetimi için çoğu kişinin ilk aklına gelen seçenek sanal makinede DBMS çalıştırmaktır. Tek seçenek bu olmadığı gibi, her zaman en iyi seçenek de değildir. Bazı durumlarda, Hizmet Olarak Platform (PaaS) yaklaşımının kullanımıyla verileri yönetmek daha mantıklıdır. Azure, ilişkisel veriler için bunu yapmanıza imkan tanıyan ve SQL Veritabanı olarak bilinen bir PaaS teknolojisi sağlar. [Şekil 3](#Fig3)'te bu seçenek gösterilmektedir. 

<a name="Fig3"></a>![Diagram of SQL Database][SQL-db]
 
**Şekil 3: SQL Veritabanı, paylaşılan bir PaaS ilişkisel depolama hizmeti sağlar.**

SQL Veritabanı kendi fiziksel SQL Server örneğini her müşteriye vermez. Bunun yerine, müşteri başına bir mantıksal SQL Veritabanı sunucusuyla çok kiracılı bir hizmet sağlar. Tüm müşteriler hizmetin sağladığı hesaplama ve depolama kapasitesini paylaşır. Ayrıca, Blob Depolamada olduğu gibi, SQL Veritabanı'ndaki tüm verilerin bir Azure veri merkezi dahilinde üç ayrı bilgisayarda depolanması, veritabanlarınıza yerleşik yüksek kullanılabilirlik (HA) özelliği kazandırır.

Uygulamalar SQL Veritabanı'nı daha çok SQL Server gibi görür. Uygulamalar ilişkisel tablolara karşı SQL sorguları verebilir, T-SQL depolanan prosedürlerini kullanabilir ve birden fazla tablo çapında işlem yürütebilir. Üstelik uygulamalar SQL Veritabanı'na erişim sağlamak için Tablo Verisi Akışı (TDS) protokolünü (SQL Server'a erişim için kullanılan aynı protokol) kullandığından, Entity Framework, ADO.NET, JDBC ve diğer bilindik erişim arabirimlerini kullanarak verilerle çalışabilirler. 

Ancak SQL Veritabanı, Azure veri merkezlerinde çalışan bir bulut hizmeti olduğundan, sistemin disk kullanımı gibi fiziksel özelliklerini yönetmenize gerek kalmaz. Yazılımları güncelleme veya diğer alt düzey yönetimsel görevleri idare etme konularını dert etmenize de gerek kalmaz. Her müşteri kuruluş şüphesiz şemalarını ve kullanıcı hesaplarını da içeren kendi veritabanlarını kontrol etmeye devam eder. Ancak sıradan yönetimsel görevlerin birçoğu sizin yerinize yapılmış olur. 

SQL Veritabanı uygulamalara daha çok SQL Server gibi görünse de, fiziksel veya sanal makinede çalışan bir DBMS ile tamamen aynı davranışı sergilemez. Paylaşılan bir donanımda çalıştığından, tüm müşterilerinin bu donanıma yüklediği yüke göre performansı değişecektir. Buna göre, örneğin SQL Veritabanı'nda depolanan bir prosedürün performansı günden güne değişiklik gösterebilir. 

Bugün SQL Veritabanı 150 gigabayta kadar veri barındıran bir veritabanı oluşturmanıza izin vermektedir. Daha büyük veritabanlarıyla çalışmanız gerekirse, bu hizmet *Federasyon* adı verilen bir seçenek sağlar. Bunun için veritabanı yöneticisi, kendi şemasına sahip ayrı birer veritabanı halinde iki veya daha fazla *federasyon üyesi* oluşturur. Veriler, çoğunlukla *paylaşım* olarak anılan bir yöntemle bu üyelere yayılır ve her bir üyeye benzersiz bir *federasyon anahtarı* atanır. Uygulama, sorgunun hedef alması gereken federasyon üyesini tanımlayan federasyon anahtarını belirterek bu verilere karşı SQL sorguları verir. Böylece, büyük veri miktarları için geleneksel bir ilişkisel yaklaşım kullanma olanağı sağlanmış olur. Her zaman olduğu gibi dengelemeler söz konusudur; örneğin, ne sorgular ne de işlemler federasyon üyelerini kapsayamaz. Ancak en iyi seçenek ilişkisel bir PaaS hizmeti olduğunda ve bu dengelemeler kabul edilebilir olduğunda, SQL Federasyonu kullanmak iyi bir çözüm olabilir.

SQL Veritabanı Azure'da ya da şirket içindeki veri merkeziniz gibi başka bir ortamda çalışan uygulamalar tarafından kullanılabilir. Bu özelliği de, hem ilişkisel verilere gerek duyulan bulut uygulamaları hem de verilerin bulutta depolanmasından fayda sağlayabilecek şirket içi uygulamalar için veritabanını yararlı kılar. Örneğin mobil bir uygulama, paylaşılan ilişkisel verileri yönetmek için SQL Veritabanı'nda güvenebileceği gibi, dünya çapında çok sayıda bayide çalışan bir envanter uygulaması için de aynı durum geçerlidir.

SQL Veritabanı'nı düşünürken bariz (ve önemli) bir sorun ortaya çıkar: Hangi durumda sanal ortamda SQL Server çalıştırmalısınız ve hangi durumda SQL Veritabanı daha iyi bir tercih olur? Her zaman olduğu gibi dengelemeler söz konusudur ve bu nedenle, hangi yaklaşımın daha iyi olduğu gereksinimlerinize göre değişir. 

Bu durumu değerlendirmenin basit bir yolu yeni uygulamalar için SQL Veritabanı'nı düşünmektir; var olan bir şirket içi uygulamayı buluta geçirirken ise sanal makinede SQL Server daha iyi bir tercihtir. Bununla birlikte, bu kararı daha ayrıntılı bir şekilde değerlendirmek de yararlı olabilir. Örneğin, kurulum ve yönetim gereksinimleri minimum düzeyde olduğundan SQL Veritabanı'nı kullanmak daha kolaydır. Ancak sanal makinede SQL Server çalıştırmak daha öngörülebilir bir performans sağlayabilir (paylaşılan bir hizmet olmaması nedeniyle) ve ayrıca, SQL Veritabanı'na göre daha büyük federasyon dışı veritabanlarını destekler. Bununla birlikte, SQL Veritabanı hem veriler hem de işlemler için yerleşik bir çoğaltma olanağı sağlayarak, çok az bir çalışmayla kullanılabilirliği yüksek DBMS avantajını etkin bir şekilde size kazandırır. SQL Server daha fazla kontrol ve belirli bir dereceye kadar daha geniş seçenek kümesi sağlarken, SQL Veritabanı'nda kurulum daha basittir ve yönetilecek işler önemli oranda daha azdır.

Son olarak, SQL Veritabanı'nın Azure'da kullanılabilen tek PaaS veri hizmeti olmadığına dikkat çekmek önemlidir. Microsoft ortakları başka seçenekler de sağlamaktadır. Örneğin, ClearDB bir MySQL PaaS teklifi sunarken, Cloudant da bir NoSQL seçeneğini satmaktadır. PaaS veri hizmetleri çoğu durumda en doğru çözümdür ve dolayısıyla, bu veri yönetimi yaklaşımı Azure'un önemli bir parçasıdır.


### <a name="datasync"></a>SQL Veri Eşitleme

SQL Veritabanı tek bir Azure veri merkezi içinde her bir veritabanının üç kopyasını tutmakla birlikte, verileri Azure veri merkezleri arasında otomatik olarak çoğaltmaz. Bunun yerine, bu işlemi yapmak için kullanabileceğiniz SQL Veri Eşitleme hizmetini sağlar. [Şekil 4](#Fig4)'te bunun nasıl göründüğü gösterilmektedir.

<a name="Fig4"></a>![Diagram of SQL data sync][SQL-datasync]
 
**Şekil 4: SQL Veri Eşitleme hizmeti, SQL Veritabanı'ndaki verileri diğer Azure ve şirket içi veri merkezlerindeki verilerle eşitler.**

Diyagramda gösterildiği gibi, SQL Veri Eşitleme farklı konumlar genelinde veri eşitleyebilir. Örneğin, bir uygulamayı birden fazla Azure veri merkezinde çalıştırdığınızı ve verilerin SQL Veritabanı'nda depolandığını varsayalım. Bu verileri eşitlenmiş olarak tutmak için SQL Veri Eşitleme hizmetini kullanabilirsiniz. SQL Veri Eşitleme aynı zamanda, bir Azure veri merkezi ile şirket içi veri merkezinde çalışan bir SQL Server örneği arasında da verileri eşitleyebilir. Hem şirket içi uygulamaların kullandığı verilerin yerel bir kopyasını hem de Azure'da çalışan uygulamaların kullandığı verilerin bulut kopyasını tutmak istediğinizde bunun yararını görebilirsiniz. Ayrıca, şekilde gösterilmemiş olsa da, SQL Veri Eşitleme özelliği SQL Veritabanı ile Azure ya da başka bir ortamda sanal makinede çalışan SQL Server arasında veri eşitlemek için de kullanılabilir.

Eşitleme iki yönlü olabilir ve tam olarak hangi verilerin eşitleneceğini ve bunun ne kadar sıklıkla yapılacağını siz belirlersiniz. (Veritabanları arasında eşitleme atomik değildir, bununla birlikte her zaman en azından küçük bir gecikme olur.) Ayrıca, nasıl kullanılırsa kullanılsın, SQL Veri Eşitleme ile eşitlemeyi ayarlamak tamamen yapılandırma yönlendirmelidir; yazılacak bir kod yoktur.


### <a name="datarpt"></a>Sanal Makineleri Kullanarak SQL Veri Raporlama

Veritabanında veri olduğunda muhtemelen birileri bu verileri kullanarak rapor oluşturmak isteyecektir. Azure, Azure Sanal Makineleri'nde SQL Server Raporlama Servisleri'ni (SSRS) çalıştırabilir. Bu, şirket içinde SQL Server Raporlama Servisleri'ni çalıştırmaya eşdeğer bir işlevselliktir. Daha sonra Azure SQL Veritabanı'nda depolanan veriler üzerinde raporları çalıştırmak için SSRS kullanabilirsiniz.  [Şekil 5](#Fig5)'te sürecin nasıl işlediği gösterilmektedir.

<a name="Fig5"></a>![Diagram of SQL reporting][SQL-report]
 
**Şekil 5: Azure Sanal Makineleri'nde çalışan SQL Server Raporlama Servisleri SQL Veritabanı'ndaki veriler için raporlama hizmetleri sağlar. .**

Kullanıcının bir raporu görebilmesi için önce birileri bu raporun nasıl görünmesi gerektiğini tanımlar (1. adım). Sanal makinede SSRS ile bu işlem, iki araçtan biri kullanılarak yapılabilir: SQL Server 2012'nin parçası olan SQL Server Veri Araçları veya bunun öncülü Business Intelligence (BI) Development Studio. SSRS'de olduğu gibi, bu rapor tanımları Rapor Tanımlama Dilinde (RDL) ifade edilir. Bir rapora ilişkin RDL dosyaları oluşturulduktan sonra bu dosyalar buluttaki bir sanal makineye yüklenir (2. adım). Rapor tanımı artık kullanılmaya hazırdır.

Daha sonra, uygulamanın bir kullanıcısı rapora erişim sağlar (3. adım). Uygulama bu isteği SSRS sanal makinesine iletir (4. adım) ve sanal makine de gerek duyduğu verileri almak üzere SQL Veritabanı veya diğer veri kaynakları ile bağlantı kurar (5. adım). SSRS bu verileri ve ilgili RDL dosyalarını kullanarak raporu işler (6. adım); raporu uygulamaya iade eder (7. adım) ve uygulama da bu raporu kullanıcıya gösterir (8. adım).

Bir raporu uygulamaya ekleme (senaryosu burada gösterilen) tek seçenek değildir. Raporları sanal makinedeki Rapor Yöneticisi'nde, sanal makinedeki SharePoint'te veya diğer yollarla görüntülemek de mümkündür. Raporlar ayrıca, bir rapor diğerinin bağlantısını içerecek şekilde birleştirilebilir.

Azure sanal makinesinde SSRS, buluttaki bir raporlama çözümü olarak size tam işlevsellik sağlar. Raporlar, SSRS tarafından desteklenen her veri kaynağını kullanabilir. Uygulamalar ve raporlar, özel davranışları destekleyecek katıştırılmış kod veya derlemeleri içerebilir. Rapor sunucusu içeriği ve altyapısı aynı sanal sunucuda birlikte çalıştığından rapor yürütme ve işleme hızlıdır.



## <a name="tblstor"></a>Tablo Depolama

İlişkisel veriler birçok durumda yararlıdır, ancak her zaman en doğru seçenek değildir. Örneğin, uygulamanızın çok büyük miktarda gevşek yapılandırılmış veriye hızlı ve basit bir yoldan erişimi gerekiyorsa, ilişkisel veritabanı çok işe yaramayabilir. NoSQL teknolojisi muhtemelen daha iyi bir seçenektir.

Azure Tablo Depolama bu tür bir NoSQL yaklaşımına örnektir. Adına rağmen, Tablo Depolama standart ilişkisel tabloları desteklemez. Bunun yerine, *anahtar/değer deposu* olarak bilinen özelliği sağlar ve bir veri kümesini belirli bir anahtar ile ilişkilendirip, uygulamanın ilgili anahtarı sağlamak suretiyle bu verilere erişimine izin verir. [Şekil 6](#Fig6)'da temel yapı gösterilmektedir.

<a name="Fig6"></a>![Diagram of table storage][SQL-tblstor]
 
**Şekil 6: Azure Tablo Depolama, büyük miktarda verilere hızlı ve basit erişim olanağı sağlayan bir anahtar/değer deposudur.**

Blob'larda olduğu gibi her tablo bir Azure depolama hesabıyla ilişkilidir. Tablolar yine blob'lara çok benzer şekilde adlandırılır ve şu biçimde bir URL vardır:

http://<*StorageAccount*>.table.core.windows.net/<*TableName*>

Şekilde gösterildiği gibi, her tablo belirli sayıda bölüme ayrılır ve bu bölümlerin de her biri ayrı bir makinede depolanabilir. (Bu, SQL Federasyon'da olduğu gibi bir paylaşım biçimidir.) Hem Azure uygulamaları hem de başka herhangi bir ortamda çalışan uygulamalar, RESTful OData protokolünü ya da Azure Depolama İstemci kitaplığını kullanarak bir tabloya erişebilir.

Tablodaki her bölme belirli sayıda *varlık* barındırır ve bunların her biri de en çok 255 *özellik* içerir. Her özelliğin bir adı, bir türü (Binary, Bool, DateTime, Int veya String) ve bir değeri vardır. İlişkisel depolamadan farklı olarak bu tablolarda sabit şema yoktur ve bu nedenle, aynı tablodaki farklı varlıklar farklı türlerde özellikler içerebilir. Örneğin, bir varlığın sadece ad içeren Dize özelliği olabilirken, aynı tablodaki bir diğer varlığın müşteri kimlik numarası ve kredi derecelendirmesini içeren iki Tamsayı özelliği olabilir.

Uygulama, tablo içinde belirli bir varlığı tanımlamak için bu varlığın anahtarını sağlar. Anahtar iki bölümden oluşur: Belirli bir bölümü tanımlayan *bölüm anahtarı* ve bu bölüm dahilinde bir varlığı tanımlayan *satır anahtarı*. Örneğin, [Şekil 6](#Fig6)'da istemci bölüm anahtarı A ve satır anahtarı 3 olan varlığı talep etmekte ve Tablo Depolama işlevi de içerdiği tüm özelliklerle birlikte bu varlığı döndürmektedir.

Bu yapı tablonun büyük olmasına imkan tanır (tek bir tablo 100 terabayta kadar veri içerebilir) ve tablonun içerdiği verilere hızlı erişim olanağı sağlar. Bununla birlikte bazı sınırlamaları da getirir. Örneğin, tabloları ve hatta tek bir tablodaki bölümleri kapsayan işlemsel güncelleme desteği yoktur. Tabloya yönelik bir grup güncelleme ancak ilgili varlıkların tümünün aynı bölümde olması koşuluyla atomik bir işlem halinde gruplandırılabilir. Özelliklerinin değerine göre tabloyu sorgulamanın hiçbir yolu olmadığı gibi, birden fazla tablo genelinde birleştirmeler için destek de mevcut değildir. Ayrıca, ilişkisel veritabanlarından farklı olarak tablolar depolanan prosedürler için destek sağlamaz.

Azure Tablo Depolama, büyük miktarlarda gevşek yapılandırılmış veriye hızlı ve ucuz erişim gerektiren uygulamalar için iyi bir seçenektir. Örneğin, çok sayıda kullanıcının profil bilgilerini depolayan bir İnternet uygulaması tabloları kullanabilir. Bu durumda hızlı erişim önemlidir ve büyük bir olasılıkla uygulama SQL'nin tam gücüne ihtiyaç duymaz. Hız ve boyut kazanmak için bu işlevsellikten vazgeçmek bazen mantıklı olabilir ve dolayısıyla bazı sorunlar için en doğru çözüm Tablo Depolama olur.


## <a name="hadoop"></a>Hadoop

Kuruluşlar onlarca yıldır veri ambarları oluşturmaktadır. Çoğu zaman ilişkisel tablolarda depolanan bu bilgi koleksiyonları kişilerin birlikte çalışabilmesini ve verilerden çok farklı şekillerde bilgi edinebilmesini sağlar. Örneğin, SQL Server ile bunu yapmak için SQL Server Analysis Services gibi araçların kullanılması yaygın bir uygulamadır.

Ancak ilişkisel olmayan veriler üzerinde çözümleme yapmak istediğinizi varsayalım. Verileriniz birçok biçime bürünebilir: Sensörlerden veya RFID etiketlerinden gelen bilgiler, sunucu gruplarındaki günlük dosyaları, web uygulamalarının oluşturduğu tıklama dizisi verileri, tıbbi tanı cihazlarından gelen görüntüler ve daha fazlası. Bu veriler aynı zamanda gerçekten büyük olabilir ve hatta geleneksel bir veri ambarında etkinlikle kullanılamayacak kadar büyük olabilir. Sadece birkaç yıl önce nadir görülen bu tür büyük veri sorunları artık oldukça yaygınlaşmıştır.

Bu tür büyük verileri çözümlemek için endüstrimiz büyük ölçüde tek bir çözümde birleşmiştir: Hadoop açık kaynak teknolojisi. Hadoop fiziksel ve sanal makinelerden oluşan bir kümede çalışır ve üzerinde çalıştığı verileri bu makinelere yayarak paralel bir biçimde işler. Hadoop'un kullanması gereken makinelerin sayısı arttıkça yaptığı işi daha hızlı tamamlayabilir.

Böyle bir sorun genel buluta doğal olarak uygundur. Zamanın büyük bir bölümünde boşta bekleyecek şirket içi sunuculardan oluşan bir ordu tutmak yerine bulut ortamında Hadoop'u çalıştırmak, sanal makineleri yalnızca ihtiyaç duyduğunuzda oluşturmanıza (ve ödeme yapmanıza) imkan tanır. Daha da iyisi, Hadoop ile çözümlemek istediğiniz büyük verilerin giderek daha fazlası bulutta oluşturulmakta ve bu da verileri dolaştırma zahmetinden sizi kurtarmaktadır. Microsoft, bu sinerjilerden istifade etmenize yardımcı olmak için Azure'da bir Hadoop hizmeti sağlamaktadır. [Şekil 7](#Fig7)'de bu hizmetin en önemli bileşenleri gösterilmektedir.

<a name="Fig7"></a>![Diagram of hadoop][hadoop]

**Şekil 7: Azure'daki Hadoop, birden fazla sanal makinenin kullanımıyla verileri paralel olarak işleyen MapReduce işlerini çalıştırır.**

Azure'da Hadoop'u kullanmak için önce bu bulut platformundan bir Hadoop kümesi oluşturmasını isteyin ve ihtiyaç duyduğunuz sanal makine sayısını belirtin. Kendi başınıza bir Hadoop kümesi oluşturmak basit olmayan bir görev olduğundan, bunu sizin için Azure'un yapmasına izin vermek akıllıca olur. Kümeyi kullanmayı bitirdiğinizde kapatırsınız. Kullanmadığınız hesaplama kaynakları için ödeme yapmaya gerek yoktur.

Genelde *iş* adıyla anılan Hadoop uygulaması *MapReduce* olarak bilenen bir programlama modeli kullanır. Şekilde gösterildiği gibi, MapReduce iş mantığı birçok sanal makine genelinde eşzamanlı çalışır. Hadoop verileri paralel işlemek suretiyle tek makineli çözümlerden çok daha hızlı veri çözümleyebilir.

Azure'da, bir MapReduce işinin üzerinde çalıştığı veriler tipik olarak blob depolamada tutulur. Ancak Hadoop'ta, MapReduce işleri verilerin *Hadoop Dağıtılmış Dosya Sistemi'nde (HDFS)* depolanmasını bekler. HDFS bazı yönleriyle Blob Depolamaya benzer; örneğin, verileri birden fazla fiziksel sunucu genelinde çoğaltır. Azure'daki Hadoop bu işlevselliği çoğaltmak yerine, Blob Depolamayı şekilde gösterildiği gibi HDFS API aracılığıyla açığa çıkarır. MapReduce işindeki mantık sıradan HDFS dosyalarına erişim sağladığını düşünürken, yapılan iş aslında blob'lardan kendisine akışı sağlanan verilerle çalışmaktır. Ayrıca, aynı veriler üzerinde birden fazla işin çalıştırıldığı durumları desteklemesi için, Azure'daki Hadoop blob'lardan sanal makinelerde çalışan tam HDFS'ye veri kopyalamaya da olanak sağlar. 

MapReduce işleri bugün genellikle Java ile yazılmaktadır ve bu Azure'daki Hadoop'u destekleyen bir yaklaşımdır. Microsoft, MapReduce işlerini C#, F# ve JavaScript'i içeren diğer dillerde oluşturma desteği de eklemiştir. Buradaki amaç, bu büyük veri teknolojisine daha büyük bir geliştirici grubunun erişebilmesini sağlamaktır.

HDFS ve MapReduce'un yanı sıra, Hadoop kişilerin kendileri bir MapReduce işi yazmadan verileri çözümlemesine izin veren diğer teknolojileri kapsar. Örneğin, Pig büyük verileri çözümlemek üzere tasarlanmış üst düzey bir dil olurken, Hive de HiveQL adı verilen SQL benzeri bir dil sunar. Hem Pig hem de Hive aslında HDFS verilerini işleyen MapReduce işlerini üretir, ancak bu karmaşıklığı kullanıcılara göstermezler. 
Azure'da Hadoop ile her ikisi de sağlanmaktadır.

Microsoft, Excel'e yönelik bir HiveQL sürücüsü de sağlar. İş analistleri bir Excel eklentisi kullanarak HiveQL sorgularını (ve dolayısıyla MapReduce işlerini) doğrudan Excel'den oluşturabilir, sonra da PowerPivot ve diğer Excel araçlarını kullanarak sonuçları işleyebilir ve görselleştirebilirler. Azure'daki Hadoop makine öğrenme kitaplıkları Mahout, grafik çıkarma sistemi Pegasus ve daha birçoğu gibi diğer teknolojileri de içerir.

Büyük verilerin çözümlemesi önemlidir ve dolayısıyla Hadoop da önemlidir. Microsoft, Azure'da yönetilen bir hizmet olarak Hadoop'u ve beraberinde Excel gibi bilindik araçların bağlantılarını sağlayarak bu teknolojiyi daha geniş bir kullanıcı grubunun erişimine sunmayı amaçlamaktadır.

Daha geniş bir kapsamda, her türden veri önemlidir. Azure'un veri yönetimi ve iş analitiği için bir seçenek yelpazesine sahip olmasının nedeni budur. Oluşturmaya çalıştığınız uygulama ne olursa olsun, bu bulut platformunda işinize yarayacak bir şeyler bulmanız muhtemeldir.

[blobs]: ./media/cloud-storage/Data_01_Blobs.png
[SQLSvr-vm]: ./media/cloud-storage/Data_02_SQLSvrVM.png
[SQL-db]: ./media/cloud-storage/Data_03_SQLdb.png
[SQL-datasync]: ./media/cloud-storage/Data_04_SQLDataSync.png
[SQL-report]: ./media/cloud-storage/Data_05_SQLReporting.png
[SQL-tblstor]: ./media/cloud-storage/Data_06_TblStorage.png
[hadoop]: ./media/cloud-storage/Data_07_Hadoop.png
