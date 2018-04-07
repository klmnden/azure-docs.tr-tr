---
title: Çok kiracılı uygulamalarla RLS ve esnek veritabanı araçlarını | Microsoft Docs
description: Yüksek oranda ölçeklenebilir veri katmanı ile bir uygulama oluşturmak için satır düzeyi güvenlik ile esnek veritabanı araçlarını kullanın.
metakeywords: azure sql database elastic tools multi tenant row level security rls
services: sql-database
manager: craigg
author: tmullaney
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: thmullan
ms.openlocfilehash: 151a21a60b6205ca9a454faaaeff65330d9b57ec
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Çok kiracılı uygulamalarla esnek veritabanı araçlarını ve satır düzeyi güvenlik

[Esnek veritabanı araçlarını](sql-database-elastic-scale-get-started.md) ve [satır düzeyi güvenlik (RLS)] [ rls] Azure SQL veritabanı olan bir çok kiracılı uygulama veri katmanı ölçeklendirmeyi etkinleştirmek üzere işbirliği yapar. Birlikte bu teknolojiler, yüksek düzeyde ölçeklenebilir veri katmanı olan bir uygulama oluşturmanıza yardımcı olur. Veri katmanı çok kiracılı parça destekler ve kullandığı **ADO.NET SqlClient** veya **Entity Framework**. Daha fazla bilgi için bkz: [Azure SQL veritabanı ile çok kiracılı SaaS uygulamaları için Tasarım desenleri](saas-tenancy-app-design-patterns.md).

- **Esnek veritabanı araçlarını** .NET kitaplıkları ve Azure hizmet şablonlarını kullanarak veri katmanı standart parçalama uygulamalar ile genişletmek, geliştiricilerin. Parça kullanarak yönetme [esnek veritabanı istemci Kitaplığı] [ s-d-elastic-database-client-library] otomatikleştirmek ve genellikle parçalama ile ilişkili infrastructural görevlerin çoğunu akıcı hale getirmesine yardımcı olur.
- **Satır düzeyi güvenlik** güvenli bir şekilde aynı veritabanında birden çok Kiracı için verileri depolamak için geliştiricilere olanak sağlar. RLS güvenlik ilkeleri bir sorgu yürütülürken kiracıya ait değil satırları filtreler. Veritabanı içinde filtre mantığı merkezileştirme bakım basitleştirir ve bir güvenlik hatası riskini azaltır. Tüm istemci kodu enfore güvenlik güvenmek alternatif risklidir.

Bu özellikleri birlikte kullanarak, bir uygulama, aynı parça veritabanında birden çok Kiracı için veri depolayabilirsiniz. Kiracılar bir veritabanını paylaştığında daha az Kiracı başına maliyetleri. Henüz aynı uygulama için kendi özel tek Kiracı parça ödeme seçeneğiniz de premium kiracılar sunabilir. Tek Kiracı yalıtımı bir yararı firmer performans garanti ' dir. Bir tek Kiracı veritabanında kaynakları için rekabete diğer Kiracı yoktur.

Esnek veritabanı istemci kitaplığını kullanma hedeftir [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) otomatik olarak verilen her Kiracı doğru parça veritabanına bağlanmak için API'ler. Yalnızca bir parça belirli Tenantıd değerinin verilen bir kiracı için içerir. Tenantıd olan *parçalama anahtar*. Bağlantı kurulduktan sonra veritabanını bir RLS güvenlik ilkesinde belirtilen Kiracı kendi Tenantıd içeren veri satırları erişebilmesini sağlar.

> [!NOTE]
> Kiracı tanımlayıcısı birden fazla sütunu oluşabilir. Bu tartışma kolaylık sağlamak için basit bir tek sütunlu Tenantıd varsayıyoruz.

![Blog uygulama mimarisi][1]

## <a name="download-the-sample-project"></a>Örnek Proje yükle

### <a name="prerequisites"></a>Önkoşullar

- Visual Studio (2012 veya sonraki sürümlerini) kullanın 
- Üç Azure SQL veritabanı oluşturma 
- Örnek projesini indirin: [Azure SQL - çok Kiracılı parça için esnek veritabanı araçları](http://go.microsoft.com/?linkid=9888163)
  - Veritabanlarınızı başındaki bilgileri doldurun **Program.cs** 

Bu proje açıklandığı bir genişletir [Azure SQL - Entity Framework tümleştirme için esnek DB Araçları](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) çok kiracılı parça veritabanları için destek ekleyerek düzenleyin. Bloglar ve gönderileri oluşturmak için basit bir konsol uygulaması projesi oluşturur. Proje dört kiracılar yanı sıra, iki çok kiracılı parça veritabanları içeriyor. Bu yapılandırma önceki diyagramda gösterilmiştir. 

Derleme ve uygulamayı çalıştırın. Bu farklı çalıştır esnek veritabanı araçlarını parça eşleme Yöneticisi bootstraps ve aşağıdaki sınama gerçekleştirir: 

1. Entity Framework ve LINQ kullanarak yeni bir blog oluşturun ve ardından her bir kiracı için tüm Web Günlüklerini Görüntüle
2. ADO.NET SqlClient kullanarak, bir kiracı için tüm Web Günlüklerini Görüntüle
3. Bir hata atılır doğrulamak yanlış Kiracı için bir blog eklemeyi deneyin  

RLS parça veritabanlarında henüz etkinleştirilmemiş olduğundan, bu testler, her bir sorun ortaya dikkat edin: kiracılar kendilerine ait değil bloglar görmeye ve uygulama yanlış Kiracı için bir blog ekleme engellenemez. Bu makalenin sonraki bölümlerinde, Kiracı yalıtımı RLS ile zorlayarak bu sorunları gidermek açıklar. İki adımı vardır: 

1. **Uygulama katmanı**: her zaman geçerli Tenantıd OTURUMDA ayarlamak için uygulama kodu değiştirmeniz\_bir bağlantı açarak sonra BAĞLAMI. Örnek Proje Tenantıd zaten bu şekilde ayarlar. 
2. **Veri katmanı**: OTURUMUNDA depolanan Tenantıd göre filtre satırlara her parça veritabanındaki bir RLS güvenlik ilkesi oluşturma\_BAĞLAMI. Her bir parça veritabanı için bir ilke oluşturmak, aksi takdirde çok kiracılı parça satırlarda değil olması filtrelenir. 

## <a name="1-application-tier-set-tenantid-in-the-sessioncontext"></a>1. Uygulama katmanı: kümesi Tenantıd oturumunda\_BAĞLAMI

Önce bir parça veritabanına esnek veritabanı istemci kitaplığının veri bağımlı yönlendirme API'lerini kullanarak bağlanın. Uygulamayı hangi Tenantıd hala veritabanı söylemelisiniz bağlantı kullanıyor. Tenantıd hangi satırların diğer kiracılar ait olarak filtre olmalıdır RLS güvenlik ilkesi söyler. Geçerli Tenantıd depolamak [oturum\_BAĞLAMI](https://docs.microsoft.com/sql/t-sql/functions/session-context-transact-sql) bağlantı.

Alternatif oturum\_BAĞLAMI kullanmaktır [BAĞLAMI\_bilgisi](https://docs.microsoft.com/sql/t-sql/functions/context-info-transact-sql). Ancak oturum\_BAĞLAMI daha iyi bir seçenek değil. OTURUMU\_BAĞLAMI kullanmak daha kolay, varsayılan değer olarak NULL döndürür ve anahtar-değer çiftleri destekler.

### <a name="entity-framework"></a>Entity Framework

Entity Framework kullanan uygulamalar için en kolay yaklaşım oturum ayarlamaktır\_BAĞLAMI içinde açıklanan ElasticScaleContext geçersiz kılma [veri bağımlı EF DbContext kullanarak yönlendirme](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Oluşturma ve yürütme Tenantıd OTURUMDA ayarlar SqlCommand\_bağlantı için belirtilen shardingKey BAĞLAMI. Ardından verileri bağımlı Yönlendirme aracılığıyla aracılı bağlantı döndür. Bu şekilde, yalnızca kod bir kez oturum ayarlamak için yazmanız gereken\_BAĞLAMI. 

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

Şimdi oturum\_ElasticScaleContext çağrılan her BAĞLAMI ile belirtilen Tenantıd otomatik olarak ayarlayın: 

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

ADO.NET SqlClient kullanan uygulamalar için yöntemi ShardMap.OpenConnectionForKey çevresinde bir sarmalayıcı işlev oluşturun. Otomatik olarak Tenantıd OTURUMDA ayarlanan sarmalayıcı sahip\_bağlantı dönmeden önce geçerli Tenantıd BAĞLAMI. Emin olmak için OTURUMU\_BAĞLAMI Ayarla her zaman, yalnızca bu sarmalayıcı işlevi kullanarak bağlantı açmanız gerekir.

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

## <a name="2-data-tier-create-row-level-security-policy"></a>2. Veri katmanı: satır düzeyi güvenlik ilkesi oluşturma

### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a>Her Kiracı erişebileceği satırları filtrelemek için bir güvenlik ilkesi oluşturma

Uygulama oturumunu ayarlama göre\_sorgulama önce geçerli Tenantıd BAĞLAMIYLA RLS güvenlik ilkesi filtre sorguları ve farklı bir Tenantıd dışlama satırları uygulayabilirsiniz.  

RLS, Transact-SQL uygulanır. Kullanıcı tanımlı bir işlev erişim mantığı tanımlar ve bir güvenlik ilkesi bu işlev tabloları herhangi bir sayıda bağlar. Bu proje için:

1. Uygulama veritabanına bağlı olduğundan ve Tenantıd OTURUMDA depolanan işlevi doğrular\_BAĞLAMI belirli bir satırın Tenantıd eşleşir.
    - Uygulama, başka bir SQL kullanıcı yerine bağlandı.

2. Bir FİLTRE koşulu seçin, GÜNCELLEŞTİRMEYİ geçmesine Tenantıd filtre karşılamak ve sorguları silmek satırları sağlar.
    - Bir ENGELLEME koşuluna eklenen veya güncelleştirilen engeller filtre başarısız satırları engeller.
    - Varsa oturum\_görünür veya eklenmesi için hiçbir satır BAĞLAMI ayarlanmamış ve işlev NULL döndürür. 

RLS tüm parça üzerinde etkinleştirmek için aşağıdaki T-SQL Visual Studio (SSDT), SSMS veya projeye dahil PowerShell komut dosyası kullanarak yürütün. Veya kullanıyorsanız [esnek veritabanı iş](sql-database-elastic-jobs-overview.md), bu T-SQL tüm parça yürütülmesi otomatik hale getirebilirsiniz.

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
> Koşul tabloları yüzlerce üzerinde eklemeniz gerekebilir karmaşık bir projede hangi can sıkıcı olabilir. Otomatik olarak bir güvenlik ilkesi oluşturur ve bir şemadaki tüm tablolarda bir koşul ekler bir yardımcı depolanan yordamı yok. Konumundaki için blog daha fazla bilgi için bkz: [geçerli satır düzeyi güvenlik tüm tablolara - Yardımcısı komut dosyası (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).

Şimdi örnek uygulamayı yeniden çalıştırın, kiracılar kendilerine ait satır konusuna bakın. Ayrıca, uygulama şu anda parça veritabanına bağlı bir diğer kiracılar ait satır eklenemiyor. Ayrıca, uygulama Tenantıd görebileceğiniz herhangi bir satır olarak güncelleştirilemiyor. Uygulamanın, aşağıdakilerden birini kullanmaya çalışırsa, bir DbUpdateException tetiklenir.

Yeni bir tablo daha sonra eklerseniz, yeni tabloda FİLTRE ve ENGELLEME koşulları eklemek için güvenlik ilkesi değiştirin.

```sql
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK  PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable;
GO 
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a>Ekler için Tenantıd otomatik olarak doldurulması için varsayılan kısıtlamalar ekleme

OTURUMDA şu anda depolanan değerle Tenantıd otomatik olarak doldurulması için her bir tabloda bir default kısıtlaması koyabilirsiniz\_satır eklerken BAĞLAMI. Bir örnek aşağıda verilmiştir. 

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

Şimdi uygulamayı satır eklerken bir Tenantıd belirtmek gerekmez: 

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
> Bir Entity Framework projesi için varsayılan kısıtlamalar kullanırsanız, önerilen aldığınız *değil* EF veri modelinizi Tenantıd sütun içerir. Bu öneri Entity Framework oturum kullanmak T-SQL ile oluşturulan varsayılan kısıtlamalar geçersiz kılma tedarik varsayılan değerleri otomatik olarak sorgular çünkü\_BAĞLAMI.
> Varsayılan kısıtlamalar örnek projesinde kullanmak için örneği için Tenantıd DataClasses.cs (ve çalışma Add-Migration Paket Yöneticisi konsolunda) kaldırın ve T-SQL alanın yalnızca veritabanı tablolarında var olduğundan emin olmak için kullanmanız gerekir. Bu şekilde EF otomatik olarak yanlış varsayılan değerler veri eklerken sağlayın.

### <a name="optional-enable-a-superuser-to-access-all-rows"></a>(İsteğe bağlı) Etkinleştirme bir *süper kullanıcı* tüm satırları erişmek için

Bazı uygulamalar oluşturmak istediğiniz bir *süper kullanıcı* kullanan tüm satırları erişebilir. Süper kullanıcı tüm parça tüm kiracılar arasında raporlama sağlayabilir. Veya bir süper kullanıcı Kiracı satırları veritabanları arasında taşınması parça bölünmüş birleştirme işlemleri gerçekleştirebilir.

Süper kullanıcı etkinleştirmek için yeni bir SQL kullanıcısı oluşturun (`superuser` Bu örnekte) her parça veritabanında. Ardından bu kullanıcının tüm satırları erişmesine imkan tanıyan yeni bir koşul işlevi güvenlik ilkesiyle değiştirin. Bu tür bir işlevi sonraki verilir.

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

- **Yeni parça ekleme**: tüm yeni parça üzerinde RLS etkinleştirmek için T-SQL komut dosyası yürütme, aksi takdirde bu parça sorgulamaları değil olması filtrelenir.
- **Yeni tablo ekleme**: yeni bir tablo oluşturulduğunda tüm bölümler için güvenlik ilkesini bir FİLTRE ve ENGELLEME koşulu ekleyin. Aksi halde yeni bir tablo sorgularında değil olması filtrelenir. Bu ek bir DDL tetikleyicisi kullanarak açıklandığı gibi otomatikleştirilebilir [yeni oluşturulan tabloları (blog) otomatik olarak Uygula satır düzeyi güvenlik](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).

## <a name="summary"></a>Özet

Esnek veritabanı araçlarını ve satır düzeyi güvenlik genişleme yapmak bir uygulamanın veri katmanı hem çok müşterili desteği ile birlikte kullanılan ve tek Kiracı parça olabilir. Çok kiracılı parça, verileri daha verimli bir şekilde depolamak için kullanılabilir. Bu verimliliği, çok sayıda kiracılar yalnızca birkaç satır veri sahip olduğu belirgin olur. Tek Kiracı parça daha sıkı performans ve yalıtım gereksinimleri olan premium kiracılar destekleyebilir.  Daha fazla bilgi için bkz: [satır düzeyi güvenlik başvurusu][rls].

## <a name="additional-resources"></a>Ek kaynaklar

- [Bir Azure esnek havuz nedir?](sql-database-elastic-pool.md)
- [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md)
- [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](saas-tenancy-app-design-patterns.md)
- [Azure AD ve OpenID Connect kullanarak çok müşterili uygulamalarda kimlik doğrulama](../guidance/guidance-multitenant-identity-authenticate.md)
- [Tailspin Surveys uygulaması](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Sorular ve özellik istekleri

Üzerinde başvuracaklarını soruları için [SQL veritabanı Forumu](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Tüm özellik istekleri ekleyin [SQL veritabanı geri bildirim Forumunda](https://feedback.azure.com/forums/217321-sql-database/).


<!--Image references-->
[1]: ./media/saas-tenancy-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->
[rls]: https://docs.microsoft.com/sql/relational-databases/security/row-level-security
[s-d-elastic-database-client-library]: sql-database-elastic-database-client-library.md

