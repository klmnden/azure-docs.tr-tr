---
title: RLS ve esnek veritabanı araçları ile çok kiracılı uygulamalar | Microsoft Docs
description: Esnek veritabanı araçlarını satır düzeyi güvenlik ile yüksek oranda ölçeklenebilir bir veri katmanı ile bir uygulama oluşturmak için kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: sstein
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 4834688496330210b273f40f1d6f11230a6ae1c8
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66234121"
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Esnek veritabanı araçlarını ve satır düzeyi güvenlik ile çok kiracılı uygulamalar

[Esnek veritabanı araçlarını](sql-database-elastic-scale-get-started.md) ve [satır düzeyi güvenlik (RLS)] [ rls] Azure SQL veritabanı ile çok kiracılı bir uygulama için veri katmanını ölçeklendirmeyi etkinleştirmek üzere işbirliği yapar. Birlikte bu teknolojiler bir yüksek oranda ölçeklenebilir bir veri katmanı olan bir uygulama oluşturmanıza yardımcı olur. Veri katmanı, çok kiracılı parçaları destekleyen ve kullandığı **ADO.NET SqlClient** veya **Entity Framework**. Daha fazla bilgi için [Azure SQL veritabanı ile çok kiracılı SaaS uygulamaları için Tasarım desenleri](saas-tenancy-app-design-patterns.md).

- **Esnek veritabanı araçlarını** geliştiricilerin .NET kitaplıkları ve Azure hizmet şablonları kullanarak veri katmanı ile standart parçalara ayırma uygulamaları, ölçeği genişletme. Parçalar kullanarak yönetme [elastik veritabanı istemci Kitaplığı] [ s-d-elastic-database-client-library] otomatikleştirmek ve genellikle parçalama yöntemiyle ilişkili altyapısal görevlerin çoğunu kolaylaştırmaya yardımcı olur.
- **Satır düzeyi güvenlik** geliştiricilerin güvenli bir şekilde aynı veritabanının birden fazla Kiracı için veri depolama sağlar. Sorguyu yürüten kiracıya ait olmayan satırları RLS güvenlik ilkeleri filtreleyin. Veritabanı içinde filtre mantığının merkezileştirerek, bakım basitleştirir ve bir güvenlik hatası riskini azaltır. Güvenliği zorlamak için tüm istemci kodu bağlı olan diğer risklidir.

Bu özellikler birlikte kullanarak, bir uygulama, aynı parça veritabanının birden fazla Kiracı için veri depolayabilir. Kiracılar bir veritabanını paylaştığında daha az Kiracı başına maliyeti. Henüz aynı uygulama için adanmış kendi tek kiracılı parça ödeme seçeneği ayrıca, premium kiracılar sunabilir. Tek tek kiracılı yalıtım firmer performans garantileri avantajdır. Bir tek kiracılı veritabanında kaynakları için rekabete hiçbir Kiracı yoktur.

Elastik veritabanı istemci kitaplığını kullanma olmaktır [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) otomatik olarak her bir kiracı doğru parça veritabanına bağlanmak için API'leri. Tek parça belirli kiracısı için belirli Tenantıd değerini içerir. Tenantıd olduğu *parçalama anahtarı*. Bağlantı kurulduktan sonra bir veritabanı içinde RLS güvenlik ilkesi belirli kiracısı, Tenantıd içeren veri satırları erişebilmesini sağlar.

> [!NOTE]
> Kiracı tanımlayıcısı, birden fazla sütunun oluşabilir. Kolaylık bu tartışma için basit bir tek sütunlu Tenantıd varsayıyoruz.

![Uygulama Mimarisi Web günlüğü][1]

## <a name="download-the-sample-project"></a>Örnek projeyi indirin

### <a name="prerequisites"></a>Önkoşullar

- Visual Studio (2012 veya üzeri) kullanın
- Üç Azure SQL veritabanı oluşturma
- Örnek projeyi indirin: [Azure SQL - çok Kiracılı parça için elastik veritabanı araçları](https://go.microsoft.com/?linkid=9888163)
  - Veritabanlarınızı başındaki bilgileri doldurun **Program.cs**

Bu proje bir genişletir [Azure SQL - Entity Framework tümleştirmesi için elastik veritabanı araçları](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) çok kiracılı parça veritabanlarına yönelik destek ekleyerek. Blog ve gönderi oluşturmaya yönelik basit bir konsol uygulaması projesi oluşturur. Proje, dört Kiracı yanı sıra, iki çok kiracılı parça veritabanı içerir. Bu yapılandırma, önceki şemada gösterilmiştir.

Derleme ve uygulamayı çalıştırın. Bu çalıştırmada esnek veritabanı araçlarını parça eşleme Yöneticisi bootstraps ve aşağıdaki testler gerçekleştirir:

1. Entity Framework ve LINQ kullanarak yeni bir blog oluşturma ve ardından her Kiracı için tüm Web günlüklerini görüntüleme
2. ADO.NET SqlClient kullanarak, bir kiracı için tüm blogları görüntüleme
3. Bir hata oluşturulur doğrulamak yanlış kiracının blog eklemeyi deneyin

RLS parça veritabanlarında henüz etkinleştirilmemiş olduğundan, bu testler, her bir sorun ortaya dikkat edin: kiracılar kendilerine ait olmayan blogları görebilirsiniz ve uygulama için yanlış kiracıya eklemesi blog engellenmez. Bu makalenin geri kalanında, Kiracı yalıtımı ile RLS zorunlu tutarak bu sorunları gidermek açıklar. İki adımı vardır:

1. **Uygulama katmanı**: Her zaman geçerli Tenantıd OTURUMU ayarlamak için uygulama kodunu değiştirmeniz\_bir bağlantı açmak sonra BAĞLAMI. Örnek Proje, Tenantıd zaten bu şekilde ayarlar.
2. **Veri katmanı**: Her parça veritabanında depolanan OTURUMUNDA Tenantıd göre filtre satırları bir RLS güvenlik ilkesi oluşturma\_BAĞLAMI. Her parça veritabanlarınızın bir ilke oluşturun, aksi takdirde çok kiracılı parça satırlarda olduğu tablolarda filtreleme yapılmaz.

## <a name="1-application-tier-set-tenantid-in-the-sessioncontext"></a>1. Uygulama katmanı: Tenantıd OTURUMU ayarlamak\_BAĞLAMI

Önce bir parça veritabanı için elastik veritabanı istemci Kitaplığı'nın verilere bağımlı yönlendirme API'lerini kullanarak bağlanın. Uygulamanın hangi Tenantıd hala veritabanı söylemelisiniz bağlantı kullanıyor. Tenantıd, hangi satırların diğer kiracılara ait olarak filtrelendi gerekir, RLS güvenlik ilkesi söyler. Geçerli bir Tenantıd içinde Store [OTURUMU\_bağlam](https://docs.microsoft.com/sql/t-sql/functions/session-context-transact-sql) bağlantı.

Alternatif oturum\_bağlam kullanmaktır [bağlam\_bilgisi](https://docs.microsoft.com/sql/t-sql/functions/context-info-transact-sql). Ancak OTURUMU\_bağlam daha iyi bir seçenektir. OTURUM\_bağlamıdır kullanımı daha kolay, varsayılan olarak NULL döndürür ve anahtar-değer çiftleri destekler.

### <a name="entity-framework"></a>Varlık Çerçevesi

Entity Framework kullanan uygulamalar için OTURUMU için kolay bir yaklaşım olan\_BAĞLAMI içinde açıklanan ElasticScaleContext geçersiz kılma [verilere bağımlı yönlendirme EF DbContext kullanma](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Oluşturma ve oturum sırasında Kiracı kimliği ayarlayan SqlCommand yürütme\_bağlantı için belirtilen shardingKey için bağlam. Verilere bağımlı Yönlendirme aracılığıyla aracılı bağlantı ardından dönün. Bu şekilde, yalnızca kod kez OTURUMU için yazmanız gereken\_BAĞLAMI.

```csharp
// ElasticScaleContext.cs
// Constructor for data-dependent routing.
// This call opens a validated connection that is routed to the
// proper shard by the shard map manager.
// Note that the base class constructor call fails for an open connection
// if migrations need to be done and SQL credentials are used.
// This is the reason for the separation of constructors.
// ...
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(
        OpenDDRConnection(shardMap, shardingKey, connectionStr),
        true)  // contextOwnsConnection
{
}

public static SqlConnection OpenDDRConnection(
    ShardMap shardMap,
    T shardingKey,
    string connectionStr)
{
    // No initialization.
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key.
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(
            shardingKey,
            connectionStr,
            ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey
        // to enable Row-Level Security filtering.
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText =
            @"exec sp_set_session_context
                @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }
        throw;
    }
}
// ...
```

Şimdi OTURUMU\_ElasticScaleContext çağrılan her BAĞLAMINI belirtilen Tenantıd ile otomatik olarak ayarlayın:

```csharp
// Program.cs
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (var db = new ElasticScaleContext<int>(
        sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;

        Console.WriteLine("All blogs for TenantId {0}:", tenantId);
        foreach (var item in query)
        {
            Console.WriteLine(item.Name);
        }
    }
});
```

### <a name="adonet-sqlclient"></a>ADO.NET SqlClient

ADO.NET SqlClient kullanan uygulamalar için yöntemi ShardMap.OpenConnectionForKey çevresinde bir sarmalayıcı işlevi oluşturun. Otomatik olarak Tenantıd OTURUMDA ayarlanan sarmalayıcı üretilmesini\_bağlantı döndürmeden önce geçerli bir Tenantıd için bağlam. Emin olmak için OTURUMU\_içerik her zaman ayarlanır, bu sarmalayıcı işlevi kullanarak bağlantıları yalnızca açmanız gerekir.

```csharp
// Program.cs
// Wrapper function for ShardMap.OpenConnectionForKey() that
// automatically sets SESSION_CONTEXT with the correct
// tenantId before returning a connection.
// As a best practice, you should only open connections using this method
// to ensure that SESSION_CONTEXT is always set before executing a query.
// ...
public static SqlConnection OpenConnectionForTenant(
    ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key.
        conn = shardMap.OpenConnectionForKey(
            tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey
        // to enable Row-Level Security filtering.
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText =
            @"exec sp_set_session_context
                @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }
        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient.
// If row-level security is enabled, only Tenant 4's blogs are listed.
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(
        sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine(@"--
All blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);

        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="2-data-tier-create-row-level-security-policy"></a>2. Veri katmanı: Satır düzeyi güvenlik ilkesi oluşturma

### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a>Her Kiracı erişebileceği satırlar filtrelemek için bir güvenlik ilkesi oluşturma

Uygulamanın, oturum ayarı göre\_sorgulama önce geçerli bir Tenantıd BAĞLAMIYLA bir RLS güvenlik ilkesi sorgular ve farklı bir Tenantıd dışlama satırları filtreleyebilirsiniz.

RLS, Transact-SQL uygulanır. Bir kullanıcı tanımlı işlev erişim mantığı tanımlar ve bir güvenlik ilkesi bu işlev tablolar herhangi bir sayıda bağlar. Bu proje için:

1. İşlev uygulaması veritabanına bağlıysa ve Tenantıd OTURUMDA depolanan doğrular\_bağlam belirli bir satırın Tenantıd eşleşir.
    - Uygulama, başka bir SQL kullanıcı yerine bağlıdır.

2. Bir FİLTRE koşulu seçin, GÜNCELLEŞTİRMEYİ geçmesine Tenantıd filtre karşılayan ve sorguları silmek satırları sağlar.
    - Bir ENGELLEME koşuluna eklenen veya güncelleştirilen engeller filtre başarısız satırları engeller.
    - Varsa oturum\_görünür veya eklenmesi için hiçbir satır BAĞLAMI ayarlanmamış ve işlev NULL döndürür.

Tüm parçalar üzerinde RLS'yi etkinleştirmek için Visual Studio (SSDT), SSMS veya projeye dahil PowerShell betiğini kullanarak aşağıdaki T-SQL yürütme. Veya kullanıyorsanız [elastik veritabanı işleri](elastic-jobs-overview.md), tüm parçalar üzerinde bu T-SQL yürütülmesi otomatik hale getirebilirsiniz.

```sql
CREATE SCHEMA rls; -- Separate schema to organize RLS objects.
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult
        -- Use the user in your application’s connection string.
        -- Here we use 'dbo' only for demo purposes!
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo')
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId;
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK  PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK  PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts;
GO
```

> [!TIP]
> Koşul tabloları yüzlerce üzerinde eklemeniz gerekebilir karmaşık bir projede, can sıkıcı olabilir. Otomatik olarak bir güvenlik ilkesi oluşturur ve bir koşul, bir şema tüm tablolarda ekler Yardımcısı depolanan yordam yoktur. Daha fazla bilgi için blog gönderisine bakın [geçerli satır düzeyi güvenlik tüm tablolara - yardımcı betik (blog)](https://blogs.msdn.com/b/sqlsecurity/archive/20../../apply-row-level-security-to-all-tables-helper-script).

Artık örnek uygulamayı yeniden çalıştırırsanız, kiracılar yalnızca kendisine ait satırları görebilirsiniz. Ayrıca, uygulama şu anda parça veritabanına bağlı diğer kiracılara ait satır eklenemiyor. Ayrıca, uygulamayı görmek herhangi bir satır içinde Tenantıd güncelleştirilemiyor. Uygulamanın aşağıdakilerden çalışırsa, bir DbUpdateException ortaya çıkar.

Daha sonra yeni bir tablo eklerseniz, yeni tabloda FİLTRE ve BLOK koşullar eklemek için güvenlik ilkesi DEĞİŞTİRİR.

```sql
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK  PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable;
GO
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a>Otomatik olarak Tenantıd için ekler doldurulması için varsayılan kısıtlama Ekle

Tenantıd OTURUMDA şu anda depolanan değeri otomatik olarak doldurmak üzere her bir tabloda bir default kısıtlaması koyabilirsiniz\_satırları eklerken BAĞLAMI. Bir örnek aşağıda verilmiştir.

```sql
-- Create default constraints to auto-populate TenantId with the
-- value of SESSION_CONTEXT for inserts.
ALTER TABLE Blogs
    ADD CONSTRAINT df_TenantId_Blogs
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId;
GO

ALTER TABLE Posts
    ADD CONSTRAINT df_TenantId_Posts
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId;
GO
```

Şimdi uygulamayı satır ekleme yaparken bir Tenantıd belirtmek gerekmez:

```csharp
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (var db = new ElasticScaleContext<int>(
        sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        // The default constraint sets TenantId automatically!
        var blog = new Blog { Name = name };
        db.Blogs.Add(blog);
        db.SaveChanges();
    }
});
```

> [!NOTE]
> Bir Entity Framework projesi için varsayılan kısıtlamalar kullanmanız önerilir, *değil* EF veri modelinizde Tenantıd sütun içerir. Entity Framework kullanma OTURUMU T-SQL ile oluşturulan varsayılan kısıtlamaları geçersiz kılma kaynağı varsayılan değerleri otomatik olarak sorgular çünkü bu öneriyi\_BAĞLAMI.
> Varsayılan kısıtlamalar örnek projesinde kullanmak için örneği için Tenantıd DataClasses.cs (ve çalışma Ekle-Paket Yöneticisi konsolunda geçiş) kaldırın ve gerekir alanı yalnızca veritabanı tablolarında var olduğundan emin olmak için T-SQL kullanın. Bu şekilde EF otomatik olarak hatalı varsayılan değerler veri girilirken sağlayın.

### <a name="optional-enable-a-superuser-to-access-all-rows"></a>(İsteğe bağlı) Etkinleştirme bir *süper kullanıcı* tüm satırları erişmek için

Bazı uygulamalar oluşturmak istediğiniz bir *süper kullanıcı* olan tüm satırları erişebilir. Bir süper kullanıcı, tüm parçalar üzerinde tüm kiracılar genelinde raporlama sağlayabilir. Veya bir süper kullanıcı Kiracı satırlar arasında veritabanları taşınması parçalara ayırma-birleştirme işlemleri gerçekleştirebilir.

Bir süper kullanıcı etkinleştirmek için yeni bir SQL kullanıcısı oluşturun (`superuser` Bu örnekte) parça her bir veritabanındaki. Ardından bu kullanıcının tüm satırları erişmesine izin veren yeni bir koşul işlevi güvenlik ilkesiyle değiştirir. Böyle bir işlevi sonraki verilir.

```sql
-- New predicate function that adds superuser logic.
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult
        WHERE
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- Replace 'dbo'.
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        )
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        );
GO

-- Atomically swap in the new predicate function on each table.
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK  PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK  PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts;
GO
```


### <a name="maintenance"></a>Bakım

- **Yeni parça ekleme**: Tüm yeni parçalar üzerinde RLS etkinleştirmek için T-SQL betiği yürütün, aksi takdirde bu parçalardaki sorgular olan tablolarda filtreleme yapılmaz.
- **Yeni tablo ekleme**: Yeni bir tablo oluşturulduğunda bir FİLTRE ve BLOK koşul tüm parçalar güvenlik ilkesine ekleyin. Aksi halde yeni bir tablo sorguları olan tablolarda filtreleme yapılmaz. Bu ekleme bir DDL tetikleyicisi kullanarak açıklandığı gibi otomatikleştirilebilir [yeni oluşturulan tablolar (blog) otomatik olarak satır düzeyi güvenlik uygulamak](https://blogs.msdn.com/b/sqlsecurity/archive/20../../apply-row-level-security-automatically-to-newly-created-tables.aspx).

## <a name="summary"></a>Özet

Esnek veritabanı araçlarını ve satır düzeyi güvenlik için ölçek genişletme bir uygulamasının veri katmanının hem çok kiracılı desteği ile birlikte kullanılır ve tek kiracılı parça olabilir. Çok kiracılı parça, verileri daha verimli bir şekilde depolamak için kullanılabilir. Bu verimlilik, çok sayıda kiracılar yalnızca birkaç satır veri olduğu belirgin olur. Tek kiracılı parça katı performans ve yalıtımı gereksinimlerine sahip premium kiracılar destekleyebilir. Daha fazla bilgi için [satır düzeyi güvenlik başvurusu][rls].

## <a name="additional-resources"></a>Ek kaynaklar

- [Azure elastik havuzu nedir?](sql-database-elastic-pool.md)
- [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md)
- [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](saas-tenancy-app-design-patterns.md)
- [Azure AD ve OpenID Connect kullanarak çok müşterili uygulamalarda kimlik doğrulama](../guidance/guidance-multitenant-identity-authenticate.md)
- [Tailspin Surveys uygulaması](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Sorular ve özellik istekleri

Sorular için bize ulaşın [SQL veritabanının Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Tüm özellik isteklerini ekleyin [SQL veritabanı geri bildirim Forumu](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/saas-tenancy-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->
[rls]: https://docs.microsoft.com/sql/relational-databases/security/row-level-security
[s-d-elastic-database-client-library]: sql-database-elastic-database-client-library.md
