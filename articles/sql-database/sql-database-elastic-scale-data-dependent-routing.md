---
title: "Azure SQL veritabanı ile yönlendirme bağımlı veri | Microsoft Docs"
description: "Veri bağımlı yönlendirme, Azure SQL veritabanında parçalı veritabanlarının bir özellik için .NET uygulamalarında ShardMapManager sınıfını kullanma"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: cad09e15-5561-4448-aa18-b38f54cda004
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: ddove
ms.openlocfilehash: 2add03568f1d111010cdfb49d850d33cdab8e21b
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="data-dependent-routing"></a>Verilere bağımlı yönlendirme
**Veri bağımlı yönlendirme** uygun bir veritabanına isteği yönlendirmek için bir sorguda veri kullanma yeteneği. Bu temel düzeni parçalı veritabanları ile çalışırken. Özellikle parçalama anahtar sorgu parçası değilse, istek bağlamını isteği yönlendirmek için de kullanılabilir. Her özel bir sorgu veya veri bağımlı yönlendirme kullanarak bir uygulamayı işlemde istek başına tek bir veritabanına erişmek için sınırlıdır. Azure SQL veritabanı esnek araçlar için bu yönlendirme ile gerçekleştirilir **ShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)) sınıfı.

Uygulama çeşitli bağlantı dizeleri ya da parçalı ortamında verileri farklı dilimleri ilişkili DB konumları izlemek zorunda değildir. Bunun yerine, [parça eşleme Yöneticisi](sql-database-elastic-scale-shard-map-management.md) tabanlı açar bağlantılar gerektiğinde, doğru veritabanlarına parça eşleme ve uygulamanın isteği hedefidir parçalama anahtarının değerini veriler üzerinde. Genellikle anahtarıdır *customer_id*, *tenant_id*, *date_key*, veya veritabanı isteğin temel parametresi bazı bir belirli tanımlayıcısı. 

Daha fazla bilgi için bkz: [ölçeklendirme çıkışı SQL Server ile veri bağımlı yönlendirme](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-the-client-library"></a>İstemci kitaplığını yükle
Karşıdan yüklemek için:
* Kitaplık Java sürümü [Maven merkezi bir depoya](https://search.maven.org/#search%7Cga%7C1%7Celastic-db-tools).
* Kitaplığı .NET sürümü [NuGet](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Bir ShardMapManager bir veri bağımlı yönlendirme uygulamasında kullanma
Uygulamaları örneği **ShardMapManager** başlatılması sırasında fabrikada çağrısı kullanarak **GetSQLShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager_factory.getsqlshardmapmanager), [.NET ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)). Bu örnekte, hem bir **ShardMapManager** ve belirli bir **ShardMap** içerdiği başlatılır. Bu örnek GetSqlShardMapManager ve GetRangeShardMap gösterir ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.getrangeshardmap), [.NET](https://msdn.microsoft.com/library/azure/dn824173.aspx)) yöntemleri.

```Java
ShardMapManager smm = ShardMapManagerFactory.getSqlShardMapManager(connectionString, ShardMapManagerLoadPolicy.Lazy);
RangeShardMap<int> rangeShardMap = smm.getRangeShardMap(Configuration.getRangeShardMapName(), ShardKeyType.Int32);
```

```csharp
ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, ShardMapManagerLoadPolicy.Lazy);
RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 
```

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a>Parça eşleme için olası en düşük ayrıcalıklı kimlik bilgileri kullanın
Bir uygulama parça eşleme düzenleme değil, Fabrika yönteminde kullanılan kimlik bilgileri yalnızca salt okunur izinleri olmalıdır **genel parça eşleme** veritabanı. Bu kimlik bilgileri bağlantıları parça eşleme Yöneticisi'ni açmak için kullanılan kimlik bilgileri genellikle farklıdır. Ayrıca bkz. [esnek veritabanı istemci kitaplığına erişmek için kullanılan kimlik bilgileri](sql-database-elastic-scale-manage-credentials.md). 

## <a name="call-the-openconnectionforkey-method"></a>OpenConnectionForKey yöntemini çağırın
**ShardMap.OpenConnectionForKey yöntemi** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper._list_shard_mapper.openconnectionforkey), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)) bir bağlantı komutları değerine göre uygun veritabanı verme için hazır döndürür **anahtar** parametresi. Parça bilgi uygulama tarafından önbelleğe alınmış **ShardMapManager**, bu istekleri genellikle bir veritabanı araması karşı içermeyen **genel parça eşleme** veritabanı. 

```Java
// Syntax: 
public Connection openConnectionForKey(Object key, String connectionString, ConnectionOptions options)
```

```csharp
// Syntax: 
public SqlConnection OpenConnectionForKey<TKey>(TKey key, string connectionString, ConnectionOptions options)
```
* **Anahtar** parametresi arama anahtarı olarak parça eşlemeye istek için uygun veritabanı belirlemek için kullanılır. 
* **ConnectionString** yalnızca kullanıcı kimlik bilgilerini istenen bağlantı için geçirmek için kullanılır. Hiçbir veritabanı adı veya sunucu adı bu dahil *connectionString* yöntemi ve veritabanı sunucusu kullanarak belirler beri **ShardMap**. 
* **ConnectionOptions** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper._connection_options), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)) ayarlanmalı **ConnectionOptions.Validate** burada parça eşlemeleri may bir ortamda, değişiklik ve satır bölme ve birleştirme işlemleri sonucunda diğer veritabanlarına taşıyabilir. Bu hedefte yerel parça eşleme için kısa bir sorgu içerir bağlantı uygulamaya teslim edilmeden önce veritabanı (değil genel parça eşleme için). 

(Önbellek yanlış olduğunu belirten) yerel parça eşleme karşı doğrulama başarısız olursa, parça eşleme Yöneticisi arama yeni doğru değerini edinme, önbellek, güncelleştirme ve elde ve uygun veritabanı bağlantısı dönmek için genel parça harita sorgular . 

Kullanım **ConnectionOptions.None** uygulamanın çevrimiçi durumdayken yalnızca zaman parça eşleme değişiklikleri beklenmez. Bu durumda, önbelleğe alınan değer her zaman doğru olması için kabul edilebilir ve hedef veritabanına ek gidiş dönüş doğrulama çağrısı güvenle atlanabilir. Veritabanı trafiğini azaltır. **ConnectionOptions** bir değer parçalama değişiklikleri beklenen olup olmadığını belirtmek için bir yapılandırma dosyası veya bir zaman aralığında sırasında değil de ayarlanabilir.  

Bu örnek bir tamsayı anahtarının değerini kullanır **CustomerID**kullanarak bir **ShardMap** adlı nesne **customerShardMap**.  

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

**OpenConnectionForKey** yöntemi doğru veritabanına yeni bir zaten açık bağlantı döndürür. Bu şekilde kullanılan bağlantılar hala bağlantı havuzu tam avantajından yararlanmak. 

**OpenConnectionForKeyAsync yöntemi** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapper._list_shard_mapper.openconnectionforkeyasync), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)) da uygulamanızı kullanım zaman uyumsuz programlama yaparsa kullanılabilir.

## <a name="integrating-with-transient-fault-handling"></a>Geçici hata işleme ile tümleştirme
Veri erişimi uygulamaları bulutta geliştirilmesi konusunda en iyi uygulama, uygulama tarafından geçici hataları yakalanır ve işlemleri hata atmadan önce birkaç kez denenir emin olmaktır. Bulut uygulamaları için işleme geçici hata geçici hata işleme sırasında ele alınmıştır ([Java](/java/api/com.microsoft.azure.elasticdb.core.commons.transientfaulthandling), [.NET](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx)). 

Geçici hata işleme, veri bağımlı yönlendirme desenle doğal olarak bulunabilir. Tüm veri erişim isteği dahil olmak üzere yeniden denemek için önemli gereksinimdir **kullanarak** veri bağımlı yönlendirme bağlantısı elde bloğu. Önceki örnekte aşağıdaki gibi yeniden yazılmıştır. 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a>Örnek - veri geçici hata işleme ile yönlendirme bağımlı
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

Esnek veritabanı örnek uygulamasını derlerken geçici hata işleme uygulanması için gerekli paketleri otomatik olarak yüklenir. 

## <a name="transactional-consistency"></a>İşlem tutarlılığı
İşlem özellikleri tüm işlemler için bir parça yerel olarak sağlanır. Örneğin, bağlantı için hedef parça kapsamında veri bağımlı yönlendirme üzerinden gönderilen işlemleri yürütün. Şu anda bir işlem içinde birden çok bağlantı kaydetme için sağlanan özelliği yoktur ve bu nedenle parça üzerinde gerçekleştirilen işlemler için işlem garanti vardır.

## <a name="next-steps"></a>Sonraki adımlar
Bir parça detach veya bir parça yeniden eklemek için bkz: [parça eşleme sorunları düzeltmek için RecoveryManager sınıfını kullanma](sql-database-elastic-database-recovery-manager.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

