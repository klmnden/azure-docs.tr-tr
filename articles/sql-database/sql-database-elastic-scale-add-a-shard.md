---
title: "Esnek veritabanı araçlarını kullanarak bir parça ekleme | Microsoft Docs"
description: "Esnek ölçeklendirme API'leri yeni parça bir parça eklemek için nasıl kullanılacağını ayarlayın."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 62a349db-bebe-406f-a120-2f1986f2b286
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 7432bf00aa4d97d8dbc4b92a6a836698ee2b84d7
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a>Esnek veritabanı araçlarını kullanarak bir parça ekleme
## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Yeni aralık veya anahtarı için bir parça eklemek için
Uygulamalar genellikle yalnızca yeni anahtarlar veya zaten bir parça eşleme için anahtar aralıklarına beklenen verileri işlemek için yeni parça eklemeniz gerekir. Örneğin, Kiracı kimliği tarafından parçalı bir uygulama için yeni bir kiracı yeni bir parça sağlamak gerekebilir veya veri parçalı aylık yeni her ayın başlangıcını önce sağlanan yeni bir parça gerekebilir. 

Yeni anahtar değerleri aralığı zaten varolan bir eşleme parçası değilse, yeni parça eklemek ve yeni bir anahtar ya da bu parça aralığa ilişkilendirmek çok daha kolaydır. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Örnek: varolan bir parça eşlemesini bir parça ve kendi aralığı ekleme
Bu örnekte [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) yöntemleri ve bir örneğini oluşturur [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.)sınıfı. Aşağıda, adlı bir veritabanı örneğinde **sample_shard_2** ve içindeki tüm gerekli şema nesneleri [300, 400). aralık tutmak için oluşturuldu  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Alternatif olarak, yeni parça eşleme Yöneticisi oluşturmak için PowerShell'i kullanabilirsiniz. Bir örneği, kullanılabilir [burada](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Var olan bir aralığı boş bir kısmı için bir parça eklemek için
Bazı durumlarda, bir parça için bir aralığı zaten eşlenmiş ve kısmen verilerle doldurulur olabilirsiniz, ancak şimdi için farklı bir parça yönlendirilmesi için yaklaşan veri istiyorsunuz. Örneğin, parça gün aralığı tarafından ve parça için zaten ayrılmış 50 gününüz vardır, ancak gelecekteki verilerin içinde farklı bir parça güden istediğiniz günde 24. Esnek veritabanı [bölünmüş Birleştirme aracı](sql-database-elastic-scale-overview-split-and-merge.md) bu işlemi gerçekleştirebilir, ancak veri taşıma yani (örneğin, [25, 50 gün aralığı için veri), gerekli değilse gün 25 özel, 50 (dahil) henüz yok), bu gerçekleştirebilirsiniz tamamen parça eşleme Yönetimi API'leri kullanarak doğrudan.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Örnek: bir aralık bölme ve yeni eklenen bir parça boş bölümü atama
"Sample_shard_2" ve içindeki tüm gerekli şema nesneleri adlı bir veritabanı oluşturuldu.  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 

        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Önemli**: aralığının eminseniz güncelleştirilmiş eşleme boş ise yalnızca bu tekniği kullanın.  Denetimleri kodunuzda dahil etmek en iyisidir yukarıdaki yöntemleri taşınan, aralığı için veri denetlemez.  Satırlar taşınan aralığında varsa, gerçek veri dağıtım güncelleştirilmiş parça eşleme eşleşmez. Kullanım [bölünmüş Birleştirme aracı](sql-database-elastic-scale-overview-split-and-merge.md) bu gibi durumlarda bunun yerine işlemi gerçekleştirmek için.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

