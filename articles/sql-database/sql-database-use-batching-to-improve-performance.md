---
title: Azure SQL veritabanı uygulama performansını artırmak için toplu işlem kullanma
description: Konu, veritabanı işlemleri büyük ölçüde toplu işleme hız ve uygulamalarınızı Azure SQL veritabanı'nın ölçeklenebilirliğini artırır, kanıt sağlar. Bu toplu işleme teknikleri herhangi bir SQL Server veritabanı için geçerli olsa da Azure üzerinde makalenin odak gelir.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: genemi
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: e76b5ecd3d6401c317f6500ec376fc25d3fa55b8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60331137"
---
# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>SQL veritabanı uygulama performansını artırmak için toplu işlem kullanma

Önemli ölçüde toplu işlemler için Azure SQL veritabanı, uygulamalarınızı ölçeklenebilirliğini ve performansı artırır. Avantajları anlamak için bu makalenin ilk bölümünü karşılaştırma sıralı ve toplu istekleri bir SQL veritabanı için bazı örnek test sonuçlarını içerir. Bu makalenin geri kalanında teknikleri, senaryolar ve başarılı bir şekilde Azure uygulamalarınızda toplu işlem kullanma size yardımcı olacak hususlar gösterir.

## <a name="why-is-batching-important-for-sql-database"></a>SQL veritabanı için önemli toplu işleme neden

Uzak bir hizmete çağrı toplu işleme, performans ve ölçeklenebilirliği artırmak için iyi bilinen bir stratejidir. İşlem maliyetleri tüm etkileşimleri serileştirme, ağ aktarımı ve seri durumundan çıkarma gibi uzak bir hizmetle var. sabittir. Tek bir toplu iş birçok ayrı işlemlere paketleme bu maliyetleri en aza indirir.

Bu belgede, çeşitli SQL veritabanı'nı stratejileri ve senaryoları toplu işleme incelemek istiyoruz. Bu stratejiler ayrıca SQL Server kullanan şirket içi uygulamalar için önemli olmakla birlikte, SQL veritabanı için toplu işlem kullanımına vurgulama için birkaç nedeni vardır:

* Varsa büyük olasılıkla daha büyük ağ gecikmesi SQL veritabanına erişirken özellikle aynı Microsoft Azure veri merkezi dışından SQL veritabanı'ndan erişme.
* SQL veritabanı çok kiracılı özelliklerini anlamına gelir, veritabanının genel ölçeklenebilirlik için veri erişim katmanı koreluje s verimliliğini. SQL veritabanı tek bir Kiracı/Kullanıcı zarar diğer kiracıları için veritabanı kaynakların tamamını kullanmasını önlemek gerekir. Önceden tanımlanmış kotalarını aşan kullanımlar için yanıt olarak, SQL veritabanı aktarım hızını azaltabilir veya azaltma özel durumları ile yanıt. Toplu işleme gibi verimliliği, SQL veritabanı hakkında daha fazla iş limitler ulaşmadan önce olanak sağlar. 
* Toplu işleme ayrıca birden çok veritabanı (parçalama) kullanan mimarileri için etkili olur. Her bir veritabanı birimi etkileşiminizi verimliliğini genel ölçeklenebilirlik bir anahtarı faktörünü hala var. 

SQL veritabanı kullanmanın avantajları veritabanını barındıran sunucular yönetmeniz gerekmez biridir. Ancak, bu yönetilen altyapısı ayrıca farklı veritabanı iyileştirmeler hakkında düşünmek zorunda anlamına gelir. Artık veritabanı donanım veya ağ altyapısını geliştirmek için de bakabilirsiniz. Microsoft Azure, bu ortamlarda denetler. Denetleyebileceğiniz ana uygulamanızı SQL veritabanıyla nasıl etkileşim kurduğu bir alandır. Toplu işleme bu iyileştirmeler biridir. 

Kağıt ilk kısmı, SQL veritabanı'nı kullanan .NET uygulamaları için toplu işleme çeşitli teknikler inceler. Son iki bölüm toplu işleme yönergeleri ve senaryoları kapsar.

## <a name="batching-strategies"></a>Toplu işleme stratejileri

### <a name="note-about-timing-results-in-this-article"></a>Bu makalede zamanlama sonuçlarıyla ilgili not

> [!NOTE]
> Sonuçları Kıyaslama değildir ancak göstermeye yöneliktir **göreli performans**. Zamanlamaları, ortalama, en az 10 test çalışmalarını temel alır. Boş bir tablo ekler işlemlerdir. Bu testleri ölçülen öncesi V12 olan ve bunlar mutlaka kullanarak yeni bir V12 veritabanında karşılaşabileceğiniz aktarım hızı için karşılık gelmez [DTU hizmet katmanları](sql-database-service-tiers-dtu.md) veya [sanal çekirdek hizmet katmanları](sql-database-service-tiers-vcore.md). Toplu işlem teknik göreli avantajı benzer olmalıdır.

### <a name="transactions"></a>İşlemler

Bir gözden geçirme işlemleri açıklayan tarafından toplu işleme, başlamak için ilginç görünüyor. Ancak istemci tarafı işlemleri kullanımı performansı artıran zarif bir toplu işlem sunucu tarafı etkisi yoktur. Ve yalnızca birkaç satır kod ile işlemleri sıralı işlemlerinin performansını artırmak için hızlı bir yolunu sağlarlar şekilde eklenebilir.

INSERT dizisini içeren aşağıdaki C# kodu göz önünde bulundurun ve basit bir tablo üzerinde işlem güncelleştirin.

```csharp
List<string> dbOperations = new List<string>();
dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
dbOperations.Add("insert MyTable values ('new value',1)");
dbOperations.Add("insert MyTable values ('new value',2)");
dbOperations.Add("insert MyTable values ('new value',3)");
```
Aşağıdaki ADO.NET kod, sırayla şu işlemleri gerçekleştirir.

```csharp
using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
{
    conn.Open();

    foreach(string commandString in dbOperations)
    {
        SqlCommand cmd = new SqlCommand(commandString, conn);
        cmd.ExecuteNonQuery();
    }
}
```

Bu kod iyileştirmek için en iyi yolu, bazı formun istemci-tarafı bu çağrılar toplu işlem uygulamaktır. Ancak, bir işlemde yalnızca çağrı sarmalama tarafından bu kod performansını artırmak için basit bir yolu yoktur. Aşağıda, bir işlem kullandığı aynı kodu verilmiştir.

```csharp
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
```

İşlem, aslında Bu örneklerin her ikisi içinde kullanılıyor. İlk örnekte, her bir çağrıyı örtük bir işlemdir. İkinci örnekte, açık bir işlem tüm çağrıları sarar. Belgelerine başına [yazma Tamamlanan işlem günlüğü](https://msdn.microsoft.com/library/ms186259.aspx), günlük kayıtları hareketi tamamlar olduğunda diske Temizlenen. İşlem, kadar bu nedenle bir işlemde çağrılar dahil ederek, işlem günlüğüne yazma gecikmeye yol açabilir. Aslında, sunucunun işlem günlüğüne yazma işlemleri için toplu işleme tanımış olursunuz.

Aşağıdaki tablo bazı geçici test sonuçlarını gösterir. Testler aynı sıralı ekler ve işlem olmadan gerçekleştirdi. Daha fazla perspektif için testlerin ilk Seti uzaktan dizüstü bilgisayarınızdan Microsoft Azure veritabanına çalıştı. Bir bulut hizmeti ve her ikisi de aynı Microsoft Azure veri merkezi içinde (Batı ABD) belgeler veritabanı testlerin ikinci Seti çalıştı. Aşağıdaki tabloda sıralı ekler ve işlem olmadan, milisaniye cinsinden gösterir.

**Şirket içinden azure'a**:

| İşlemler | Hiçbir işlem (ms) | İşlem (ms) |
| --- | --- | --- |
| 1 |130 |402 |
| 10 |1208 |1226 |
| 100 |12662 |10395 |
| 1000 |128852 |102917 |

**Azure'dan Azure'a (aynı veri merkezinde)** :

| İşlemler | Hiçbir işlem (ms) | İşlem (ms) |
| --- | --- | --- |
| 1 |21 |26 |
| 10 |220 |56 |
| 100 |2145 |341 |
| 1000 |21479 |2756 |

> [!NOTE]
> Sonuçları Kıyaslama değildir. Bkz: [bu makaledeki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-article).

Önceki test sonuçlarına göre bir işlemde tek bir işlem sarmalama aslında performansı düşürür. Ancak tek bir işlem içinde işlemlerin sayısı arttıkça, performans geliştirmesinden daha işaretli olur. Microsoft Azure veri merkezi içinde tüm işlemleri ortaya çıktığında bir performans farkı ayrıca daha belirgin. SQL veritabanı'ndan Microsoft Azure veri merkezi dışından kullanarak daha yüksek gecikme süresiyle işlemleri kullanarak performans kazancı gölgelendiren.

İşlem kullanımı performansı artırabilirsiniz ancak devam [işlemleri ve bağlantılar için en iyi gözlemleyin](https://msdn.microsoft.com/library/ms187484.aspx). İşlemin olabildiğince kısa tutun ve iş tamamlandıktan sonra veritabanı bağlantısını kapatın. Önceki örnekte using deyimi izleyen kod bloğunun tamamlandığında bağlantı kapalı olduğundan emin olmasını sağlar.

Önceki örnekte, iki satır ile ADO.NET kod için yerel bir işlem ekleyebilirsiniz gösterir. İşlem sıralı ekleme, güncelleştirme ve silme işlemleri yapan kod performansını artırmak için hızlı bir yol sunar. Ancak, en hızlı performans elde etmek için tablo değerli parametreleri gibi istemci tarafı, toplu işleme, avantajından daha fazla sınırlandıramazsınız kodunun değiştirilmesi göz önünde bulundurun.

ADO.NET'te işlemleri hakkında daha fazla bilgi için bkz. [ADO.NET'te yerel işlemler](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).

### <a name="table-valued-parameters"></a>Tablo değerli Parametreler

Tablo değerli parametreleri kullanıcı tanımlı tablo türleri parametre olarak Transact-SQL deyimleri, saklı yordamları ve işlevleri destekler. Bu istemci-tarafı toplu teknik tablo değerli parametre içinde verilerinizin birden çok satır göndermenize olanak sağlar. Tablo değerli parametreleri kullanmak için öncelikle bir tablo türü tanımlar. Aşağıdaki Transact-SQL deyimini adlı bir tablo türü oluşturur **MyTableType**.

```sql
    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
```

Kod içinde oluşturduğunuz bir **DataTable** tablo türüne türlerini ve tam olarak aynı ada sahip. Bunu geçirmek **DataTable** metin sorgusu veya saklı yordam parametre çağırın. Aşağıdaki örnek, bu teknik gösterir:

```csharp
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
```

Önceki örnekte, **SqlCommand** nesnesi bir tablo değerli parametresi, satırları ekler  **\@TestTvp**. Önceden oluşturulmuş **DataTable** nesnesinin ile bu parametreye atandığı **SqlCommand.Parameters.Add** yöntemi. Tek bir çağrıdaki ekler önemli ölçüde toplu işleme sıralı ekler performansını artırır.

Önceki örneği daha da geliştirmek için metin tabanlı bir komut yerine bir saklı yordamı kullanın. Aşağıdaki Transact-SQL komutunu isteyen bir saklı yordamı oluşturur **SimpleTestTableType** tablo değerli parametre.

```sql
CREATE PROCEDURE [dbo].[sp_InsertRows] 
@TestTvp as MyTableType READONLY
AS
BEGIN
INSERT INTO MyTable(mytext, num) 
SELECT mytext, num FROM @TestTvp
END
GO
```

Ardından değiştirme **SqlCommand** önceki kod örneğinde aşağıdaki bildirim nesnesi.

```csharp
SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
cmd.CommandType = CommandType.StoredProcedure;
```

Çoğu durumda, diğer toplu işleme teknikleri daha eşit veya daha iyi performans tablo değerli parametrelere sahip. Diğer seçenekleri daha esnek olduğundan tablo değerli parametre genellikle tercih,. Örneğin, SQL toplu kopyalama gibi başka teknikler yalnızca yeni satırların eklenmesini izin verir. Ancak tablo değerli parametre mantığını saklı yordamda güncelleştirmelerinin hangi satırların belirlemek için kullanabileceğiniz ve hangi ekler. Tablo türünü de belirtilen satırın eklenen, güncelleştirilen veya silinen olup olmadığını gösteren bir "İşlem" sütun içerecek şekilde değiştirilebilir.

Aşağıdaki tabloda, milisaniye cinsinden tablo değerli parametre kullanımı için geçici test sonuçlarını gösterir.

| İşlemler | Şirket içinden azure'a (ms) | Aynı Azure veri merkezi (ms) |
| --- | --- | --- |
| 1 |124 |32 |
| 10 |131 |25 |
| 100 |338 |51 |
| 1000 |2615 |382 |
| 10000 |23830 |3586 |

> [!NOTE]
> Sonuçları Kıyaslama değildir. Bkz: [bu makaledeki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-article).
> 
> 

Toplu işleme gelen performans artışı hemen açıktır. Önceki sıralı testte 1000 işlem 129 saniye veri merkezinin dışında ve veri merkezi içinde 21 saniyeden uzun sürdü. Ancak, tablo değerli parametreleri ile 1000 işlem yalnızca 2.6 saniye veri merkezinin dışında ve veri merkezi içinde 0,4 saniye sürebilir.

Tablo değerli parametreler hakkında daha fazla bilgi için bkz. [Table-Valued parametreleri](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>SQL toplu kopyalama

SQL toplu kopyalama büyük miktarlarda verinin bir hedef veritabanına eklemek için başka bir yoludur. .NET uygulamalarında kullanabileceğiniz **SqlBulkCopy** sınıfı toplu gerçekleştirmek için işlemler Ekle. **SqlBulkCopy** için komut satırı aracı, işlevde benzer **Bcp.exe**, ya da Transact-SQL deyimini **BULK INSERT**. Aşağıdaki kod örneği nasıl toplu kopyalama kaynak satırları gösterir **DataTable**, SQL Server, hedef tabloda MyTable tablo.

```csharp
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
```

Toplu kopyalama tablo değerli parametreleri tercih edilen olduğu bazı durumlar vardır. Tablo değerli parametre toplu ekleme işlemleri makaledeki karşı karşılaştırma tablosuna bakın [Table-Valued parametreleri](https://msdn.microsoft.com/library/bb510489.aspx).

Aşağıdaki geçici test sonuçları ile toplu işleme performansını gösterir **SqlBulkCopy** milisaniye cinsinden.

| İşlemler | Şirket içinden azure'a (ms) | Aynı Azure veri merkezi (ms) |
| --- | --- | --- |
| 1 |433 |57 |
| 10 |441 |32 |
| 100 |636 |53 |
| 1000 |2535 |341 |
| 10000 |21605 |2737 |

> [!NOTE]
> Sonuçları Kıyaslama değildir. Bkz: [bu makaledeki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-article).
> 
> 

Daha küçük toplu boyutlarında kullanım tablo değerli parametresi'ü geride bırakmıştır **SqlBulkCopy** sınıfı. Ancak, **SqlBulkCopy** 1.000 ve 10.000 satırları testler için 12-%31 tablo değerli parametre sayısından daha hızlı gerçekleştirilir. Tablo değerli parametreleri gibi **SqlBulkCopy** işlem olmayan toplu performansı için özellikle karşılaştırıldığında toplu eklemeleri için iyi bir seçenek olan.

ADO.NET'te toplu kopyalama hakkında daha fazla bilgi için bkz. [SQL Server'da toplu kopyalama işlemleri](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Birden çok satır parametreli INSERT deyimleri

Bir küçük toplu işler için birden çok satır ekleyen büyük bir parametreli INSERT deyimini oluşturmak için alternatiftir. Aşağıdaki kod örneği, bu tekniği gösterir.

```csharp
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
```

Bu örnek, temel kavramı göstermeye yöneliktir. Daha gerçekçi bir senaryo, sorgu dizesi ve komut parametreleri aynı anda oluşturmak için gerekli varlıkları döngü. Bunu bu şekilde işlenebilecek satırların toplam sayısını sınırlar için toplam 2100 sorgu parametreleri için sınırlı olursunuz.

Aşağıdaki geçici test sonuçları, bu tür bir INSERT deyimini performansını milisaniye cinsinden gösterir.

| İşlemler | Tablo değerli parametreleri (ms) | Tek deyim Ekle (ms) |
| --- | --- | --- |
| 1 |32 |20 |
| 10 |30 |25 |
| 100 |33 |51 |

> [!NOTE]
> Sonuçları Kıyaslama değildir. Bkz: [bu makaledeki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-article).
> 
> 

Bu yaklaşım, 100'den daha az satır toplu işlemler için biraz daha hızlı olabilir. Geliştirme küçük olsa da, bu teknik, belirli bir uygulama senaryonuzda iyi çalışabilir başka bir seçenektir.

### <a name="dataadapter"></a>DataAdapter

**DataAdapter** sınıfı değiştirmenize olanak sağlayan bir **veri kümesi** nesnesi ve ardından değişiklikleri INSERT, UPDATE ve DELETE işlemleri olarak gönderin. Kullanıyorsanız **DataAdapter** bu şekilde, her distinct işlemi için ayrı çağrıları yapıldığını unutmayın. Performansı artırmak için kullanmak **UpdateBatchSize** özelliği için aynı anda toplu işlemlerin sayısı. Daha fazla bilgi için [toplu işlemleri kullanarak DataAdapters gerçekleştirme](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Varlık çerçevesi

Entity Framework, toplu işleme şu anda desteklemiyor. Farklı geliştiriciler Topluluğu'nda çalıştı geçersiz kılma gibi geçici çözümler göstermek **SaveChanges** yöntemi. Ancak çözümler genellikle karmaşık ve özelleştirilmiş uygulama ve veri modeli. Entity Framework codeplex projesi, şu anda bu özellik isteğinde tartışma sayfasında yok. Bu tartışmayı görüntülemek için bkz: [tasarım Toplantı Notları - 2 Ağustos 2012](https://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML

Eksiksiz olması için toplu bir strateji olarak XML bahsedeceğiz önemli olduğunu düşünüyor. Ancak, diğer yöntemleri hiçbir avantaj ve bazı olumsuz yanları XML kullanımı vardır. Tablo değerli parametrelere benzer bir yaklaşımdır, ancak kullanıcı tanımlı bir tablo yerine bir saklı yordam için geçirilen bir dize veya XML dosyası. Saklı yordam saklı yordamı komutlar ayrıştırır.

Bu yaklaşım bazı dezavantajları vardır:

* XML ile çalışma hantal olabilir ve hataya açık alanlardır.
* Veritabanında XML ayrıştırma, yoğun CPU kullanımına neden olabilir.
* Çoğu durumda, bu yöntem, tablo değerli parametre yavaştır.

Bu nedenlerle, toplu işlem sorguları için XML kullanılması önerilmez.

## <a name="batching-considerations"></a>Toplu işlem konuları

Aşağıdaki bölümler, SQL veritabanı uygulamalarında toplu işleme kullanmak için daha fazla rehberlik sağlar.

### <a name="tradeoffs"></a>Ödünler

Mimarinizi bağlı olarak, toplu işleme, performans ve esneklik arasında bir denge içerebilir. Örneğin, burada rolünüz beklenmedik bir şekilde arıza yaşanırsa senaryoyu düşünün. Bir satır veri kaybederseniz, daha büyük bir grup gönderilmeyen satır kaybetme etkisini daha küçük bir etkisidir. Belirtilen bir zaman penceresi veritabanına göndermeden satırlar arabellek olmadığında büyük sokması mümkündür.

Bu bir tradeoff nedeniyle, toplu işlem türü değerlendirin. Batch daha agresif bir biçimde (büyük toplu işler ve uzun zaman pencereleri) daha az kritik verilerle.

### <a name="batch-size"></a>Toplu işlem boyutu

Testlerimiz, genellikle daha küçük öbeklere büyük toplu işler bozucu herhangi bir avantaj sağlamaz vardı. Aslında, bu alt genellikle tek bir büyük toplu iş gönderme daha yavaş performans sonuçlandı. Örneğin, 1000 satırı eklemek istediğiniz bir senaryo düşünün. Daha küçük toplu işler bölündüğünde 1000 satırı eklemek için tablo değerli parametreleri kullanmak için ne kadar sürer, aşağıdaki tabloda gösterilmektedir.

| Toplu işlem boyutu | Yinelemeler | Tablo değerli parametreleri (ms) |
| --- | --- | --- |
| 1000 |1 |347 |
| 500 |2 |355 |
| 100 |10 |465 |
| 50 |20 |630 |

> [!NOTE]
> Sonuçları Kıyaslama değildir. Bkz: [bu makaledeki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-article).
> 
> 

Aynı anda göndermek için en iyi performans için 1000 satırı olduğunu görebilirsiniz. (Burada gösterilmiyor) diğer testlerinde 10000 satır batch 5000 iki yığınlara ayırmak için bir küçük bir performans kazancı vardı. Ancak, belirli veri ve bu bulgular doğrulamak için toplu iş boyutu testleri gerçekleştirmeniz gereken şekilde bu testler için tablo şemasını görece basit.

Dikkate alınması gereken diğer bir etken toplam toplu iş çok büyük olursa, SQL veritabanı azaltma ve toplu işleme şüphelenirse olmasıdır. En iyi sonuçlar için ideal bir toplu iş boyutu olup olmadığını belirlemek için kendi senaryonuza test edin. Toplu iş boyutu performans veya hataları göre hızlı ayarlamalarını etkinleştir zamanında yapılandırılabilir olun.

Son olarak, toplu iş boyutu toplu işlem ile ilişkili riskleri taşıyan dengeleyin. Geçici hatalar veya bir rolü başarısız olursa işlemin yeniden denenmesi veya toplu işlemdeki veri kaybı sonuçları göz önünde bulundurun.

### <a name="parallel-processing"></a>Paralel işleme

Ne toplu iş boyutu azaltma yaklaşımı benimsediğimiz ancak birden çok iş parçacığı işi yürütmek için kullanılır? Yeniden testlerimizin birkaç çok iş parçacıklı daha küçük toplu işler genellikle tek bir büyük toplu iş daha da kötüsü gerçekleştirdiği gösterdi. Aşağıdaki sınama, 1000 satırı bir veya daha fazla paralel toplu olarak eklemek çalışır. Bu test nasıl daha fazla eşzamanlı toplu işler gerçekten düşük performansla gösterir.

| Toplu iş boyutu [yinelemeler] | İki iş parçacığı (ms) | Dört iş parçacığı (ms) | Altı iş parçacıkları (ms) |
| --- | --- | --- | --- |
| 1000 [1] |277 |315 |266 |
| 500 [2] |548 |278 |256 |
| 250 [4] |405 |329 |265 |
| 100 [10] |488 |439 |391 |

> [!NOTE]
> Sonuçları Kıyaslama değildir. Bkz: [bu makaledeki zamanlama sonuçlarıyla ilgili Not](#note-about-timing-results-in-this-article).
> 
> 

Paralellik nedeniyle performans düşüşü için birkaç olası nedeni vardır:

* Bir yerine birden çok eşzamanlı ağ çağrıları vardır.
* Birden çok işlem tek bir tabloda Çekişme ve engelleme neden olabilir.
* İle ilişkili ek yüklerini vardır çoklu iş parçacığı kullanımı.
* Birden çok bağlantı açma gider paralel işleme avantajı ağır.

Farklı tablolar veya veritabanlarına hedefliyorsanız, bu strateji ile elde bazı performansını görmek mümkündür. Bir senaryo için bu yaklaşım, veritabanı parçalama veya federasyonlar olacaktır. Parçalama, birden çok veritabanını kullanır ve her bir veritabanı için farklı veri yönlendirir. Her küçük bir toplu iş farklı bir veritabanına olacaksa, paralel olarak ardından işlemleri daha etkili olabilir. Ancak, performans kazanç veritabanı parçalama çözümünüzde kullanmak için karar temeli olarak kullanmak önemli değildir.

Bazı tasarımlarında daha küçük toplu işler paralel yürütülmesi istekleri iyi aktarım hızı yük altında bir sistem sonuçlanabilir. Tek bir büyük toplu iş işlem daha hızlı bir şekilde olsa da, bu durumda, paralel olarak birden çok toplu iş işlem daha verimli olabilir.

Paralel yürütme kullanıyorsanız, en fazla çalışan iş parçacığı sayısını denetleme göz önünde bulundurun. Daha küçük bir sayı daha az çekişme ve daha hızlı bir yürütme süresi neden. Ayrıca, bu hedef veritabanı bağlantıları ve işlemleri yerleştirir ek yükü göz önünde bulundurun.

### <a name="related-performance-factors"></a>İlgili performans etmenleri

Toplu işleme, veritabanı performansı normal yönergeler de etkiler. Örneğin, performans, büyük bir birincil anahtar veya birçok kümelenmemiş dizinler içeren tablolar için azaltılır.

Tablo değerli parametre bir saklı yordam kullanıyorsanız, komut kullanabilirsiniz **SET NOCOUNT ON** yordamın başında. Bu deyim dönüş yordamda etkilenen satır sayısı bastırır. Ancak kullanımını testlerimizin içinde **SET NOCOUNT ON** etkiye sahip ya da performansı düşebilir. Test saklı yordamı ile tek bir basit **Ekle** tablo değerli parametre komutu. Daha karmaşık saklı yordamlar bu deyimden avantaj elde edecektir mümkündür. Ancak bu ekleme varsaymayın **SET NOCOUNT ON** depolanmış yordamınızdaki otomatik olarak performansı artırır. Etkisini anlamak için depolanmış yordamınızdaki olmadan test **SET NOCOUNT ON** deyimi.

## <a name="batching-scenarios"></a>Toplu işleme senaryoları

Aşağıdaki bölümlerde, üç uygulama senaryolarında tablo değerli parametreleri kullanma açıklanmaktadır. İlk senaryo, arabelleğe alma ve yığınlama birlikte nasıl çalışabileceğini gösterir. İkinci senaryo, bir tek bir saklı yordam çağrısında ana öğe-ayrıntı işlemleri gerçekleştirerek performansını artırır. Son senaryo, bir "UPSERT" işleminde tablo değerli parametre kullanmayı gösterir.

### <a name="buffering"></a>arabelleğe alma

Toplu işleme için belirgin aday olan bazı senaryolarda olsa da, geciken işlem tarafından toplu işleme avantajı sürebilir birçok senaryo vardır. Ancak, Gecikmeli işleme ayrıca veri beklenmeyen bir hata durumunda büyük bir risk taşır. Bu riskleri öğrenmek ve sonuçlara göz önünde bulundurun önemlidir.

Örneğin, her kullanıcı Gezinti geçmişini izleyen bir web uygulamasını göz önünde bulundurun. Her sayfa isteğinde uygulama bir veritabanı kullanıcının sayfa görünümü kaydetmek için arama yapabilirsiniz. Ancak, kullanıcı Gezinti etkinlikleri arabelleğe alma ve sonra da bu verileri toplu işlemleri veritabanına göndererek daha yüksek performans ve ölçeklenebilirlik gerçekleştirilebilir. Geçen süre ve/veya arabellek boyutu ile veritabanı güncelleştirme tetikleyebilirsiniz. Örneğin, bir kural arabellek 1000 öğe ulaştığında veya 20 saniye sonra batch işleneceğini belirtebilirsiniz.

Aşağıdaki kod örneğinde [Reactive Extensions - Rx](https://msdn.microsoft.com/data/gg577609) bir izleme sınıfı tarafından oluşturulan arabelleğe alınan olayları işlemek için. Arabellek doldururken veya zaman aşımına ulaşıldığı, batch, kullanıcı verilerinin tablo değerli bir parametre ile veritabanına gönderilir.

Kullanıcı Gezinti ayrıntıları aşağıdaki NavHistoryData sınıfı modeller. Kullanıcı tanımlayıcısı, erişilen URL'si ve erişim zamanı gibi temel bilgileri içerir.

```csharp
public class NavHistoryData
{
    public NavHistoryData(int userId, string url, DateTime accessTime)
    { UserId = userId; URL = url; AccessTime = accessTime; }
    public int UserId { get; set; }
    public string URL { get; set; }
    public DateTime AccessTime { get; set; }
}
```

Kullanıcı Gezinti verilerini veritabanına ara belleğe alma için sorumlu NavHistoryDataMonitor sınıftır. Bir yöntem yükselterek yanıt veren RecordUserNavigationEntry içerdiği bir **OnAdded** olay. Aşağıdaki kod Rx olayı temel alan gözlemlenebilir bir koleksiyon oluşturmak için kullandığı Oluşturucu mantığı gösterir. Ardından arabellek yöntemiyle gözlemlenebilir bu koleksiyona abone. Aşırı yükleme, arabellek her 20 saniyede veya 1000 girişleri olarak gönderilmesi gerektiğini belirtir.

```csharp
public NavHistoryDataMonitor()
{
    var observableData =
        Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

    observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);
}
```

İşleyici arabelleğe alınan öğelerin tümünü bir tablo değerli türüne dönüştürür ve ardından bu toplu işler bir saklı yordam geçirir. Aşağıdaki kod NavHistoryDataEventArgs hem NavHistoryDataMonitor sınıfları için eksiksiz tanımını gösterir.

```csharp
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
```

İşleyici arabelleğe alınan öğelerin tümünü bir tablo değerli türüne dönüştürür ve ardından bu toplu işler bir saklı yordam geçirir. Aşağıdaki kod NavHistoryDataEventArgs hem NavHistoryDataMonitor sınıfları için eksiksiz tanımını gösterir.

```csharp
    public class NavHistoryDataEventArgs : System.EventArgs
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
```

Arabelleğe alma Bu sınıf kullanmak için uygulama bir statik NavHistoryDataMonitor nesnesi oluşturur. Bir kullanıcı bir sayfa her eriştiğinde uygulama NavHistoryDataMonitor.RecordUserNavigationEntry yöntemini çağırır. Bu girişler toplu işlemleri veritabanına gönderme halletmeniz için arabelleğe alma mantığını ilerler.

### <a name="master-detail"></a>Ana ayrıntı

Tablo değerli parametreleri basit ekleme senaryoları için yararlıdır. Ancak, birden fazla tablo içeren toplu ekleme daha zor olabilir. "Ana/ayrıntı" senaryo iyi bir örnektir. Ana Tablo birincil varlık tanımlar. Bir veya daha fazla ayrıntı tabloları varlık hakkında daha fazla veri depolar. Bu senaryoda, yabancı anahtar ilişkileri benzersiz bir ana varlık ayrıntıları arasındaki ilişkiyi uygular. Basitleştirilmiş bir sürümünü PurchaseOrder tablo ve onun ilişkili OrderDetail tablosu göz önünde bulundurun. Aşağıdaki Transact-SQL ile dört sütun PurchaseOrder tablo oluşturur: OrderID, OrderDate, Müşteri Kimliği ve durumu.

```sql
CREATE TABLE [dbo].[PurchaseOrder](
[OrderID] [int] IDENTITY(1,1) NOT NULL,
[OrderDate] [datetime] NOT NULL,
[CustomerID] [int] NOT NULL,
[Status] [nvarchar](50) NOT NULL,
CONSTRAINT [PrimaryKey_PurchaseOrder] 
PRIMARY KEY CLUSTERED ( [OrderID] ASC ))
```

Her bir order bir veya daha fazla ürün satın alma işlemleri içerir. Bu bilgiler PurchaseOrderDetail tabloda yakalanır. Aşağıdaki Transact-SQL ile beş sütun PurchaseOrderDetail tablo oluşturur: OrderID, OrderDetailID, ProductID, UnitPrice ve OrderQty.

```sql
CREATE TABLE [dbo].[PurchaseOrderDetail](
[OrderID] [int] NOT NULL,
[OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
[ProductID] [int] NOT NULL,
[UnitPrice] [money] NULL,
[OrderQty] [smallint] NULL,
CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
( [OrderID] ASC, [OrderDetailID] ASC ))
```

OrderID PurchaseOrderDetail tablosundaki bir sipariş PurchaseOrder tablosundan başvurmalıdır. Yabancı anahtar aşağıdaki tanımını bu kısıtlamayı zorlar.

```sql
ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
REFERENCES [dbo].[PurchaseOrder] ([OrderID])
```

Tablo değerli parametreleri kullanmak için her hedef tablosu için bir kullanıcı tanımlı tablo türü olması gerekir.

```sql
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
```

Ardından bu tür tablo kabul eden bir saklı yordam tanımlayın. Bu yordamı, bir uygulamanın yerel olarak bir dizi siparişlerini ve tek bir çağrı sırası ayrıntılarını toplu sağlar. Aşağıdaki Transact-SQL bu satınalma siparişi örneği için tam bir saklı yordam bildirimi sağlar.

```sql
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
```

Bu örnekte, yerel olarak tanımlanan @IdentityLink tablo, yeni eklenen satırlar gerçek OrderID değerleri depolar. Bu sipariş tanımlayıcılarını geçici OrderID değerleri farklı @orders ve @details tablo değerli parametreleri. Bu nedenle, @IdentityLink tablo ardından OrderID değerlerinden bağlanır @orders PurchaseOrder tablosunda yeni satırlar için gerçek OrderID değerler için parametre. Bu adımdan sonra @IdentityLink tablo kolaylaştırmak yabancı anahtar kısıtlamasını karşılayan gerçek OrderID ile sipariş ayrıntılarını ekleme.

Bu saklı yordamı, kod veya diğer Transact-SQL çağrıları kullanılabilir. Bu belgede bir kod örneği için tablo değerli parametre bölümüne bakın. Aşağıdaki Transact-SQL sp_InsertOrdersBatch çağırma gösterilmektedir.

```sql
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
```

Bu çözüm, her toplu iş 1'den başlar OrderID değerler kümesini kullanmak izin verir. Bu geçici OrderID değerleri batch ilişkileri tanımlamak, ancak gerçek OrderID değerler ekleme işlemi zamanında belirlenir. Önceki örnekte, tekrar tekrar aynı deyimleri çalıştırın ve veritabanında benzersiz siparişler oluşturmak. Bu nedenle, bu tekniği toplu işlem kullanırken yinelenen siparişler engeller. daha fazla kod veya veritabanı mantığı eklemeyi düşünün.

Bu örnek, tablo değerli parametreleri kullanarak ana öğe-ayrıntı işlemleri gibi daha karmaşık veritabanı işlemleri gruplanabilecek gösterir.

### <a name="upsert"></a>UPSERT

Başka bir toplu işleme senaryosu, aynı anda var olan satır, ekleme yeni satırları güncelleştirme içerir. Bu işlem, bazen bir "UPSERT" (güncelleştirme + INSERT) işlem olarak da adlandırılır. EKLEME ve güncelleştirme için ayrı çağrılar yapmak yerine, MERGE deyiminin bu görev için idealdir. MERGE deyiminin her iki INSERT işlemi ve güncelleştirme işlemlerinin tek bir çağrı kullanabilirsiniz.

Tablo değerli parametre ile MERGE deyimi, güncelleştirmeler ve ekler gerçekleştirmek için kullanılabilir. Örneğin, şu sütunları içeren bir Basitleştirilmiş çalışan tablosuna göz önünde bulundurun: EmployeeID, FirstName, LastName, SocialSecurityNumber:

```sql
CREATE TABLE [dbo].[Employee](
[EmployeeID] [int] IDENTITY(1,1) NOT NULL,
[FirstName] [nvarchar](50) NOT NULL,
[LastName] [nvarchar](50) NOT NULL,
[SocialSecurityNumber] [nvarchar](50) NOT NULL,
CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
([EmployeeID] ASC ))
```

Bu örnekte, SocialSecurityNumber birden çok çalışan bir birleştirme işlemini gerçekleştirmek için benzersiz olduğunu kullanabilirsiniz. İlk olarak, kullanıcı tanımlı tablo türü oluşturun:

```sql
CREATE TYPE EmployeeTableType AS TABLE 
( Employee_ID INT,
    FirstName NVARCHAR(50),
    LastName NVARCHAR(50),
    SocialSecurityNumber NVARCHAR(50) );
GO
```

Ardından, bir saklı yordam oluşturma veya güncelleştirme gerçekleştirmek ve eklemek için MERGE deyiminin kullanan kod yazın. Aşağıdaki örnek, bir tablo değerli parametre MERGE deyiminin kullanır @employees, EmployeeTableType türü. İçeriğini @employees tablo burada gösterilmez.

```sql
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
```

Belgeler ve örnekler MERGE deyiminin için daha fazla bilgi için bkz. Aynı iş bir çok-depolanan adımda gerçekleştirilebilir ancak yordam çağrısı ile ayrı ekleme ve güncelleştirme işlemleri, MERGE deyiminin daha verimlidir. Veritabanı kod MERGE deyimi, INSERT ve UPDATE için iki veritabanı çağrıları doğrudan gerektirmeden kullanan Transact-SQL çağrıları da oluşturabilirsiniz.

## <a name="recommendation-summary"></a>Öneri özeti

Aşağıdaki listede, bu makalede ele alınan toplu işlem önerilerin bir özeti verilmiştir:

* SQL veritabanı uygulamalarının ölçeklenebilirliğini ve performansı artırmak için arabelleğe alma ve toplu işlem'i kullanın.
* Toplu işleme/arabelleğe alma ve esneklik arasında dengelerin anlayın. Bir rol hata sırasında işlenmemiş bir toplu iş açısından kritik veri kaybetme riski toplu işleme performans avantajı gölgede bırakabilir.
* Veritabanı tek bir veri merkezi içinde gecikme süresini azaltmak için tüm çağrıları korumak çalışır.
* Tablo değerli parametre, tek bir toplu işlem teknik seçerseniz, en iyi performans ve esneklik sunar.
* En hızlı ekleme performansı için aşağıdaki genel yönergeleri izleyin ancak senaryonuza test:
  * < 100 satır için tek bir parametreli Ekle komutu kullanın.
  * < 1000 satırı için tablo değerli parametreleri kullanır.
  * İçin > = 1000 satır SqlBulkCopy kullanın.
* İçin güncelleştirme ve silme işlemleri, her satırda bir tablo parametresi doğru işlemi belirler saklı yordam mantıksal tablo değerli parametreleri kullanın.
* Toplu iş boyutu yönergeleri:
  * Uygulama ve iş gereksinimleri için anlamlı en büyük toplu iş boyutu kullanılır.
  * Geçici ya da yıkıcı hataları risklerini ile büyük toplu işlemlerin performans kazancı dengeleyin. Yeniden deneme adımlamayla veya toplu işlemdeki veri kaybı nedir? 
  * SQL veritabanı da işlemi reddedebilir değil olduğunu doğrulamak için en büyük toplu iş boyutu test edin.
  * Yapılandırma ayarları, bu denetim, toplu iş boyutu veya arabelleğe alma zaman penceresi gibi toplu işleme oluşturun. Bu ayarlar, esneklik sağlar. Bulut hizmetine yeniden dağıtmaya gerek kalmadan üretim toplu işleme davranışını değiştirebilirsiniz.
* Bir veritabanında tek bir tablo üzerinde çalışacağı toplu işlemlerin Paralel yürütme kaçının. Tek bir toplu iş birden fazla çalışan iş parçacıkları arasında bölmek seçerseniz, ideal iş parçacığı sayısını belirlemek için testleri çalıştırın. Belirtilmeyen bir eşik sonra daha fazla iş parçacığı performansını düşürebilir yerine artırmak.
* Boyut ve daha fazla senaryo için toplu işleme uygulamanın yolu sürede arabelleğe almayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede nasıl veritabanı tasarımı ve kodlama tekniklerini toplu işleme için ilgili uygulama performansı ve ölçeklenebilirliği artırabilir üzerinde odaklanır. Ancak, yalnızca bir faktör olarak genel stratejinizi budur. Performans ve ölçeklenebilirliği artırmak daha fazla yolu için bkz: [tek veritabanları için Azure SQL Database performans rehberi](sql-database-performance-guidance.md) ve [elastik havuzlar için fiyat ve performans hususları](sql-database-elastic-pool-guidance.md).

