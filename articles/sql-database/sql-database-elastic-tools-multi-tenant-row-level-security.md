---
title: "Çok kiracılı uygulamalarla esnek veritabanı araçlarını ve satır düzeyi güvenlik"
description: "Azure SQL veritabanı üzerinde çoklu kiracı parça destekleyen bir yüksek düzeyde ölçeklenebilir veri katmanı ile bir uygulama oluşturmak için satır düzeyi güvenlik ile birlikte esnek veritabanı araçlarını kullanmayı öğrenin."
metakeywords: azure sql database elastic tools multi tenant row level security rls
services: sql-database
documentationcenter: 
manager: jhubbard
author: tmullaney
ms.assetid: e72d3cfe-e9be-4326-b776-9c6d96c0a18e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: thmullan;torsteng
ms.openlocfilehash: 73f1210b8d1f5ceca8fac9534d498bdc23d96d48
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Çok kiracılı uygulamalarla esnek veritabanı araçlarını ve satır düzeyi güvenlik
[Esnek veritabanı araçlarını](sql-database-elastic-scale-get-started.md) ve [satır düzeyi güvenlik (RLS)](https://msdn.microsoft.com/library/dn765131) güçlü bir esnek ve verimli bir şekilde Azure SQL veritabanı olan bir çok kiracılı uygulama veri katmanı ölçeklendirmeye yönelik özellik kümesi sunar. Bkz: [Azure SQL veritabanı ile çok kiracılı SaaS uygulamaları için Tasarım desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md) daha fazla bilgi için. 

Bu makalede bu teknolojiler birlikte kullanarak çok kiracılı parça destekleyen bir yüksek düzeyde ölçeklenebilir veri katmanı ile bir uygulama oluşturmak için nasıl kullanılacağını anlatan **ADO.NET SqlClient** ve/veya **Entity Framework**.  

* **Esnek veritabanı araçlarını** bir uygulamanın bir dizi .NET kitaplıkları ve Azure hizmet şablonları kullanarak endüstri standardı parçalama yöntemler aracılığıyla veri katmanı genişletmek için geliştiricilere olanak sağlar. Esnek veritabanı istemci kitaplığı kullanılarak ile parça yönetme otomatikleştirmek ve genellikle parçalama ile ilişkili infrastructural görevlerin çoğunu akıcı hale getirmesine yardımcı olur. 
* **Satır düzeyi güvenlik** aynı veritabanında bir sorgu yürütülürken kiracıya ait değil satırları filtrelemek için güvenlik ilkeleri kullanılarak birden çok Kiracı için verileri depolamak için geliştiricilere olanak sağlar. Veritabanı içinde erişim mantığı RLS ile merkezileştirme uygulamada, bakım kolaylaştırır ve uygulama takımın kod temeline etme gibi hata riskini azaltır yerine artar. 

Bu özellikleri birlikte kullanarak, bir uygulama için aynı parça veritabanında birden çok kiracıya veri depolayarak maliyet tasarrufu ve verimliliği elde eder yararlanabilirsiniz. Aynı anda bir uygulama hala daha sıkı performans garanti çok kiracılı parça kiracılar arasında eşit kaynak dağıtım garanti etmiyoruz beri duyan "premium" kiracılar için yalıtılmış, tek kiracılı parça teklif esnekliğine sahiptir.  

Kısacası, esnek veritabanı istemci kitaplığının [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) API'leri otomatik olarak kiracılar kendi parçalama anahtarı (genellikle bir "Tenantıd") içeren doğru parça veritabanına bağlanın. Bağlantı kurulduktan sonra veritabanını bir RLS güvenlik ilkesinde kiracılar kendi Tenantıd içeren satırları yalnızca erişebilmesini sağlar. Tüm tabloları hangi satırların her kiracıya ait belirtmek için bir Tenantıd sütunu içeren varsayılır. 

![Blog uygulama mimarisi][1]

## <a name="download-the-sample-project"></a>Örnek Proje yükle
### <a name="prerequisites"></a>Ön koşullar
* Visual Studio (2012 veya sonraki sürümlerini) kullanın 
* Üç Azure SQL veritabanı oluşturma 
* Örnek projesini indirin: [Azure SQL - çok Kiracılı parça için esnek veritabanı araçları](http://go.microsoft.com/?linkid=9888163)
  * Veritabanlarınızı başındaki bilgileri doldurun **Program.cs** 

Bu proje açıklandığı bir genişletir [Azure SQL - Entity Framework tümleştirme için esnek DB Araçları](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) çok kiracılı parça veritabanları için destek ekleyerek düzenleyin. Bloglar ve gönderileri, dört kiracılar ve Yukarıdaki diyagramda gösterildiği gibi iki parça çok Kiracı veritabanı oluşturmak için basit bir konsol uygulaması oluşturur. 

Derleme ve uygulamayı çalıştırın. Bu esnek veritabanı araçlarını parça eşleme Yöneticisi bootstrap ve aşağıdaki testleri çalıştırın: 

1. Entity Framework ve LINQ kullanarak yeni bir blog oluşturun ve ardından her bir kiracı için tüm Web Günlüklerini Görüntüle
2. ADO.NET SqlClient kullanarak, bir kiracı için tüm Web Günlüklerini Görüntüle
3. Bir hata atılır doğrulamak yanlış Kiracı için bir blog eklemeyi deneyin  

RLS parça veritabanlarında henüz etkinleştirilmemiş olduğundan, bu testler, her bir sorun ortaya dikkat edin: kiracılar kendilerine ait değil bloglar görmeye ve uygulama yanlış Kiracı için bir blog ekleme engellenemez. Bu makalenin sonraki bölümlerinde, Kiracı yalıtımı RLS ile zorlayarak bu sorunları gidermek açıklar. İki adımı vardır: 

1. **Uygulama katmanı**: her zaman bir bağlantı açıldıktan sonra geçerli Tenantıd SESSION_CONTEXT ayarlamak için uygulama kodu değiştirin. Örnek Proje zaten bu yapmıştır. 
2. **Veri katmanı**: SESSION_CONTEXT içinde depolanan Tenantıd göre filtre satırlara her parça veritabanındaki bir RLS güvenlik ilkesi oluşturun. Her parça veritabanlarınız için bunu yapmanız gerekir, aksi takdirde çok kiracılı parça satırlarda değil filtrelenir. 

## <a name="step-1-application-tier-set-tenantid-in-the-sessioncontext"></a>1. adım) uygulama katmanı: SESSION_CONTEXT içinde Tenantıd ayarlayın
Böylece bir RLS güvenlik ilkesini ait satır filtrelemek bağımlı yönlendirme API, uygulamanın halen veritabanı bildirmek için gereken esnek veritabanı istemci kitaplığının verileri kullanarak bir parça veritabanına bağlandıktan sonra hangi Tenantıd o bağlantı kullanıyor diğer kiracılar. Bu bağlantı için geçerli Tenantıd depolamak için bu bilgileri geçirmek için önerilen yöntem olduğu [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx). (Not: alternatif olarak kullanabileceğinizi [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), ancak SESSION_CONTEXT daha iyi bir seçenek kullanmak daha kolay olduğundan varsayılan değer olarak NULL döndürür ve anahtar-değer çiftleri destekler.)

### <a name="entity-framework"></a>Entity Framework
Entity Framework kullanan uygulamalar için en kolay yaklaşım içinde açıklanan ElasticScaleContext geçersiz kılma SESSION_CONTEXT ayarlamaktır [EF DbContext kullanarak veri bağımlı yönlendirme](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Veri bağımlı Yönlendirme aracılığıyla aracılı bağlantı dönmeden önce oluşturun ve o bağlantı için belirtilen shardingKey için SESSION_CONTEXT 'Tenantıd' ayarlar SqlCommand yürütün. Bu şekilde, yalnızca bir kez SESSION_CONTEXT ayarlamak için kod yazmanız gerekir. 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data dependent routing. This call will open a validated connection routed to the proper 
// shard by the shard map manager. Note that the base class c'tor call will fail for an open connection 
// if migrations need to be done and SQL credentials are used. This is the reason for the  
// separation of c'tors into the DDR case (this c'tor) and the internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
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

Şimdi ElasticScaleContext çağrılan her SESSION_CONTEXT ile belirtilen Tenantıd otomatik olarak ayarlanır: 

```
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
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
ADO.NET SqlClient kullanan uygulamalar için otomatik olarak 'Tenantıd' SESSION_CONTEXT doğru Tenantıd için bir bağlantı döndürmeden önce ayarlar ShardMap.OpenConnectionForKey() çevresinde bir sarmalayıcı işlevi oluşturma önerilen yaklaşımdır. SESSION_CONTEXT her zaman ayarlandığından emin olmak için yalnızca bu sarmalayıcı işlevi kullanarak bağlantı açmanız gerekir.

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets SESSION_CONTEXT with the correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method to ensure that SESSION_CONTEXT is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
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

// Example query via ADO.NET SqlClient
// If row-level security is enabled, only Tenant 4's blogs will be listed
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine("--\nAll blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);
        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="step-2-data-tier-create-row-level-security-policy"></a>2. adım) veri katmanı: satır düzeyi güvenlik ilkesi oluşturma
### <a name="create-a-security-policy-to-filter-the-rows-each-tenant-can-access"></a>Her Kiracı erişebileceği satırları filtrelemek için bir güvenlik ilkesi oluşturma
Uygulama SESSION_CONTEXT geçerli Tenantıd ile sorgulama önce ayarı, bir RLS güvenlik ilkesi sorguları filtrelemek ve farklı bir tenantıd değerine sahip satırları dışlama.  

RLS, T-SQL uygulanır: kullanıcı tanımlı bir işlev erişim mantığı tanımlar ve bir güvenlik ilkesi bu işlev tabloları herhangi bir sayıda bağlar. Bu proje için işlevi yalnızca uygulama (başka bir SQL kullanıcı yerine) veritabanına bağlanır ve belirli bir satırın Tenantıd eşleşen 'Tenantıd' SESSION_CONTEXT depolanan doğrular. Bir filtre koşulu seçin, güncelleştirme ve silme sorguları için filtre geçmesine bu koşullara uyan satırları izin verir; ve bir engelleme koşuluna eklenen veya güncelleştirilen engeller Bu koşulları ihlal satırları engeller. SESSION_CONTEXT ayarlı değil, NULL ve hiçbir satır görünür veya eklenecek mümkün olacaktır döndürür. 

RLS etkinleştirmek için Visual Studio (SSDT), SSMS veya projeye dahil PowerShell komut dosyası kullanarak tüm parça aşağıdaki T-SQL yürütmek (veya kullanıyorsanız [esnek veritabanı iş](sql-database-elastic-jobs-overview.md), bu yürütülmesi otomatikleştirmek için kullanın Tüm parça üzerinde T-SQL): 

```
CREATE SCHEMA rls -- separate schema to organize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- the user in your application’s connection string (dbo is only for demo purposes!)         
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts
GO 
```

> [!TIP]
> Tabloları yüzlerce üzerinde koşulu eklemek için gereken daha karmaşık projelerde otomatik olarak bir şemadaki tüm tablolarda bir koşul ekleme bir güvenlik ilkesi oluşturur bir yardımcı depolanan yordamı kullanabilirsiniz. Bkz: [geçerli satır düzeyi güvenlik tüm tablolara - Yardımcısı komut dosyası (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).  
> 
> 

Şimdi örnek uygulamayı yeniden çalıştırın, kiracılar kendilerine ait satır görmeyebilir olur. Ayrıca, uygulama bir parça veritabanı şu anda bağlı ve farklı bir Tenantıd olmasını görünen satır güncelleştirilemiyor dışında kiracılara ait satır eklenemiyor. Uygulamanın, aşağıdakilerden birini kullanmaya çalışırsa, bir DbUpdateException gerçekleştirilecektir.

Yeni bir tablo daha sonra eklerseniz, yalnızca güvenlik ilkesi ALTER ve filtre eklemek ve yeni bir tablo üzerinde koşulları engelle: 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-to-automatically-populate-tenantid-for-inserts"></a>Ekler için Tenantıd otomatik olarak doldurulması için varsayılan kısıtlamalar ekleme
Varsayılan kısıtlama Tenantıd şu anda satır eklerken SESSION_CONTEXT içinde depolanan değerle otomatik olarak doldurulması için her bir tabloda koyabilirsiniz. Örneğin: 

```
-- Create default constraints to auto-populate TenantId with the value of SESSION_CONTEXT for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO 
```

Şimdi uygulamayı satır eklerken bir Tenantıd belirtmek gerekmez: 

```
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var blog = new Blog { Name = name }; // default constraint sets TenantId automatically     
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [!NOTE]
> Bir Entity Framework projesi için varsayılan kısıtlamalar kullanırsanız, Tenantıd sütun EF veri modelinizi içermez önerilir. Entity Framework sorguları otomatik olarak SESSION_CONTEXT kullanan T-SQL ile oluşturulan varsayılan kısıtlamalar geçersiz kılar varsayılan değerleri sağlayın olmasıdır. Varsayılan kısıtlamalar örnek projesinde kullanmak için örneği için Tenantıd DataClasses.cs (ve çalışma Add-Migration Paket Yöneticisi konsolunda) kaldırın ve T-SQL alanın yalnızca veritabanı tablolarında var olduğundan emin olmak için kullanmanız gerekir. Bu şekilde EF otomatik olarak yanlış varsayılan değerler veri eklerken sağlamaz. 
> 
> 

### <a name="optional-enable-a-superuser-to-access-all-rows"></a>(İsteğe bağlı) "Tüm satırları erişmek bir süper kullanıcı" etkinleştirme
Bazı uygulamalarda tüm parça tüm kiracılar arasında raporlamayı etkinleştirmek için ya da Kiracı satırları veritabanları arasında taşınması parça bölünmüş/birleştirme işlemleri gerçekleştirmek için "tüm satırları örneği için erişebilen bir süper kullanıcı" oluşturmak isteyebilirsiniz. Bu ayarı etkinleştirmek için her parça veritabanında yeni bir SQL kullanıcısının ("süper kullanıcı" Bu örnekte) oluşturmanız gerekir. Ardından bu kullanıcının tüm satırları erişmesine imkan tanıyan yeni bir koşul işlevi güvenlik ilkesiyle alter:

```
-- New predicate function that adds superuser logic
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- note, should not be dbo!
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        )
GO

-- Atomically swap in the new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### <a name="maintenance"></a>Bakım
* **Yeni parça ekleme**: tüm yeni parça üzerinde RLS etkinleştirmek için T-SQL betiği yürütmeniz gerekir, aksi takdirde bu parça sorgulamaları değil filtrelenir.
* **Yeni tablo ekleme**: yeni bir tablo oluşturulduğunda tüm bölümler için güvenlik ilkesini için bir filtre ve engelleme koşulu eklemeniz gerekir, aksi halde yeni bir tablo sorgularında değil filtrelenir. Bu DDL tetikleyicisi kullanarak açıklandığı gibi otomatikleştirilebilir [yeni oluşturulan tabloları (blog) otomatik olarak Uygula satır düzeyi güvenlik](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).

## <a name="summary"></a>Özet
Esnek veritabanı araçlarını ve satır düzeyi güvenlik genişleme yapmak bir uygulamanın veri katmanı hem çok müşterili desteği ile birlikte kullanılan ve tek Kiracı parça olabilir. Çok kiracılı parça (çok sayıda kiracılar yalnızca birkaç satır veri olduğu özellikle durumlarda), verileri daha verimli bir şekilde depolamak için kullanılabilecek tek-Kiracı sırasında parça, premium kiracılar daha sıkı performans ve yalıtım ile desteklemek için kullanılabilir gereksinimleri.  Daha fazla bilgi için bkz: [satır düzeyi güvenlik başvurusu](https://msdn.microsoft.com/library/dn765131). 

## <a name="additional-resources"></a>Ek kaynaklar
* [Bir Azure esnek havuz nedir?](sql-database-elastic-pool.md)
* [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md)
* [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Desenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Azure AD ve OpenID Connect kullanarak çok müşterili uygulamalarda kimlik doğrulama](../guidance/guidance-multitenant-identity-authenticate.md)
* [Tailspin Surveys uygulaması](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Sorular ve özellik istekleri
Sorular için lütfen bize üzerinde ulaşmak [SQL veritabanı Forumu](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) ve özellik istekler için lütfen bunları Ekle [SQL veritabanı geri bildirim Forumunda](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


