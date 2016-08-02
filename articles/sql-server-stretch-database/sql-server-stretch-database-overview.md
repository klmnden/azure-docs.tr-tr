<properties
    pageTitle="Esnetme Veritabanı'na genel bakış | Microsoft Azure"
    description="Esnetme Veritabanı'nın geçmiş verilerinizi saydam ve güvenli bir şekilde Microsoft Azure bulutuna nasıl geçirdiğini öğrenin."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager=""
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/17/2016"
    ms.author="douglasl"/>

# Esnetme Veritabanı'na genel bakış

Esnetme Veritabanı, geçmiş verilerinizi Microsoft Azure bulutuna saydam ve güvenli bir şekilde taşır.

Yalnızca Esnetme Veritabanını hemen kullanmaya başlamak istiyorsanız bkz. [Esnetme İçin Veritabanını Etkinleştirme Sihirbazını çalıştırarak kullanmaya başlama](sql-server-stretch-database-wizard.md).

## Esnetme Veritabanı'nın yararları nelerdir?
Esnetme Veritabanı aşağıdaki faydaları sağlar:

**Soğuk veriler için uygun maliyetli kullanılabilirlik sunar** SQL Server Esnetme Veritabanı ile sıcak ve soğuk işlem verilerini SQL Server'dan Microsoft Azure'a dinamik olarak esnetin. Genel soğuk veri depolamadan farklı olarak, verileriniz her zaman çevrimiçidir ve sorgulama için kullanılabilir. Müşteri Siparişi Geçmişi gibi büyük tablolar için aşırı masraf yapmadan daha uzun süreli veri bekletme zaman çizelgeleri sağlayabilirsiniz. Pahalı şirket içi depolama için ölçeklendirmek yerine, Azure'ın düşük maliyetinden yararlanın. Fiyatlandırma katmanınızı seçin ve Azure Portal'da ayarları yapılandırarak maliyetler üzerinde kontrolünüzü koruyun. İhtiyaca göre ölçeği artırın veya azaltın. Ayrıntılı bilgi için [SQL Server Esnetme Veritabanı Fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) sayfasını ziyaret edin.

**Sorgular veya uygulamalar için değişiklik gerektirmez** Şirket içi veya buluta esnetilmiş olmasından bağımsız olarak, SQL Server verilerinize sorunsuz şekilde erişin.  Verilerin nerede depolanacağını belirleyen ilkeyi ayarladığınızda, SQL Server arka planda veri hareketlerini işler. Tüm tablo her zaman için çevrimiçi ve sorgulanabilirdir. Aynı zamanda Esnetme Veritabanı var olan sorgularda veya uygulamalarda herhangi bir değişiklik gerektirmez, verilerin konumu uygulama için tamamen saydamdır.

**Şirket içi veri bakımını kolaylaştırır** Verileriniz için şirket içi bakım ve depolamayı azaltın. Verilerinizin bulut bölümünün yedekleri otomatik olarak çalışır. Şirket içi verilerinizin yedekleri bakım süresi içinde daha hızlı çalışır ve tamamlanır. Şirket içi depolama gereksinimleriniz önemli ölçüde azalır. Azure depolama, şirket içi SSD'ye ekleme yapmaya göre %80 daha ucuz olabilir.

**Geçiş sırasında bile verilerinizin güvenliğini sağlar** En önemli uygulamalarınızı güvenli bir şekilde buluta esnetirken içiniz rahat olsun. SQL Server'ın Her Zaman Şifreli özelliği, hareket halindeki verileriniz için şifreleme sağlar. Satır Düzeyi Güvenlik (RLS) ve diğer gelişmiş SQL Server güvenlik özellikleri, verilerinizi korumak için Esnetme Veritabanı ile de çalışır.

## Esnetme Veritabanı ne yapar?
Esnetme Veritabanı'nı bir SQL Server örneği, bir veritabanı ve en az bir tablo için etkinleştirdikten sonra, geçmiş verileriniz sessiz bir şekilde Azure'a geçirilmeye başlanır.

-   Geçmiş verileri ayrı bir tabloda saklarsanız tüm tabloyu geçirebilirsiniz.

-   Tablonuz hem geçmiş hem de geçerli verileri içeriyorsa geçirilecek satırları seçmek için bir filtre koşulu belirtebilirsiniz.

Esnetme Veritabanı, geçiş sırasında bir hata oluşması durumunda hiçbir verinin kaybedilmemesini sağlar. Aynı zamanda geçiş sırasında oluşabilecek bağlantı sorunlarını gidermek için yeniden deneme mantığına sahiptir. Dinamik bir yönetim görünümü, geçişin durumuyla ilgili bilgi sağlar.

Yerel sunucudaki sorunları gidermek veya kullanılabilir ağ bant genişliğini en üst düzeye çıkarmak için veri geçişini duraklatabilirsiniz.

Var olan sorguları ve istemci uygulamalarını değiştirmeniz gerekmez. Veri geçişi sırasında bile hem yerel hem de uzak verilere sorunsuz şekilde erişim sağlamaya devam edersiniz. Uzak sorgular için kısa süreli gecikme gerçekleşir ancak bu gecikmeyle yalnızca geçmiş verileri sorguladığınızda karşılaşırsınız.

![Esnetme veritabanına genel bakış][StretchOverviewImage1]

## Esnetme Veritabanı sizin için uygun mu?
Aşağıdaki ifadeler sizin için geçerliyse Esnetme Veritabanı gereksinimlerinizi karşılamanıza ve sorunlarınızı çözmenize yardımcı olabilir.

|Karar verme yetkisine sahipseniz|DBA iseniz|
|------------------------------|-------------------|
|İşlem verilerini uzun süreyle tutmam gerekiyor.|Tablolarımın boyutu kontrolden çıkıyor.|
|Bazen geçmiş verileri sorgulamam gerekiyor.|Kullanıcılarım geçmiş verilere erişmek istediklerini söylüyor ancak bunları nadiren kullanıyorlar.|
|Eski uygulamalar dahil olmak üzere, güncelleştirmek istemediğim uygulamalarım var.|Sürekli daha fazla depolama alanı almam ve eklemem gerekiyor.|
|Depolama alanında paradan tasarruf etmem gerekiyor.|SLA'da bu kadar büyük tabloları yedekleyemiyor veya geri yükleyemiyorum.|

## Ne tür veritabanları ve tablolar Esnetme Veritabanı için aday niteliği taşır?
Esnetme Veritabanı, genellikle az sayıda tablo içinde saklanan ve çok miktarda geçmiş veri içeren işlem veritabanlarını hedefler. Bu tablolar bir milyardan fazla satır içerebilir.

SQL Server 2016'nın zamana bağlı tablo özelliğini kullanırsanız ilgili geçmiş tablosunun tamamını veya bir kısmını Azure'ın uygun maliyetli depolama alanına geçirmek için Esnetme Veritabanı'nı kullanın. Daha fazla bilgi için bkz. [Sistem Sürümü Tutulan Zamana Bağlı Tablolarda Geçmiş Verilerin Bekletilmesini Yönetme](https://msdn.microsoft.com/library/mt637341.aspx).

Esnetme Veritabanı için veritabanlarını ve tabloları tanımlamak üzere SQL Server 2016 Yükseltme Danışmanı'nın bir özelliği olan Esnetme Veritabanı Danışmanı'nı kullanın. Daha fazla bilgi için bkz. [Esnetme Veritabanı için veritabanlarını ve tabloları tanımlama](sql-server-stretch-database-identify-databases.md). Olası engelleme sorunları hakkında daha fazla bilgi edinmek için bkz. [Esnetme Veritabanı için yüzey alanı sınırlamaları ve engelleme sorunları](sql-server-stretch-database-limitations.md).

## <a name="FAQ"></a>Esnetme Veritabanı hakkında sık sorulan sorular
**Esnetme Veritabanı &lt;SQL Server özelliği adı&gt; ile çalışır mı?**
-   Bir tablonun Esnetme için uygun olmamasına neden olan SQL Server özelliklerinin bir listesi için bkz. [Esnetme Veritabanı için yüzey alanı sınırlamaları ve engelleme sorunları](sql-server-stretch-database-limitations.md).

-   İsteğe bağlı olarak, Esnetme Veritabanı için aday niteliği taşıyan veritabanlarını ve tabloları tanımlamak üzere SQL Server 2016 Yükseltme Danışmanı'nı indirin ve Esnetme Veritabanı Danışmanı'nı çalıştırın. Daha fazla bilgi için bkz. [Esnetme Veritabanı için veritabanlarını ve tabloları tanımlama](sql-server-stretch-database-identify-databases.md).

**Esnetme Veritabanı için başka bir yerel SQL Server örneğini hedefleyebilir miyim?**
Hayır. Esnetme Veritabanı uzak uç nokta olarak başka bir yerel SQL Server örneğini desteklemez.

**Esnetmeyi devre dışı bırakıp geçirilen verileri yerel tabloya geri taşıyabilir miyim?**
Evet. Daha fazla bilgi için bkz. [Esnetme Veritabanı'nı devre dışı bırakma ve uzak verileri geri getirme](sql-server-stretch-database-disable.md).

## Koşullar
**Yerel veritabanı**. Şirket içi SQL Server veritabanı.

**Uzak uç nokta**. Veritabanının uzak verilerini içeren Microsoft Azure içindeki konum.

**Yerel veriler**. Veritabanındaki tabloların Esnetme Veritabanı yapılandırması doğrultusunda Azure'a taşınmayacak olan, Esnetme Veritabanı etkinleştirilmiş veritabanı içindeki veriler.

**Uygun veriler**. Veritabanındaki tabloların Esnetme Veritabanı yapılandırması doğrultusunda Azure'a henüz taşınmamış ancak taşınacak olan, Esnetme Veritabanı etkinleştirilmiş veritabanı içindeki veriler.

**Uzak veriler**. Azure'a zaten taşınmış olan, Esnetme Veritabanı etkinleştirilmiş veritabanı içindeki veriler.

## Mimari
Esnetme Veritabanı, arşiv verileri depolama alanını ve sorgu işlemeyi boşaltmak için Microsoft Azure kaynaklarından yararlanır.

Bir veritabanında Esnetme Veritabanı'nı etkinleştirdiğinizde, Esnetme Veritabanı şirket içi SQL sunucusunda güvenli bağlantılı bir sunucu tanımı oluşturur. Bu bağlantılı sunucu tanımının hedefi uzak uç noktadır. Veritabanındaki bir tabloda Esnetme Veritabanı'nı etkinleştirdiğinizde uzak kaynaklar sağlar ve geçiş etkinleştirilmişse uygun verileri geçirmeye başlar.

Esnetme Veritabanı etkinleştirilmiş tablolarda yürütülen sorgular otomatik olarak hem yerel veritabanında hem de uzak uç noktada çalıştırılır. Esnetme Veritabanı, sorguları yeniden yazarak uzak verilerde sorguları çalıştırmak için Azure'ın işleme gücünü kullanır. Bu yeniden yazma işlemi, yeni sorgu planında "uzak sorgu" işleci olarak görünür.

![Esnetme Veritabanı mimarisi][StretchOverviewImage2]

## Güvenlik ve izinler

### Esnetme Veritabanı'nı bir SQL Server örneği için etkinleştirme ve devre dışı bırakma
Esnetme Veritabanı için veritabanlarını yapılandırmaya başlarken, öncelikle sp\_configure kullanarak "uzak veri arşivi" örnek düzeyi yapılandırma seçeneğini değşitirmeniz gerekir. Bu işlem, sysadmin veya serveradmin ayrıcalıklarını gerektirir. Bu seçenek etkinleştirildiğinde, uzak uç noktada verileri sorgulayabilir, verileri geçirebilir ve Esnetme Veritabanı için veritabanlarını yapılandırabilirsiniz.

### Esnetme Veritabanı'nı bir veritabanı veya tablo için etkinleştirme ya da devre dışı bırakma
Bir veritabanını Esnetme Veritabanı için yapılandırırken CONTROL DATABASE izninizin olması gerekir. Ayrıca uzak uç nokta için yönetici kimlik bilgilerini sağlamanız gerekir.

Bir tabloyu Esnetme Veritabanı için yapılandırırken, tabloda ALTER ayrıcalığınızın olması ve veritabanının Esnetme Veritabanı için zaten yapılandırılmış olması gereklidir.

### Veri erişimi
Yalnızca sistem işlemleri Esnetme Veritabanı'nın arkasındaki bağlantılı sunucu tanımına erişebilir. Kullanıcı oturumları, bağlantılı sunucu tanımı yoluyla uzak uç noktaya sorgu yürütemez.

Esnetme Veritabanı var olan bir veritabanının izinler modelini değiştirmez. Kullanıcı oturumları, yerel veritabanı yoluyla Esnetme Veritabanı etkinleştirilmiş bir tablodaki verileri sorgulayabilir. Yerel veritabanı, kullanıcı tarafından başlatılan tüm eylemler için diğer tüm nesnelerle aynı yolla izin denetimleri gerçekleştirir. Esnetme Veritabanı etkinleştirilmiş tabloya erişim yetkiniz varsa verilerin fiziksel konumundan bağımsız olarak, tablonun yetkiye sahip olduğunuz tüm içeriklerine erişebilirsiniz.

![Esnetme Veritabanı veri erişim modeli][StretchOverviewImage3]

## Esnetme Veritabanı'nda test kullanımı
**Esnetme Veritabanı'nda AdventureWorks örnek veritabanı ile test kullanımı yapın.** AdventureWorks örnek veritabanını almak için en azından veritabanı dosyasını ve örnekler ile betikler dosyasını [buradan](https://www.microsoft.com/download/details.aspx?id=49502) indirin. Örnek veritabanını bir SQL Server 2016 örneğine geri yükledikten sonra, örnekler dosyasının sıkıştırmasını açın ve Stretch DB klasöründen Stretch DB Örnekleri dosyasını açın. Esnetme Veritabanı'nı etkinleştirmeden önce ve etkinleştirdikten sonra verilerinizin kullandığı alanı denetlemek, veri geçişinin ilerleme durumunu izlemek, ayrıca hem veri geçişi sırasında hem de sonrasında var olan verileri sorgulayabildiğinizi ve yeni veriler ekleyebildiğinizi onaylamak için bu dosyadaki betikleri çalıştırın.

## Sonraki adım
**Esnetme Veritabanı için aday niteliği taşıyan veritabanlarını ve tabloları tanımlayın.** Esnetme Veritabanı için aday niteliği taşıyan veritabanlarını ve tabloları tanımlamak üzere SQL Server 2016 Yükseltme Danışmanı'nı indirin ve Esnetme Veritabanı Danışmanı'nı çalıştırın. Esnetme Veritabanı Danışmanı engelleme sorunlarını da tanımlar. Daha fazla bilgi için bkz. [Esnetme Veritabanı için veritabanlarını ve tabloları tanımlama](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png



<!---HONumber=Jun16_HO2-->


