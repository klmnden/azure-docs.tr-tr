<properties
    pageTitle="Esnetme Veritabanı'na genel bakış | Microsoft Azure"
    description="Esnetme Veritabanı'nın soğuk verilerinizi şeffaf ve güvenli bir şekilde Microsoft Azure bulutuna nasıl geçirdiğini öğrenin."
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
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# Esnetme Veritabanı'na genel bakış

Esnetme Veritabanı soğuk verilerinizi şeffaf ve güvenli bir şekilde Microsoft Azure bulutuna geçirir.

Yalnızca Esnetme Veritabanını hemen kullanmaya başlamak istiyorsanız bkz. [Esnetme İçin Veritabanını Etkinleştirme Sihirbazını çalıştırarak kullanmaya başlama](sql-server-stretch-database-wizard.md).

## Esnetme Veritabanı'nın yararları nelerdir?
Esnetme Veritabanı aşağıdaki faydaları sağlar:

### Soğuk veriler için uygun\-maliyetli kullanılabilirlik sağlar
SQL Server Esnetme Veritabanı ile sıcak ve soğuk işlem verilerini SQL Server'dan Microsoft Azure'a dinamik olarak esnetin. Genel soğuk veri depolamadan farklı olarak, verileriniz her zaman çevrimiçidir ve sorgulama için kullanılabilir. Müşteri Siparişi Geçmişi gibi büyük tablolar için aşırı masraf yapmadan daha uzun süreli veri bekletme zaman çizelgeleri sağlayabilirsiniz. Pahalı şirket içi depolama için ölçeklendirmek yerine, Azure'ın düşük maliyetinden yararlanın. Fiyatlandırma katmanınızı seçin ve Azure Portal'da ayarları yapılandırarak maliyetler üzerinde kontrolünüzü koruyun. İhtiyaca göre ölçeği artırın veya azaltın. Ayrıntılı bilgi için [SQL Server Esnetme Veritabanı Fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/) sayfasını ziyaret edin.

### Sorgu veya uygulamalarda değişiklik yapılmasını gerektirmez
Şirket\-içi veya buluta esnetilmiş olmasından bağımsız olarak, SQL Server verilerinize sorunsuz şekilde erişin.  Verilerin nerede depolanacağını belirleyen ilkeyi ayarladığınızda, SQL Server arka planda veri hareketlerini işler. Tüm tablo her zaman için çevrimiçi ve sorgulanabilirdir. Aynı zamanda Esnetme Veritabanı var olan sorgularda veya uygulamalarda herhangi bir değişiklik gerektirmez, verilerin konumu uygulama için tamamen saydamdır.

### Şirket\-içi veri bakımını modern hale getirir
Verileriniz için şirket\-içi bakım ve depolama ihtiyacını azaltır. Şirket içi verilerinizin yedekleri bakım süresi içinde daha hızlı çalışır ve tamamlanır. Verilerinizin bulut bölümünün yedekleri otomatik olarak çalışır. Şirket içi depolama gereksinimleriniz önemli ölçüde azalır. Azure depolama, şirket içi SSD'ye ekleme yapmaya göre %80 daha ucuz olabilir.

### Geçiş sırasında bile verilerinizi güvende tutar
En önemli uygulamalarınızı güvenli bir şekilde buluta esnetirken içiniz rahat olsun. SQL Server'ın Her Zaman Şifreli özelliği, hareket halindeki verileriniz için şifreleme sağlar. Satır Düzeyi Güvenlik (RLS) ve diğer gelişmiş SQL Server güvenlik özellikleri, verilerinizi korumak için Esnetme Veritabanı ile de çalışır.

## Esnetme Veritabanı ne yapar?
Esnetme Veritabanı'nı bir SQL Server örneği, bir veritabanı ve en az bir tablo için etkinleştirdikten sonra Esnetme Veritabanı soğuk verilerinizi sessiz bir şekilde Azure'a geçirmeye başlar.

-   Soğuk verileri ayrı bir tabloda saklarsanız tüm tabloyu geçirebilirsiniz.

-   Tablonuz hem sıcak hem de soğuk verileri içeriyorsa geçirilecek satırları seçmek için bir filtre işlevi belirtebilirsiniz.

**Var olan sorguları ve istemci uygulamalarını değiştirmeniz gerekmez.** Veri geçişi sırasında bile hem yerel hem de uzak verilere sorunsuz şekilde erişim sağlamaya devam edersiniz. Uzak sorgular için kısa süreli gecikme gerçekleşir ancak bu gecikmeyle yalnızca soğuk verileri sorguladığınızda karşılaşırsınız.

**Esnetme Veritabanı**, geçiş sırasında bir hata oluşması durumunda hiçbir verinin kaybedilmemesini sağlar. Aynı zamanda geçiş sırasında oluşabilecek bağlantı sorunlarını gidermek için yeniden deneme mantığına sahiptir. Dinamik bir yönetim görünümü, geçişin durumuyla ilgili bilgi sağlar.

Yerel sunucudaki sorunları gidermek veya kullanılabilir ağ bant genişliğini en üst düzeye çıkarmak için **veri geçişini duraklatabilirsiniz**.

![Esnetme veritabanına genel bakış][StretchOverviewImage1]

## Esnetme Veritabanı sizin için uygun mu?
Aşağıdaki ifadeler sizin için geçerliyse Esnetme Veritabanı gereksinimlerinizi karşılamanıza ve sorunlarınızı çözmenize yardımcı olabilir.

|Karar verme yetkisine sahipseniz|DBA iseniz|
|------------------------------|-------------------|
|İşlem verilerini uzun süreyle tutmam gerekiyor.|Tablolarımın boyutu kontrolden çıkıyor.|
|Bazen soğuk verileri sorgulamam gerekiyor.|Kullanıcılarım soğuk verilere erişmek istediklerini söylüyor ancak bunları nadiren kullanıyorlar.|
|Eski uygulamalar dahil olmak üzere, güncelleştirmek istemediğim uygulamalarım var.|Sürekli daha fazla depolama alanı almam ve eklemem gerekiyor.|
|Depolama alanında paradan tasarruf etmem gerekiyor.|SLA'da bu kadar büyük tabloları yedekleyemiyor veya geri yükleyemiyorum.|

## Ne tür veritabanları ve tablolar Esnetme Veritabanı için aday niteliği taşır?
Esnetme Veritabanı, genellikle az sayıda tablo içinde saklanan ve çok miktarda soğuk veri içeren işlem veritabanlarını hedefler. Bu tablolar bir milyardan fazla satır içerebilir.

SQL Server 2016'nın zamana bağlı tablo özelliğini kullanırsanız ilgili geçmiş tablosunun tamamını veya bir kısmını Azure'ın uygun maliyetli depolama alanına geçirmek için Esnetme Veritabanı'nı kullanın. Daha fazla bilgi için bkz. [Sistem Sürümü Tutulan Zamana Bağlı Tablolarda Geçmiş Verilerin Bekletilmesini Yönetme](https://msdn.microsoft.com/library/mt637341.aspx).

Esnetme Veritabanı için veritabanlarını ve tabloları tanımlamak üzere SQL Server 2016 Yükseltme Danışmanı'nın bir özelliği olan Esnetme Veritabanı Danışmanı'nı kullanın. Daha fazla bilgi için bkz. [Esnetme Veritabanı için veritabanlarını ve tabloları tanımlama](sql-server-stretch-database-identify-databases.md). Olası engelleme sorunları hakkında daha fazla bilgi edinmek için bkz. [Esnetme Veritabanı Sınırlamaları](sql-server-stretch-database-limitations.md).

## Esnetme Veritabanı'nda test kullanımı
**Esnetme Veritabanı'nda AdventureWorks örnek veritabanı ile test kullanımı yapın.** AdventureWorks örnek veritabanını almak için en azından veritabanı dosyasını ve örnekler ile betikler dosyasını [buradan](https://www.microsoft.com/download/details.aspx?id=49502) indirin. Örnek veritabanını bir SQL Server 2016 örneğine geri yükledikten sonra, örnekler dosyasının sıkıştırmasını açın ve Stretch DB klasöründen Stretch DB Örnekleri dosyasını açın. Esnetme Veritabanı'nı etkinleştirmeden önce ve etkinleştirdikten sonra verilerinizin kullandığı alanı denetlemek, veri geçişinin ilerleme durumunu izlemek, ayrıca hem veri geçişi sırasında hem de sonrasında var olan verileri sorgulayabildiğinizi ve yeni veriler ekleyebildiğinizi onaylamak için bu dosyadaki betikleri çalıştırın.

## Sonraki adım
**Esnetme Veritabanı için aday niteliği taşıyan veritabanlarını ve tabloları tanımlayın.** Esnetme Veritabanı için aday niteliği taşıyan veritabanlarını ve tabloları tanımlamak üzere SQL Server 2016 Yükseltme Danışmanı'nı indirin ve Esnetme Veritabanı Danışmanı'nı çalıştırın. Esnetme Veritabanı Danışmanı engelleme sorunlarını da tanımlar. Daha fazla bilgi için bkz. [Esnetme Veritabanı için veritabanlarını ve tabloları tanımlama](sql-server-stretch-database-identify-databases.md).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png



<!--HONumber=Aug16_HO1-->


