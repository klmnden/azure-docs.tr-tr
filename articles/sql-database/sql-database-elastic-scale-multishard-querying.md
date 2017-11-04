---
title: "Sorgu parçalı Azure SQL veritabanları | Microsoft Docs"
description: "Esnek veritabanı istemci kitaplığı kullanılarak parça sorgular çalıştırın."
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: a4379c15-f213-4026-ab6f-a450ee9d5758
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2016
ms.author: torsteng
ms.openlocfilehash: b4827fafdfd2f8b094c4f541a7511d6233dd4e6f
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="multi-shard-querying"></a>Çok parça sorgulama
## <a name="overview"></a>Genel Bakış
İle [esnek veritabanı araçlarını](sql-database-elastic-scale-introduction.md), parçalı veritabanı çözümleri oluşturabilirsiniz. **Çok parça sorgulama** koleksiyonu/bir sorgu çalıştırılarak gerektiren raporlama verilerini birden fazla parça uzatır gibi görevler için kullanılır. (Bu Karşıtlık [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md), tek bir parça tüm çalışma gerçekleştirir.) 

1. Alma bir [ **RangeShardMap** ](https://msdn.microsoft.com/library/azure/dn807318.aspx) veya [ **ListShardMap** ](https://msdn.microsoft.com/library/azure/dn807370.aspx) kullanarak [ **TryGetRangeShardMap** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [ **TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), veya [ **GetShardMap** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) yöntemi. Bkz: [ **bir ShardMapManager oluşturma** ](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) ve [ **RangeShardMap veya ListShardMap almak**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Oluşturma bir  **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)**  nesnesi.
3. Oluşturma bir  **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
4. Ayarlama  **[CommandText özelliği](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)**  T-SQL komutu.
5. Çağırarak komutu yürütün  **[ExecuteReader yöntemi](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
6. Kullanarak sonuçları görüntülemek  **[MultiShardDataReader sınıfı](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Örnek
Aşağıdaki kodu kullanarak sorgulama yapmayı çok parça kullanımını göstermektedir bir verilen **ShardMap** adlı *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
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


En önemli fark, çok parça bağlantılarının yapıdır. Burada **SqlConnection** tek bir veritabanı üzerinde çalışır **MultiShardConnection** geçen bir ***parça koleksiyonunu*** giriş olarak. Parça parça eşlemesinden koleksiyonunu doldurun. Sorgu kullanarak parça koleksiyonunu yürütülür **UNION ALL** semantiği tek bir genel sonuç birleştirin. İsteğe bağlı olarak, satırın kaynaklandığı parça adını kullanarak çıktı eklenebilir **ExecutionOptions** özelliği komutu. 

Çağrı Not **myShardMap.GetShards()**. Bu yöntem tüm parça parça eşlemesinden alır ve tüm ilgili veritabanları arasında bir sorgu çalıştırmak için kolay bir yol sağlar. Parça koleksiyonunu çok parça sorgu Gelişmiş için daha fazla koleksiyon üzerinde bir LINQ sorgusu gerçekleştirerek sağlayıcıdan döndürülen çağrısı **myShardMap.GetShards()**. Kısmi sonuçlar İlkesi ile birlikte, çok parça sorgulama içinde geçerli yetenek parça yüzlerce kadar onlarca için iyi çalışmak üzere tasarlanmıştır.

Çok parça sorgulama ile bir sınırlama şu anda doğrulama parça ve sorgulanır shardlets yetersizliğidir. Veri bağımlı yönlendirme sorgulama sırasında verilen parça parça eşleme parçası olduğunu doğrularken çok parça sorguları bu denetimi gerçekleştirme. Bu parça eşlemesinden kaldırılan veritabanları üzerinde çalışan çok parça sorguları neden olabilir.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Çok parça sorgular ve bölünmüş birleştirme işlemleri
Çok parça sorguları shardlets sorgulanan veritabanı üzerinde devam eden bölünmüş birleştirme işlemleri katılan olup olmadığını denetlemez. (Bkz [esnek veritabanı bölünmüş birleştirme aracını kullanarak ölçeklendirme](sql-database-elastic-scale-overview-split-and-merge.md).) Burada aynı çok parça sorguda birden çok veritabanları için aynı shardlet satırları göster bu tutarsızlıklar için yol açabilir. Bu sınırlamalara dikkat edin ve boşaltma devam eden bölünmüş birleştirme işlemleri ve parça eşleme değişiklikler çok parça sorguları gerçekleştirirken göz önünde bulundurun.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Ayrıca bkz.
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)**  sınıflar ve yöntemler.

Parça kullanarak yönetmek [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md). Adlı bir ad alanı içeren [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) tek bir sorgu ve sonuç kullanarak birden çok parça sorgu olanağı sağlar. Bu, bir parça koleksiyon sorgulanırken bir Özet sağlar. Ayrıca, alternatif yürütme ilkelerini, birçok parça sorgularken hata ile mücadele etmek için özellikle kısmi sonuçlar sağlar.  

