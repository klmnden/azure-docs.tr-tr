---
title: "Toplu işleme Azure SQL veritabanı uygulama performansını artırmak için nasıl kullanılacağını"
description: "Konu, toplu veritabanı işlemleri kanıt sağlar büyük ölçüde imroves hızını ve Azure SQL veritabanı uygulamalarınızın ölçeklenebilirlik. Bu toplu teknikler herhangi bir SQL Server veritabanı için çalışır makaleyi odağını Azure üzerinde olsa da."
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: article
ms.date: 07/12/2016
ms.author: sstein
ms.openlocfilehash: 5c7846fdd8d6a7584cab2b4f3811151332171ba4
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Toplu işleme SQL veritabanı uygulama performansını artırmak için nasıl kullanılacağını
Azure SQL veritabanı işlemleri önemli ölçüde toplu işleme, uygulamalarınızın ölçeklenebilirliğini ve performansı artırır. Avantajları anlamak için bir SQL veritabanına sıralı ve toplu istekleri karşılaştırmak bazı örnek test sonuçlarını ilk bölümü, bu makalenin ele alınmaktadır. Makalenin kalanında teknikleri, senaryoları ve başarıyla Azure uygulamalarınızda toplu işleme kullanmanıza yardımcı olması için dikkat edilecek noktalar gösterir.

## <a name="why-is-batching-important-for-sql-database"></a>SQL veritabanı için önemli toplu işleme neden?
Uzak Hizmet çağrıları toplu işleme, performans ve ölçeklenebilirliği artırmak için iyi bilinen bir stratejidir. Seri hale getirme, ağ aktarımı ve seri durumdan çıkarma gibi uzak bir hizmet ile herhangi bir etkileşimi işlem maliyetleri vardır düzeltilmiştir. Tek bir toplu birçok ayrı işlemlere paketleme bu maliyetleri en aza indirir.

Bu yazıda, stratejilerini ve senaryoları toplu işleme çeşitli SQL veritabanı incelemek istiyoruz. Bu stratejileri ayrıca SQL Server kullanan şirket içi uygulamaları için önemli olsa da, SQL veritabanı için toplu işleme kullanımını vurgulama birkaç nedeni vardır:

* Olup olmadığını potansiyel olarak büyük ağ gecikmesi SQL veritabanına erişim özellikle aynı Microsoft Azure veri merkezi dışından gelen SQL veritabanına erişme.
* SQL veritabanı çok müşterili özelliklerini anlamına gelir, veri erişim katmanı karşılık gelen veritabanı genel ölçeklenebilirlik için verimliliğini. SQL veritabanı, veritabanı kaynaklarına diğer kiracıların zarar tekeline tek bir Kiracı/Kullanıcı engellemeniz gerekir. SQL veritabanı için önceden tanımlanmış kotaları aşan kullanımına yanıt olarak üretilen iş azaltın veya özel durumlar azaltma ile yanıt. Toplu işleme gibi verimliliği, SQL veritabanı üzerinde daha fazla iş bu sınırları ulaşmadan önce olanak sağlar. 
* Toplu işleme ayrıca birden çok veritabanı (parçalama) kullanan mimari için geçerli değildir. Her veritabanı birimi ile etkileşim verimliliğini hala bir anahtar genel ölçeklenebilirlik içinde faktördür. 

SQL veritabanı kullanmanın yararları veritabanını barındıracak sunucuları yönetmek yok biridir. Ancak, bu yönetilen altyapı ayrıca farklı veritabanı iyileştirmeler hakkında düşünmek zorunda anlamına gelir. Artık veritabanı donanım veya ağ altyapısı artırmak için bakabilirsiniz. Microsoft Azure konusu ortamlara denetler. Denetleyebileceğiniz ana uygulamanızın SQL Database ile nasıl etkileşim kurduğu alanıdır. Toplu işleme, bu en iyi duruma getirme biridir. 

Kağıt ilk bölümü SQL veritabanını kullan .NET uygulamaları için çeşitli toplu teknikleri inceler. Son iki bölüm toplu kılavuzları ve senaryoları kapsar.

## <a name="batching-strategies"></a>Toplu işleme stratejileri
### <a name="note-about-timing-results-in-this-topic"></a>Bu konudaki zamanlama sonuçlarıyla ilgili unutmayın
> [!NOTE]
> Sonuçları kıyaslamaları değildir ancak göstermeye yöneliktir **göreli performans**. Zamanlamaları, en az 10 test çalışmaları ortalama üzerinde temel alır. Boş bir tablo ekler işlemleridir. Bu testler ölçülen öncesi V12 olan ve bunlar mutlaka kullanarak yeni bir V12 veritabanında karşılaşabileceğiniz işleme karşılık değil [hizmet katmanları](sql-database-service-tiers.md). Toplu işleme tekniği göreli yararı benzer olmalıdır.
> 
> 

### <a name="transactions"></a>İşlemler
Bir gözden geçirme işlemleri ele tarafından toplu işleme, başlamak için garip gibi görünüyor. Ancak istemci-tarafı işlemleri kullanımını performansını artıran zarif bir toplu sunucu tarafı etkisi olmaz. Ve sıralı işlemlerinin performansını artırmak için hızlı bir şekilde sağlamak için yalnızca birkaç kod satırıyla, işlemleri eklenebilir.

INSERT dizisini içeren aşağıdaki C# kod göz önünde bulundurun ve basit bir tablo üzerinde işlem güncelleştirin.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

Aşağıdaki ADO.NET kodu sırayla bu işlemleri gerçekleştirir.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

Bu kodu en iyi duruma getirmek için en iyi yolu, istemci-tarafı bu çağrıları toplu işleme biçimi uygulamaktır. Ancak yalnızca bir işlem içinde çağrısı sırası kaydırma tarafından bu kodu performansını artırmak için basit bir yolu yoktur. Aşağıda, bir işlem kullandığı aynı kodu verilmiştir.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();

        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }

        transaction.Commit();
    }

İşlemleri, aslında bu örnekler ikisinde kullanılıyor. İlk örnekte, her tek tek arama örtük bir işlemdir. İkinci örnekte, açık bir işlem tüm çağrılarını sarmalar. Belgelerine başına [yazma ileriye işlem günlüğü](https://msdn.microsoft.com/library/ms186259.aspx), işlem onayladığında, günlük kayıtlarını diske atılmış. İşlem kadar bu nedenle bir işlemde daha fazla çağrıları dahil ederek, işlem günlüğü yazma gecikmeye yol açabilir. Aslında, sunucunun işlem günlüğüne yazma işlemleri için toplu işleme olanaklı kılıyor.

Aşağıdaki tabloda bazı geçici test sonuçlarını gösterir. Aynı sıralı ekler ve işlemleri kullanılmadan gerçekleştirilen testler. Daha fazla perspektif için testlerin ilk Seti uzaktan bir dizüstü bilgisayarınız Microsoft Azure veritabanına verdi. Bir bulut hizmeti ve her ikisi de aynı Microsoft Azure veri merkezi içinde (Batı ABD) belgeler veritabanı testlerin ikinci Seti verdi. Aşağıdaki tabloda, sıralı ekler ve işlemleri kullanılmadan milisaniye cinsinden süre gösterilmektedir.

**Şirket içi Azure**:

| İşlemler | Hiçbir işlem (ms) | İşlem (ms) |
| --- | --- | --- |
| 1 |130 |402 |
| 10 |1208 |1226 |
| 100 |12662 |10395 |
| 1000 |128852 |102917 |

**Azure için Azure (aynı veri merkezinde)**:

| İşlemler | Hiçbir işlem (ms) | İşlem (ms) |
| --- | --- | --- |
| 1 |21 |26 |
| 10 |220 |56 |
| 100 |2145 |341 |
| 1000 |21479 |2756 |

> [!NOTE]
> Sonuçları kıyaslamaları değildir. Bkz: [bu konudaki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-topic).
> 
> 

Önceki test sonuçlarına bağlı olarak bir işlem içinde tek bir işlem kaydırma aslında performansı düşürür. Ancak, tek bir işlem içinde işlemlerinin sayısı arttıkça, performans geliştirmesi daha olarak işaretlenmiş olur. Tüm işlemler Microsoft Azure veri merkezi içinde olduğunda performans ayrıca daha belirgin farktır. Microsoft Azure veri merkezi dışında SQL veritabanından kullanmanın daha yüksek gecikme süresi işlemleri kullanarak performans kazancı gölgelendiren.

İşlemler kullanımı performansı artırabilirsiniz ancak devam [işlemleri ve bağlantıları için en iyi uygulamaları gözlemlemek](https://msdn.microsoft.com/library/ms187484.aspx). İşlemin olabildiğince kısa tutun ve iş tamamlandıktan sonra veritabanı bağlantısını kapatın. Önceki örnekte deyimiyle izleyen kod bloğunun tamamlandığında bağlantı kapalı olduğundan emin olmasını sağlar.

Önceki örnekte, iki satır ile ADO.NET kod için yerel işlem ekleyebilirsiniz gösterir. İşlemler sıralı ekleme, güncelleştirme ve silme işlemleri yapan kod performansını artırmak için hızlı bir yol sunar. Ancak, hızlı performans elde etmek için tablo değerli parametreleri gibi istemci tarafı, toplu işleme, avantajından daha fazla sınırlandıramazsınız kod değiştirmeyi göz önünde bulundurun.

ADO.NET işlemleri hakkında daha fazla bilgi için bkz: [ADO.NET yerel işlemlerde](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).

### <a name="table-valued-parameters"></a>Tablo değerli parametreleri
Tablo değerli parametreleri kullanıcı tanımlı tablo türü parametre olarak Transact-SQL deyimi, saklı yordamları ve işlevleri destekler. Bu istemci-tarafı toplu teknik tablo değerli parametre içindeki verilerin birden çok satır göndermenize izin verir. Tablo değerli parametrelerini kullanmak için önce bir tablo türü tanımlayın. Aşağıdaki Transact-SQL deyimini adlı bir tablo türü oluşturur **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


Kod içinde oluşturduğunuz bir **DataTable** tam aynı adlara ve tablo türü tür. Bunu geçirmek **DataTable** metin sorgusu veya saklı yordam parametresinde çağırın. Aşağıdaki örnek, bu teknik gösterir:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }

        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);

        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });

        cmd.ExecuteNonQuery();
    }

Önceki örnekte, **SqlCommand** nesnesi bir tablo değerli parametresinden satırları ekler  **@TestTvp** . Önceden oluşturulmuş **DataTable** nesne bu parametreyle atanması **SqlCommand.Parameters.Add** yöntemi. Bir çağrısında eklemeleri önemli ölçüde toplu işleme sıralı eklemeleri performansı artırır.

Daha önceki örnekteki artırmak için metin tabanlı komutu yerine bir saklı yordamı kullanın. Aşağıdaki Transact-SQL komutunu isteyen bir saklı yordamı oluşturur **SimpleTestTableType** tablo değerli parametre.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Ardından değiştirme **SqlCommand** nesne önceki kod örneğinde şu bildirimi.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

Çoğu durumda, diğer toplu teknikleri daha eşdeğer veya daha iyi performans tablo değerli parametrelere sahip. Diğer seçenekler daha esnek olduğundan tablo değerli genellikle tercih, parametreleridir. Örneğin, SQL toplu kopyalama gibi başka teknikler yalnızca yeni satır eklemeye izin verir. Ancak tablo değerli parametreleri ile mantığını saklı yordam, güncelleştirmelerinin hangi satırların belirlemek için kullanabilirsiniz ve hangi ekler. Tablo türü, belirtilen satır eklenmesi, silinmiş veya güncelleştirilmesi olup olmadığını gösteren bir "İşlem" sütunu içerecek şekilde da değiştirilebilir.

Aşağıdaki tabloda, milisaniye cinsinden geçici tablo değerli parametre kullanımı için test sonuçlarını gösterir.

| İşlemler | Şirket içi Azure (ms) | Azure aynı veri merkezinde (ms) |
| --- | --- | --- |
| 1 |124 |32 |
| 10 |131 |25 |
| 100 |338 |51 |
| 1000 |2615 |382 |
| 10000 |23830 |3586 |

> [!NOTE]
> Sonuçları kıyaslamaları değildir. Bkz: [bu konudaki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-topic).
> 
> 

Toplu işleme gelen performans kazancı hemen açıktır. Önceki sıralı testinde 1000 işlemleri veri merkezi dışında ve veri merkezi içinde 21 saniyeden 129 saniye sürdü. Ancak tablo değerli parametreleri ile 1000 işlemleri yalnızca 2.6 saniye veri merkezi dışında ve veri merkezi içinde 0,4 saniye sürer.

Tablo değerli parametreleri hakkında daha fazla bilgi için bkz: [tablo değerli parametreleri](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>SQL toplu kopyalama
SQL toplu kopyalama büyük miktarlarda verinin bir hedef veritabanına eklemek için başka bir yoludur. .NET uygulamaları kullanabileceğiniz **SqlBulkCopy** toplu gerçekleştirmek için sınıf ekleme işlemleri. **SqlBulkCopy** için komut satırı aracı, işlevinde benzer **Bcp.exe**, veya Transact-SQL deyimini **BULK INSERT**. Aşağıdaki kod örneğinde kopyalama toplu satırları kaynak gösterilmektedir **DataTable**, SQL Server, hedef tabloya MyTable tablo.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Toplu kopyalama tablo değerli parametre tercih edilen olduğu bazı durumlar vardır. Tablo değerli parametreleri karşılık toplu ekleme işlemleri konusunda karşılaştırma tablosuna bakın [tablo değerli parametreleri](https://msdn.microsoft.com/library/bb510489.aspx).

Aşağıdaki geçici test sonuçları ile toplu işleme performansını gösterir **SqlBulkCopy** milisaniye cinsinden.

| İşlemler | Şirket içi Azure (ms) | Azure aynı veri merkezinde (ms) |
| --- | --- | --- |
| 1 |433 |57 |
| 10 |441 |32 |
| 100 |636 |53 |
| 1000 |2535 |341 |
| 10000 |21605 |2737 |

> [!NOTE]
> Sonuçları kıyaslamaları değildir. Bkz: [bu konudaki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-topic).
> 
> 

Daha küçük toplu boyutlarını kullan tablo değerli parametreleri outperformed **SqlBulkCopy** sınıfı. Ancak, **SqlBulkCopy** 12-%31 tablo değerli parametreleri daha hızlı 1.000 ile 10.000 satırları testler için gerçekleştirilir. Tablo değerli parametreleri gibi **SqlBulkCopy** özellikle toplu olmayan işlemler performansını karşılaştırıldığında toplu eklemeleri için iyi bir seçenek değil.

ADO.NET toplu kopyalama hakkında daha fazla bilgi için bkz: [SQL Server toplu kopyalama işlemleri](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Birden çok satır parametreli INSERT deyimleri
Bir küçük toplu işlemler için birden çok satır ekleyen büyük bir parametreli INSERT deyimi oluşturmak için alternatiftir. Aşağıdaki kod örneği, bu tekniği gösterir.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";

        SqlCommand cmd = new SqlCommand(insertCommand, connection);

        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }

        cmd.ExecuteNonQuery();
    }


Bu örnek, temel kavramlar göstermek için tasarlanmıştır. Daha gerçekçi bir senaryo, sorgu dizesi ve komut parametreleri aynı anda oluşturmak için gerekli varlıkları döngüsü. Bu bu şekilde işlenebilir satırların toplam sayısını sınırlar şekilde 2100 sorgu parametreleri, toplam için sınırlı olur.

Aşağıdaki geçici test sonuçları INSERT deyimi bu tür performansını milisaniye olarak gösterir.

| İşlemler | Tablo değerli parametreleri (ms) | Tek deyimli Ekle (ms) |
| --- | --- | --- |
| 1 |32 |20 |
| 10 |30 |25 |
| 100 |33 |51 |

> [!NOTE]
> Sonuçları kıyaslamaları değildir. Bkz: [bu konudaki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-topic).
> 
> 

Bu yaklaşım, 100'den az satırlar toplu işlemler için biraz daha hızlı olabilir. Geliştirme küçük olsa da, bu teknik iyi belirli uygulama senaryonuz çalışabilir başka bir seçenektir.

### <a name="dataadapter"></a>DataAdapter
**DataAdapter** sınıfı değiştirmenize olanak verir bir **DataSet** nesne ve değişiklikleri INSERT, UPDATE ve DELETE işlemleri olarak gönderin. Kullanıyorsanız **DataAdapter** bu şekilde, her ayrı bir işlem için ayrı çağrıları yapılmadan unutmayın. Performansı artırmak için kullanıyoruz **UpdateBatchSize** özelliği için aynı anda toplu işlem sayısı. Daha fazla bilgi için bkz: [toplu işlemleri kullanarak DataAdapters gerçekleştirme](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Varlık çerçevesi
Toplu işleme Entity Framework şu anda desteklemiyor. Topluluktaki farklı geliştiriciler çalıştı geçici çözümler, geçersiz kılma gibi göstermek **SaveChanges** yöntemi. Ancak çözümler genellikle karmaşık ve özelleştirilmiş uygulama ve veri modeli. Entity Framework codeplex proje şu anda bu özellik isteğinde tartışma sayfa yok. Bu tartışma görüntülemek için bkz: [tasarım Toplantı Notları - 2 Ağustos 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
Bütünlük açısından XML toplu bir strateji olarak kadar önemli olduğunu düşündüğünüz. Ancak, XML kullanımını diğer yöntemleri hiçbir avantajları ve birkaç dezavantajları vardır. Tablo değerli parametreleri yaklaşım benzer, ancak bir XML dosyası veya dize kullanıcı tanımlı bir tablo yerine bir saklı yordam geçirilir. Saklı yordam saklı yordamı komutlarda ayrıştırır.

Bu yaklaşımın birkaç dezavantajları şunlardır:

* XML ile çalışma sıkıcı olabilir ve hataya.
* Veritabanında XML ayrıştırma, yoğun CPU kullanımına neden olabilir.
* Çoğu durumda, bu yöntem tablo değerli parametreleri yavaştır.

Bu nedenlerle, toplu sorgular için XML kullanılması önerilmez.

## <a name="batching-considerations"></a>Dikkat edilecek noktalar toplu işleme
Aşağıdaki bölümlerde SQL veritabanı uygulamalarında toplu işleme kullanmak için daha fazla yönergeler sağlar.

### <a name="tradeoffs"></a>Bileşim
Mimarinizi bağlı olarak, toplu işleme kolaylığını performans ve dayanıklılık arasındaki içerebilir. Örneğin, burada rolünüze beklenmedik bir şekilde arıza senaryoyu göz önünde bulundurun. Bir veri satırı kaybederseniz, büyük bir grup gönderilmeyen satır kaybetme etkisini küçük bir etkisidir. Belirtilen zaman penceresi veritabanına gönderilmeden satırları arabellek yapıldığında büyük bir risk alınamıyor.

Bu kolaylığını nedeniyle, toplu işlem türünü değerlendirin. Daha agresif toplu (büyük toplu işlemler ve daha uzun zaman pencereleri) verilerle daha az önemlidir.

### <a name="batch-size"></a>Toplu işlem boyutu
Bizim testlerinde genellikle daha küçük parçalara büyük toplu çiğnemekten avantajlı vardı. Aslında, bu alt bölümü genellikle tek bir büyük toplu iş gönderme daha yavaş performans ile sonuçlandı. Örneğin, 1000 satırı eklemek istediğiniz bir senaryo düşünün. Aşağıdaki tabloda küçük yığınlara bölünmüş zaman 1000 satır eklemek için tablo değerli parametrelerini kullanmak için gereken süreyi gösterir.

| Toplu işlem boyutu | Yineleme | Tablo değerli parametreleri (ms) |
| --- | --- | --- |
| 1000 |1 |347 |
| 500 |2 |355 |
| 100 |10 |465 |
| 50 |20 |630 |

> [!NOTE]
> Sonuçları kıyaslamaları değildir. Bkz: [bu konudaki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-topic).
> 
> 

Aynı anda göndermek için en iyi performans için 1000 satırı olduğunu görebilirsiniz. (Burada gösterilmiyor) diğer testlerinde 10000 satır toplu 5000 iki yığınlara ayırmak için bir küçük performans kazancı vardı. Ancak, belirli veri ve bu bulgularını doğrulamak için toplu iş boyutu testleri gerçekleştirmeniz gerekir böylece bu testler için tablo şemasını görece basit.

Dikkate alınması gereken diğer bir etken toplam toplu iş çok büyük olursa, SQL veritabanı azaltma ve toplu iş yürütmesi Reddet olmasıdır. En iyi sonuçlar için ideal toplu iş boyutu olup olmadığını belirlemek için size özel senaryonun test edin. Toplu iş boyutu performans veya hatalar göre hızlı ayarlamalar etkinleştirmek için çalışma zamanında yapılandırılabilir olun.

Son olarak, toplu iş boyutu toplu işleme ile ilişkili riskleri taşıyan dengeleyin. Geçici bir hata veya rol başarısız olursa, işlemi yeniden denemeden veya toplu veri kaybı sonuçlarıyla göz önünde bulundurun.

### <a name="parallel-processing"></a>Paralel işleme
Ne toplu iş boyutunu azaltma yaklaşım sürdü ancak çoklu iş parçacığı çalışmayı yürütmek için kullanılan? Yeniden testlerimizde birkaç küçük birden çok iş parçacıklı toplu işlemi genellikle tek bir büyük toplu daha da kötüsü gerçekleştirdiği gösterdi. Aşağıdaki sınama, 1000 satırı bir veya daha fazla paralel toplu olarak eklemek çalışır. Bu test nasıl daha fazla eşzamanlı toplu gerçekte performansı düşebilir gösterir.

| Toplu iş boyutu [yineleme] | İki iş parçacığı (ms) | Dört iş parçacığı (ms) | Altı iş parçacıkları (ms) |
| --- | --- | --- | --- |
| 1000 [1] |277 |315 |266 |
| 500 [2] |548 |278 |256 |
| 250 [4] |405 |329 |265 |
| 100 [10] |488 |439 |391 |

> [!NOTE]
> Sonuçları kıyaslamaları değildir. Bkz: [bu konudaki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-topic).
> 
> 

Paralellik nedeniyle performans düşüşü birkaç olası nedenleri şunlardır:

* Bir yerine birden çok eşzamanlı ağ çağrıları vardır.
* Birden çok işlem tek bir tabloyu karşı Çekişme ve engelleme neden olabilir.
* İle ilişkili ek yüklerini vardır çoklu iş parçacığı kullanımı.
* Birden çok bağlantı açma gider paralel işleme yararı ağır.

Farklı tablolar veya veritabanlarına hedefliyorsanız, bu strateji ile elde bazı performans görmek mümkündür. Veritabanı parçalama veya Federasyon bir senaryo için bu yaklaşım olacaktır. Parçalama birden çok veritabanını kullanır ve her bir veritabanı için farklı veri yönlendirir. Her küçük bir toplu iş farklı bir veritabanına olacaksa, paralel olarak sonra işlemleri daha etkili olabilir. Ancak, performans kazancı çözümünüzde veritabanı parçalama kullanmaya karar için temel olarak kullanmak önemli değildir.

Bazı tasarımları küçük toplu işlemlerin Paralel yürütme yük altında bir sistem isteklerin geliştirilmiş verimini sonuçlanabilir. Tek bir büyük toplu işlemek daha hızlı olsa da, bu durumda, birden çok toplu işlem paralel işleme daha etkili olabilir.

Paralel yürütme kullanırsanız, en fazla çalışan iş parçacığı sayısını denetleme göz önünde bulundurun. Daha küçük bir sayı çekişmelerinin azalmasını ve daha hızlı bir yürütme süresi neden olabilir. Ayrıca, bu bağlantıları ve işlemleri hedef veritabanı yerleştirir ek yük göz önünde bulundurun.

### <a name="related-performance-factors"></a>İlgili performans Etkenler
Toplu işleme veritabanı performansını normal yönergeler de etkiler. Örneğin, INSERT performans büyük birincil bir anahtar veya birçok kümelenmemiş dizinlerine sahip tablolar için azaltılır.

Tablo değerli parametreleri bir saklı yordam kullanırsanız, komutunu kullanabilirsiniz **SET NOCOUNT ON** yordamın başlangıcında. Bu bildirimi yordamda etkilenen satırların sayımını dönüş gizler. Ancak, testlerinde bizim, kullanımını **SET NOCOUNT ON** hiçbir etkisi olan ya da performansı düşebilir. Test saklı yordamı ile tek bir basit **Ekle** tablo değerli parametre komutu. Daha karmaşık saklı yordamlar bu deyimden yararlanabileceğini mümkündür. Ancak bu ekleme varsaymayın **SET NOCOUNT ON** saklı yordama otomatik olarak performansı artırır. Etkisini anlamak için saklı yordam olan ve olmayan test **SET NOCOUNT ON** deyimi.

## <a name="batching-scenarios"></a>Toplu işleme senaryoları
Aşağıdaki bölümlerde üç uygulama senaryolarında tablo değerli parametreleri kullanma açıklanmaktadır. İlk senaryoda arabelleğe alma ve yığınlama birlikte nasıl çalışabileceğini gösterir. İkinci senaryo, bir tek bir saklı yordam çağrısında ana-ayrıntı işlemleri gerçekleştirerek performansı artırır. Son senaryo, bir "UPSERT" işleminde tablo değerli parametreleri kullanmayı gösterir.

### <a name="buffering"></a>Arabelleğe alma
Toplu işleme için belirgin aday olan bazı senaryolar olsa da, Gecikmeli işlemesi toplu işleme avantajı ele geçirebilir birçok senaryo vardır. Ancak, Gecikmeli işleme beklenmeyen bir hata olması durumunda verileri kaybolur daha fazla risk taşır. Sonuçları düşünün ve bu riski anlamak önemlidir.

Örneğin, her kullanıcının Gezinti geçmişini izleyen bir web uygulaması göz önünde bulundurun. Her sayfa isteğinde, uygulama kullanıcının sayfa görünümü kaydetmek için çağrısı bir veritabanı hale getirebilir. Ancak daha yüksek performans ve ölçeklenebilirlik kullanıcıların Gezinti etkinlikleri arabelleğe alma ve toplu veritabanına bu verileri göndererek elde edilebilir. Veritabanı Güncelleştirme geçen süre ve/veya arabellek boyutu ile tetikleyebilirsiniz. Örneğin, bir kural toplu sonra 20 saniye veya arabellek 1000 öğeleri ulaştığında işlenmesi gerektiğini belirtebilirsiniz.

Aşağıdaki kod örneğinde [reaktif uzantılar - Rx](https://msdn.microsoft.com/data/gg577609) bir izleme sınıfı tarafından başlatılan arabelleğe alınan olayları işlemek için. Arabellek doldurduğunda veya bir zaman aşımı ulaşıldığında, kullanıcı verilerini toplu tablo değerli bir parametre olan veritabanına gönderilir.

Aşağıdaki NavHistoryData sınıfı kullanıcı Gezinti ayrıntıları modeller. Kullanıcı tanımlayıcısı, erişilen URL ve erişim zamanı gibi temel bilgileri içerir.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

NavHistoryDataMonitor sınıfı, kullanıcı Gezinti verilerinin veritabanına arabelleğe almak için sorumludur. Bir yöntemi, yükselterek yanıt RecordUserNavigationEntry içeren bir **OnAdded** olay. Aşağıdaki kod, olay tabanlı bir observable koleksiyonu oluşturmak için Rx kullanan Oluşturucu mantığı gösterir. Ardından bu observable koleksiyona arabellek yöntemiyle abone. Aşırı arabellek 20 dakikada veya 1000 girişleri olarak gönderilmesi gerektiğini belirtir.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

İşleyici arabelleğe alınan öğelerin tümünü bir tablo değerli türüne dönüştürür ve ardından bu tür toplu işler bir saklı yordamı geçirir. Aşağıdaki kod NavHistoryDataEventArgs ve NavHistoryDataMonitor sınıfları tam tanımını gösterir.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }

    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;

        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }

        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }

        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }

            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();

                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;

                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });

                cmd.ExecuteNonQuery();
            }
        }
    }

Arabelleğe alma Bu sınıf kullanmak için uygulama statik NavHistoryDataMonitor nesnesi oluşturur. Sayfasında, bir kullanıcının eriştiği her zaman uygulama NavHistoryDataMonitor.RecordUserNavigationEntry yöntemini çağırır. Arabelleğe alma mantığını yığınlardaki veritabanına bu girişler gönderme ilgilenebilmek devam eder.

### <a name="master-detail"></a>Ana ayrıntısı
Tablo değerli parametreleri basit Ekle senaryoları için kullanışlıdır. Ancak, birden fazla tablo içeren toplu ekler daha zor olabilir. "Ana/ayrıntı" senaryosu iyi bir örnektir. Ana Tablo birincil varlık tanımlar. Bir veya daha fazla ayrıntı tabloları varlık hakkında daha fazla veri depolar. Bu senaryoda, benzersiz bir ana varlık ayrıntıları ilişki yabancı anahtar ilişkileri uygulayın. Bir PurchaseOrder tablosu ve onun ilişkili OrderDetail tablosu basitleştirilmiş bir sürümünü göz önünde bulundurun. Aşağıdaki Transact-SQL ile dört sütun PurchaseOrder tablo oluşturur: OrderID, OrderDate, CustomerID ve durum.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Her sipariş bir veya daha fazla ürün satın alma işlemleri içerir. Bu bilgiler PurchaseOrderDetail tablosunda yakalanır. Aşağıdaki Transact-SQL beş sütunlarla PurchaseOrderDetail tablo oluşturur: OrderID, OrderDetailID, ProductID, UnitPrice ve OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

PurchaseOrderDetail tablosundaki OrderID sütunu sipariş PurchaseOrder tablosundan başvurmalıdır. Bir yabancı anahtar aşağıdaki tanımını bu kısıtlamayı zorlar.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Tablo değerli parametreleri kullanmak üzere her bir hedef tablo için bir kullanıcı tanımlı tablo türü olması gerekir.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO

    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Ardından, bu tür tabloları kabul eden bir saklı yordam tanımlayın. Bu yordam bir uygulamanın yerel olarak bir dizi siparişleri ve tek bir çağrı sipariş ayrıntılarını toplu sağlar. Aşağıdaki Transact-SQL bu satın alma siparişi örneği için tam saklı yordam bildirimi sağlar.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;

    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );

          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;

    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;

    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

Bu örnekte, yerel olarak tanımlanmış @IdentityLink tablo, yeni eklenen satırlar gerçek OrderID değerleri depolar. Bu sipariş tanımlayıcıları değerden geçici OrderID farklı @orders ve @details tablo değerli parametreleri. Bu nedenle, @IdentityLink tablo ardından OrderID değerleri bağlanır @orders PurchaseOrder tablosundaki yeni satırlar için gerçek OrderID değerler için parametre. Bu adımdan sonra @IdentityLink tablo kolaylaştırmak yabancı anahtar kısıtlaması karşılayan gerçek OrderID sipariş ayrıntılarla ekleniyor.

Bu saklı yordam, kod veya diğer Transact-SQL çağrıları kullanılabilir. Kod örneği için Bu raporda, tablo değerli parametreleri bölümüne bakın. Aşağıdaki Transact-SQL sp_InsertOrdersBatch çağrı gösterilmektedir.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType

    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')

    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)

    exec sp_InsertOrdersBatch @orders, @details

Bu çözüm, 1'den başlayan OrderID değerler kümesini kullanmak üzere her bir toplu iş izin verir. Bu geçici OrderID değerleri toplu ilişkileri açıklar ancak gerçek OrderID değerler ekleme işlemi sırasındaki belirlenir. Aynı deyimleri önceki örnekte art arda çalıştırabilir ve bu veritabanında benzersiz siparişler oluşturmaz. Bu nedenle, bu teknik toplu işleme kullanırken yinelenen siparişleri engelleyen daha fazla kod veya veritabanı mantığı eklemeyi düşünün.

Bu örnek, tablo değerli parametreleri kullanarak ana / ayrıntı işlemleri gibi daha karmaşık veritabanı işlemleri toplu olduğunu gösterir.

### <a name="upsert"></a>UPSERT
Başka bir toplu senaryo, aynı anda var olan satır ve ekleme yeni satırlar güncelleştirilmesini gerektirir. Bu işlem, bazen bir "UPSERT" (güncelleştirme + Ekle) işlem olarak da adlandırılır. Ekle ve GÜNCELLEŞTİRMEK için ayrı çağrıları yapma yerine MERGE deyimi bu görev için uygundur. MERGE deyiminin hem INSERT işlemi ve tek bir çağrı işlemlerinde güncelleştirin.

Tablo değerli parametreleri ile MERGE deyimi, güncelleştirmeleri ve eklemeleri gerçekleştirmek için kullanılabilir. Örneğin, şu sütunları içerir basitleştirilmiş bir çalışan tablosu göz önünde bulundurun: EmployeeID, FirstName, LastName, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))

Bu örnekte, SocialSecurityNumber birden fazla çalışanı birleştirme benzersiz olduğunu olgu kullanabilirsiniz. İlk olarak, kullanıcı tanımlı tablo türü oluşturun:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Ardından, bir saklı yordam oluşturma veya güncelleştirme gerçekleştirmek ve eklemek için MERGE deyiminin kullanan kodu yazın. Aşağıdaki örnek, bir tablo değerli parametre MERGE deyiminin kullanır @employees, EmployeeTableType türünde. İçeriğini @employees tablo aşağıda gösterilmez.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Daha fazla bilgi için belgeler ve örnekler MERGE deyimi için bkz. Aynı iş bir çok-depolanan adımda gerçekleştirilebilir rağmen yordam çağrısı ile ayrı ekleme ve güncelleştirme işlemleri, MERGE deyiminin daha verimli. Veritabanı kod iki veritabanı çağrılarını ekleme ve güncelleştirme için gerek kalmadan doğrudan MERGE deyiminin kullanan Transact-SQL çağrıları de oluşturabilirsiniz.

## <a name="recommendation-summary"></a>Öneri özeti
Aşağıdaki listede, bu konuda tartışılan toplu önerilerin bir özeti verilmiştir:

* Arabelleğe alma ve toplu işleme SQL veritabanı uygulamaları ölçeklenebilirliğini ve performansı artırmak için kullanır.
* Toplu işleme/arabelleğe alma ve dayanıklılık arasındaki dengelemeden anlayın. Bir rol hata sırasında işlenmemiş bir toplu iş açısından kritik verilerin kaybı riski toplu işleme performans avantajı üstün olabilir.
* Gecikme süresini azaltmak için tüm çağrılar veritabanı tek bir veri merkezi içinde tutmak çalışır.
* Tek bir toplu teknik seçerseniz, tablo değerli parametreleri en iyi performans ve esneklik sunar.
* Hızlı Ekle performansı için aşağıdaki genel yönergeleri izleyin, ancak senaryonuz test:
  * < 100 satır için tek bir parametreli Ekle komutunu kullanın.
  * < 1000 satırı için tablo değerli parametreleri kullanın.
  * İçin > = 1000 satır SqlBulkCopy kullanın.
* İçin güncelleştirme ve silme işlemleri, her satır tablo parametresindeki doğru işlemi belirler saklı yordam mantığı ile tablo değerli parametreleri kullanın.
* Toplu iş boyutu yönergeleri:
  * Uygulama ve iş gereksinimleri için anlamlı en büyük toplu iş boyutu kullanılır.
  * Büyük toplu performans kazancı geçici veya geri dönülemez hataları riskleri taşıyan dengeleyin. Yeniden deneme sonucu veya toplu veri kaybı nedir? 
  * SQL veritabanı reddetmek değil olduğunu doğrulamak için en büyük toplu iş boyutu sınayın.
  * Bu denetim, toplu iş boyutu veya arabelleğe alma zaman penceresi gibi toplu işleme yapılandırma ayarları oluşturur. Bu ayarları esneklik sağlar. Bulut hizmeti yeniden dağıtmadan üretim toplu davranışı değiştirebilirsiniz.
* Bir veritabanı tek bir tabloda çalışmayabilir toplu işlemlerin Paralel yürütme kaçının. Tek bir toplu iş birden fazla çalışan iş parçacıkları arasında bölmek seçerseniz, ideal iş parçacığı sayısını belirlemek için testleri çalıştırın. Belirtilmeyen bir eşik sonra daha fazla iş parçacığı performansını düşürebilir yerine onu artırın.
* Boyut ve daha fazla senaryoları için toplu işleme uygulamanın yolu sürede arabelleğe almayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede nasıl veritabanı tasarımını ve toplu işleme için ilgili teknikleri kodlama uygulama performansı ve ölçeklenebilirliği artırabilir üzerinde odaklanmıştır. Ancak bu yalnızca bir faktör olarak genel stratejinize. Daha fazla yolunu performansı ve ölçeklenebilirliği artırmak için bkz: [tek veritabanları için Azure SQL Database performans rehberi](sql-database-performance-guidance.md) ve [esnek havuzlar için fiyat ve performans konuları](sql-database-elastic-pool-guidance.md).

