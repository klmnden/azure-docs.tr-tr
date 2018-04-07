---
title: Sorgu parçalı Azure SQL veritabanları | Microsoft Docs
description: Esnek veritabanı istemci kitaplığı kullanılarak parça sorgular çalıştırın.
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 13d62250112d47ce79f0345828ba69358f153c2d
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="multi-shard-querying"></a>Çok parça sorgulama
## <a name="overview"></a>Genel Bakış
İle [esnek veritabanı araçlarını](sql-database-elastic-scale-introduction.md), parçalı veritabanı çözümleri oluşturabilirsiniz. **Çok parça sorgulama** koleksiyonu/bir sorgu çalıştırılarak gerektiren raporlama verilerini birden fazla parça uzatır gibi görevler için kullanılır. (Bu Karşıtlık [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md), tek bir parça tüm çalışma gerçekleştirir.) 

1. Alma bir **RangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807318.aspx)) veya **ListShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._list_shard_map), [.NET ](https://msdn.microsoft.com/library/azure/dn807370.aspx)) kullanarak **TryGetRangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetrangeshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx)), **TryGetListShardMap** ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetlistshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)), veya **GetShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.getshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx)) yöntemi. Bkz: **[bir ShardMapManager oluşturma](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager)** ve  **[RangeShardMap veya ListShardMap almak](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap)**.
2. Oluşturma bir **MultiShardConnection** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_connection), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)) nesnesi.
3. Oluşturma bir **MultiShardStatement veya MultiShardCommand** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)). 
4. Ayarlama **CommandText özelliği** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)) bir T-SQL komutu.
5. Çağırarak komutu yürütün **ExecuteQueryAsync veya ExecuteReader** ([Java](), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)) yöntemi.
6. Kullanarak sonuçları görüntülemek **MultiShardResultSet veya MultiShardDataReader** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_result_set), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)) sınıfı. 

## <a name="example"></a>Örnek
Aşağıdaki kodu kullanarak sorgulama yapmayı çok parça kullanımını göstermektedir bir verilen **ShardMap** adlı *myShardMap*. 

```csharp
using (MultiShardConnection conn = new MultiShardConnection(myShardMap.GetShards(), myShardConnectionString)) 
{ 
    using (MultiShardCommand cmd = conn.CreateCommand())
    { 
        cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
        cmd.CommandType = CommandType.Text; 
        cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
        cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

        using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
        { 
            while (sdr.Read())
            { 
                var c1Field = sdr.GetString(0); 
                var c2Field = sdr.GetFieldValue<int>(1); 
                var c3Field = sdr.GetFieldValue<Int64>(2);
            } 
        } 
    } 
} 
```

En önemli fark, çok parça bağlantılarının yapıdır. Burada **SqlConnection** tek bir veritabanı üzerinde çalışır **MultiShardConnection** geçen bir ***parça koleksiyonunu*** giriş olarak. Parça parça eşlemesinden koleksiyonunu doldurun. Sorgu kullanarak parça koleksiyonunu yürütülür **UNION ALL** semantiği tek bir genel sonuç birleştirin. İsteğe bağlı olarak, satırın kaynaklandığı parça adını kullanarak çıktı eklenebilir **ExecutionOptions** özelliği komutu. 

Çağrı Not **myShardMap.GetShards()**. Bu yöntem tüm parça parça eşlemesinden alır ve tüm ilgili veritabanları arasında bir sorgu çalıştırmak için kolay bir yol sağlar. Parça koleksiyonunu çok parça sorgu Gelişmiş için daha fazla koleksiyon üzerinde bir LINQ sorgusu gerçekleştirerek sağlayıcıdan döndürülen çağrısı **myShardMap.GetShards()**. Kısmi sonuçlar İlkesi ile birlikte, çok parça sorgulama içinde geçerli yetenek parça yüzlerce kadar onlarca için iyi çalışmak üzere tasarlanmıştır.

Çok parça sorgulama ile bir sınırlama şu anda doğrulama parça ve sorgulanır shardlets yetersizliğidir. Veri bağımlı yönlendirme sorgulama sırasında verilen parça parça eşleme parçası olduğunu doğrularken çok parça sorguları bu denetimi gerçekleştirme. Bu parça eşlemesinden kaldırılan veritabanları üzerinde çalışan çok parça sorguları neden olabilir.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Çok parça sorgular ve bölünmüş birleştirme işlemleri
Çok parça sorguları shardlets sorgulanan veritabanı üzerinde devam eden bölünmüş birleştirme işlemleri katılan olup olmadığını denetlemez. (Bkz [esnek veritabanı bölünmüş birleştirme aracını kullanarak ölçeklendirme](sql-database-elastic-scale-overview-split-and-merge.md).) Burada aynı çok parça sorguda birden çok veritabanları için aynı shardlet satırları göster bu tutarsızlıklar için yol açabilir. Bu sınırlamalara dikkat edin ve boşaltma devam eden bölünmüş birleştirme işlemleri ve parça eşleme değişiklikler çok parça sorguları gerçekleştirirken göz önünde bulundurun.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


