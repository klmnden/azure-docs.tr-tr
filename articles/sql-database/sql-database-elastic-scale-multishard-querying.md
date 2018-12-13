---
title: Azure SQL veritabanlarını parçalı sorgulama | Microsoft Docs
description: Elastik veritabanı istemci kitaplığını kullanarak parçalar arasında sorguları çalıştırın.
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
ms.date: 10/05/2018
ms.openlocfilehash: bc0ca62699c21848e4d5fdc18bef292dce4634bf
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52863474"
---
# <a name="multi-shard-querying-using-elastic-database-tools"></a>Esnek veritabanı araçlarını kullanarak çok parçalı sorgulama

## <a name="overview"></a>Genel Bakış

İle [esnek veritabanı araçlarını](sql-database-elastic-scale-introduction.md), parçalı veritabanı çözümleri oluşturabilirsiniz. **Çok parçalı sorgulama** koleksiyon/çalışan bir sorgu gerektiren raporlama verilerini birden fazla parçaya uzatır gibi görevler için kullanılır. (Bu Karşıtlık [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md), tek bir parçanın tüm çalışma gerçekleştirir.) 

1. Alma bir **RangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807318.aspx)) veya **ListShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._list_shard_map), [.NET ](https://msdn.microsoft.com/library/azure/dn807370.aspx)) kullanarak **TryGetRangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetrangeshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx)), **TryGetListShardMap** ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetlistshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)), veya **GetShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.getshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx)) yöntemi. Bkz: **[bir ShardMapManager oluşturmak](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager)** ve  **[RangeShardMap veya ListShardMap](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap)**.
2. Oluşturma bir **MultiShardConnection** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_connection), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)) nesne.
3. Oluşturma bir **MultiShardStatement veya MultiShardCommand** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)). 
4. Ayarlama **CommandText özelliğinin** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)) için bir T-SQL komutu.
5. Çağırarak bağlamını **ExecuteQueryAsync veya ExecuteReader** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement.executeQueryAsync), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)) yöntemi.
6. Kullanarak sonuçları görüntülemek **MultiShardResultSet veya MultiShardDataReader** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_result_set), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)) sınıfı. 

## <a name="example"></a>Örnek

Aşağıdaki kodu kullanarak sorgulama çok parçalı kullanımını bir verilen **ShardMap** adlı *myShardMap*. 

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

Önemli bir fark bağlantı çok parçalı bir yapıdır. Burada **SqlConnection** tek bir veritabanı üzerinde çalışır **MultiShardConnection** götüren bir ***parçalar koleksiyonu*** giriş olarak. Parça parça eşlemesinden koleksiyonunu doldurur. Sorgu kullanarak parçalar koleksiyonu üzerinde yürütülür **UNION ALL** semantiği tek bir genel sonuç derlemek için. İsteğe bağlı olarak satır kaynaklandığı parça adını kullanarak çıkış eklenebilir **ExecutionOptions** özelliği komutu. 

Çağrı Not **myShardMap.GetShards()**. Bu yöntem, tüm parça parça eşlemesinden alır ve tüm ilgili veritabanları arasında sorgu çalıştırmak için kolay bir yol sağlar. Parçalar koleksiyonu çok parçalı sorgu daraltılmış olabilir. daha fazla koleksiyon üzerinde bir LINQ Sorgu gerçekleştirerek döndürülen çağrısından **myShardMap.GetShards()**. Kısmi sonuçlar İlkesi ile birlikte, çok parçalı sorgulama geçerli özellik parçalar yüzlerce en fazla on için iyi çalışacak şekilde tasarlanmıştır.

Çok parçalı sorgulama ile bir şu anda doğrulama parçalar ve sorgulanır parçacıklarda eksikliği sınırlamasıdır. Verilere bağımlı yönlendirme sorgulama sırasında verilen parça parça eşlemesinin bir parçası olduğunu doğrular, ancak çok parçalı sorgular, bu onay gerçekleştirmeyin. Bu çok parçalı sorgular parça eşlemesinden kaldırılmış olan veritabanlarında çalışan neden olabilir.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Çok parçalı sorgular ve ayırma-birleştirme işlemleri

Çok parçalı sorgular parçacıklara sorgulanan veritabanı üzerinde devam eden bölme-birleştirme işlemleri katılan olup olmadığını doğrulamayın. (Bkz [elastik veritabanı bölme-birleştirme aracını kullanarak ölçeklendirme](sql-database-elastic-scale-overview-split-and-merge.md).) Burada aynı çok parçalı sorguda birden çok veritabanları için aynı parçacık satırları göster bu tutarsızlıklara yol açabilir. Bu sınırlamaları unutmayın ve boşaltma bölme-birleştirme işlemleri ve parça eşlemesi değişiklikler çok parçalı sorgular gerçekleştirirken göz önünde bulundurun.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]