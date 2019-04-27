---
title: Azure SQL veritabanı ile verilere bağımlı yönlendirme | Microsoft Docs
description: Verilere bağımlı yönlendirme, Azure SQL veritabanı'nda parçalı veritabanlarından oluşan bir özellik için .NET uygulamalarında ShardMapManager sınıfı kullanma
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: fe9098592fcfde2d5e23b78a3e33f2b4ebb9e2dc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60584973"
---
# <a name="use-data-dependent-routing-to-route-a-query-to-appropriate-database"></a>Veri bağımlı bir sorgu için uygun veritabanı yönlendirmek için yönlendirme kullanın

**Verilere bağımlı yönlendirme** veri isteği yönlendirmek için uygun bir veritabanı için sorguda kullanma yeteneğidir. Veri bağımlı yönlendirme temel düzeni, parçalı veritabanları ile çalışırken. Özellikle parçalama anahtarı sorgunun bir parçası değilse, istek bağlamı isteği yönlendirmek için de kullanılabilir. Her özel bir sorgu veya verilere bağımlı yönlendirme kullanarak uygulama işlemde istek başına bir veritabanı erişimi sınırlıdır. Azure SQL veritabanı elastik araçlar için bu yönlendirme ile gerçekleştirilir **ShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager)) sınıfı.

Uygulama çeşitli bağlantı dizelerini veya farklı parçalı ortamında veri dilimleri ilişkili DB konumları izlemeniz gerekmez. Bunun yerine, [parça eşleme Yöneticisi](sql-database-elastic-scale-shard-map-management.md) parça eşlemesi ve uygulamanın isteğin hedefi parçalama anahtarı değerini veriler temelinde gerekli değilse, doğru veritabanlarına bağlantı açar. Genellikle anahtardır *customer_id*, *Kiracı*, *date_key*, veya bir temel veritabanı istek parametresi bazı bir belirli tanımlayıcısı.

Daha fazla bilgi için [ölçeklendirme kullanıma SQL Server ile verilere bağımlı yönlendirme](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-the-client-library"></a>İstemci Kitaplığı'nı indirin

İndirmek için:

* Java sürümü kitaplığının görmek [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Celastic-db-tools).
* .NET sürümü kitaplığının görmek [NuGet](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Verilere bağımlı yönlendirme uygulamada bir ShardMapManager kullanma

Uygulamaları örneğini **ShardMapManager** başlatma sırasında Fabrika kullanarak **GetSQLShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.getsqlshardmapmanager), [.NET ](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)). Bu örnekte, hem bir **ShardMapManager** ve belirli bir **ShardMap** içerdiği başlatılır. Bu örnek GetSqlShardMapManager ve GetRangeShardMap gösterir ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.getrangeshardmap), [.NET](https://docs.microsoft.com/previous-versions/azure/dn824173(v=azure.100))) yöntemleri.

```Java
ShardMapManager smm = ShardMapManagerFactory.getSqlShardMapManager(connectionString, ShardMapManagerLoadPolicy.Lazy);
RangeShardMap<int> rangeShardMap = smm.getRangeShardMap(Configuration.getRangeShardMapName(), ShardKeyType.Int32);
```

```csharp
ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnectionString, ShardMapManagerLoadPolicy.Lazy);
RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 
```

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a>Parça eşlemesini almak için olası en düşük ayrıcalıklı kimlik bilgileri kullanın.

Bir uygulama parça eşlemesi düzenleme yok, Fabrika yöntemde kullanılan kimlik bilgileri salt okunur izinlere sahip **genel parça eşleme** veritabanı. Bu kimlik bilgileri, parça eşleme Yöneticisi için bağlantıları'nı açmak için kullanılan kimlik bilgileri genellikle farklıdır. Ayrıca bkz: [elastik veritabanı istemci kitaplığı erişmek için kimlik bilgileri kullanılan](sql-database-elastic-scale-manage-credentials.md).

## <a name="call-the-openconnectionforkey-method"></a>OpenConnectionForKey yöntemini çağırın

**ShardMap.OpenConnectionForKey yöntemi** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkey), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey)) bir bağlantı hazır değerini temel alarak uygun veritabanı komutları verme döndürür **anahtar** parametresi. Parça bilgi uygulama tarafından önbelleğe alınan **ShardMapManager**, bu istekler genellikle veritabanı arama içermeyen **genel parça eşleme** veritabanı.

```Java
// Syntax:
public Connection openConnectionForKey(Object key, String connectionString, ConnectionOptions options)
```

```csharp
// Syntax:
public SqlConnection OpenConnectionForKey<TKey>(TKey key, string connectionString, ConnectionOptions options)
```

* **Anahtar** parametre bir arama anahtarı parça eşlemesine istek için uygun veritabanı belirlemek için kullanılır.
* **ConnectionString** yalnızca istenen bağlantı kullanıcı kimlik bilgilerini geçirmek için kullanılır. Hiçbir veritabanı adı veya sunucu adı bu dahildir *connectionString* yöntemi veritabanı ve sunucu kullanarak belirler. bu yana **ShardMap**.
* **ConnectionOptions** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.connectionoptions), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions)) ayarlanmalıdır **ConnectionOptions.Validate** nerede parça eşlemeleri Mayıs bir ortam ise değişiklik ve satır bölme ve birleştirme işlemleri sonucu olarak başka bir veritabanına taşınabilir. Bu doğrulama hedef yerel parça eşlemesine kısa bir sorgu içerir bağlantı uygulamaya teslim edilmeden önce veritabanı (değil genel parça eşleme için).

(Önbellek yanlış olduğunu belirten) yerel parça eşlemesi karşı doğrulama başarısız olursa, parça eşleme Yöneticisi, arama için yeni doğru değeri elde etmek, önbellek, güncelleştirme ve elde ve uygun veritabanı bağlantısı dönmek için global parça eşleme sorgular. .

Kullanım **ConnectionOptions.None** uygulama çevrimiçi durumdayken yalnızca zaman parça eşleme değişiklikleri beklenmiyor. Bu durumda, önbelleğe alınan değerler her zaman doğru olarak kabul edilebilir ve hedef veritabanına ek gidiş dönüş doğrulama çağrısı güvenli bir şekilde atlanabilir. Bu, veritabanı trafiğini azaltır. **ConnectionOptions** parçalama değişiklikleri beklenen olup olmadığını belirtmek için bir yapılandırma dosyası veya bir süre boyunca değil bir değer da ayarlanabilir.  

Bu örnekte bir tamsayı anahtarı değerini **CustomerID**kullanarak bir **ShardMap** adlı nesne **customerShardMap**.  

```Java
int customerId = 12345;
int productId = 4321;
// Looks up the key in the shard map and opens a connection to the shard
try (Connection conn = shardMap.openConnectionForKey(customerId, Configuration.getCredentialsConnectionString())) {
    // Create a simple command that will insert or update the customer information
    PreparedStatement ps = conn.prepareStatement("UPDATE Sales.Customer SET PersonID = ? WHERE CustomerID = ?");

    ps.setInt(1, productId);
    ps.setInt(2, customerId);
    ps.executeUpdate();
} catch (SQLException e) {
    e.printStackTrace();
}
```

```csharp
int customerId = 12345;
int newPersonId = 4321;

// Connect to the shard for that customer ID. No need to call a SqlConnection
// constructor followed by the Open method.
using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate))
{
    // Execute a simple command.
    SqlCommand cmd = conn.CreateCommand();
    cmd.CommandText = @"UPDATE Sales.Customer
                        SET PersonID = @newPersonID WHERE CustomerID = @customerID";

    cmd.Parameters.AddWithValue("@customerID", customerId);cmd.Parameters.AddWithValue("@newPersonID", newPersonId);
    cmd.ExecuteNonQuery();
}  
```

**OpenConnectionForKey** yöntemi doğru veritabanına yeni bir bağlantı zaten açık döndürür. Bu şekilde kullanılan bağlantılar, hala bağlantı havuzu tam avantajından faydalanın.

**OpenConnectionForKeyAsync yöntemi** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper.listshardmapper.openconnectionforkeyasync), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync)) da uygulamanızın kullanım zaman uyumsuz programlama yaparsa kullanılabilir.

## <a name="integrating-with-transient-fault-handling"></a>Geçici hata işleme ile tümleştirme

Veri erişimi uygulamaları bulutta geliştirilmesi konusunda en iyi uygulama, geçici hataların uygulama tarafından yakalanır ve işlemleri bir hata atamadan önce birkaç kez şartıyla sağlamaktır. Geçici hata işleme bulut uygulamaları için geçici hata işleme sırasında ele alınmıştır ([Java](/java/api/com.microsoft.azure.elasticdb.core.commons.transientfaulthandling), [.NET](https://docs.microsoft.com/previous-versions/msp-n-p/dn440719(v=pandp.60))).

Geçici hata işleme verilere bağımlı yönlendirme desen ile doğal olarak bulunabilir. Tüm veri erişim isteği dahil olmak üzere yeniden denemek için önemli gereksinimdir **kullanarak** verilere bağımlı yönlendirme bağlantısı elde edilen blok. Yukarıdaki örnekte şu şekilde yazılması.

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Örnek - verilere bağımlı yönlendirme ile geçici hata işleme

```Java
int customerId = 12345;
int productId = 4321;
try {
    SqlDatabaseUtils.getSqlRetryPolicy().executeAction(() -> {
        // Looks up the key in the shard map and opens a connection to the shard
        try (Connection conn = shardMap.openConnectionForKey(customerId, Configuration.getCredentialsConnectionString())) {
            // Create a simple command that will insert or update the customer information
            PreparedStatement ps = conn.prepareStatement("UPDATE Sales.Customer SET PersonID = ? WHERE CustomerID = ?");

            ps.setInt(1, productId);
            ps.setInt(2, customerId);
            ps.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    });
} catch (Exception e) {
    throw new StoreException(e.getMessage(), e);
}
```

```csharp
int customerId = 12345;
int newPersonId = 4321;

Configuration.SqlRetryPolicy.ExecuteAction(() =&gt;
{
    // Connect to the shard for a customer ID.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate))
    {
        // Execute a simple command
        SqlCommand cmd = conn.CreateCommand();

        cmd.CommandText = @"UPDATE Sales.Customer
                            SET PersonID = @newPersonID
                            WHERE CustomerID = @customerID";

        cmd.Parameters.AddWithValue("@customerID", customerId);
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId);
        cmd.ExecuteNonQuery();

        Console.WriteLine("Update completed");
    }
});
```

Esnek veritabanı örnek uygulamasını derlerken, geçici hata işleme uygulamak için gerekli paketleri otomatik olarak yüklenir.

## <a name="transactional-consistency"></a>İşlem tutarlılığı

İşlem özellikleri tüm işlemler için yerel bir parçaya garanti edilir. Örneğin, bağlantı için hedef parça kapsamında verilere bağımlı Yönlendirme aracılığıyla gönderilen işlemler yürütün. Şu anda bir işlem içinde birden çok bağlantı kaydetme için sağlanan hiçbir özellikleri vardır ve bu nedenle parçalar arasında gerçekleştirilen işlemleri için işlemsel tutarlılık garantisi yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Bir parçaya ayırmak için veya bir parça yeniden eklemek için bkz: [parça eşleme sorunlarını düzeltme RecoveryManager sınıfı kullanma](sql-database-elastic-database-recovery-manager.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]