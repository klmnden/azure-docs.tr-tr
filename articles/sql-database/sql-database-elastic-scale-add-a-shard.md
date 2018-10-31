---
title: Esnek veritabanı araçlarını kullanarak bir parça ekleme | Microsoft Docs
description: Yeni parçaları için parça ekleme için esnek ölçeklendirme API'leri kullanmayı ayarlayın.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 3be9723d0beddc6bfcd6b6a34b56f4b8fd80aa31
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239942"
---
# <a name="adding-a-shard-using-elastic-database-tools"></a>Esnek veritabanı araçlarını kullanarak bir parça ekleme
## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Yeni aralık veya anahtarı için bir parça eklemek için
Uygulamalar genellikle yeni anahtarlar veya zaten bir parça eşlemesi için anahtar aralıklarına beklenen verileri işlemek için yeni parçalara eklemeniz gerekir. Örneğin, Kiracı Kimliğine göre parçalı bir uygulama için yeni bir kiracı yeni bir parça sağlaması gerekebilir veya veri parçalı aylık sağlanan her yeni ayın başlangıcını önce yeni bir parça gerekebilir. 

Yeni anahtar değerleri aralığı zaten varolan bir eşleme parçası değilse, yeni parça eklemek ve yeni bir anahtar ya da bu parçayı aralığa ilişkilendirmek basit bir işlemdir. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Örnek: var olan bir parça eşlemesine bir parça ve aralığı ekleme
Bu örnek TryGetShard kullanır ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.trygetshard), [.NET](https://msdn.microsoft.com/library/azure/dn823929.aspx)) CreateShard ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.createshard), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)), CreateRangeMapping ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.createrangemapping), [.NET](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\))) yöntemleri ve ShardLocation örneği oluşturur ([Java](/java/api/com.microsoft.azure.elasticdb.shard.base._shard_location), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.)) sınıfı. Aşağıda, adlı bir veritabanı örneğinde **sample_shard_2** ve içindeki tüm gerekli şema nesneleri [300, 400). aralık tutmak için oluşturulmuş  

```csharp
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
```

.NET sürüm için de PowerShell alternatif olarak yeni bir parça eşleme Yöneticisi oluşturmak için kullanabilirsiniz. Bir örnek kullanılabilir [burada](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Var olan bir aralığı boş bir parçası için parça ekleme
Bazı durumlarda, bir aralık bir parçaya zaten eşlenmiş ve veri ile dolu kısmen olabilirsiniz, ancak şimdi farklı bir parçaya yönlendirilmesine yaklaşan veri istiyorsunuz. Örneğin, parça gün aralığı tarafından ve bir parçaya önceden ayrılmış 50 gününüz vardır, ancak farklı bir parçada yerleşmesi gelecekteki verilerle istediğiniz günü 24. Elastik veritabanı [bölme-birleştirme aracını](sql-database-elastic-scale-overview-split-and-merge.md) bu işlemi gerçekleştirebilir, ancak veri taşıma diğer bir deyişle (örneğin, veriler [25, 50 gün aralığı için), gerekli değilse, günde 25 50 özel, kapsamlı henüz mevcut değil), bu gerçekleştirebilirsiniz yalnızca doğrudan parça eşleme Yönetimi API'leri kullanarak.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Örnek: bir aralık bölme ve yeni eklenen bir parçaya boş bölümü atama
"Sample_shard_2" ve içindeki tüm gerekli şema nesneleri adlı bir veritabanı oluşturuldu.  

```csharp
// sm is a RangeShardMap object.
// Add a new shard to hold the range we will move 
Shard shard2 = null; 

if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
{ 
    shard2 = sm.CreateShard(new ShardLocation(shardServer,"sample_shard_2"));  
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
```

**Önemli**: aralığının eminseniz güncelleştirilmiş eşleme boş ise yalnızca bu tekniği kullanın.  Kodunuzda denetimler şunlardır en iyisidir önceki yöntemler taşınan, aralığı için veri denetlemez.  Satırlar taşınan aralığında varsa, gerçek veri dağıtım güncelleştirilmiş parça eşlemesi eşleşmez. Kullanım [bölme-birleştirme aracını](sql-database-elastic-scale-overview-split-and-merge.md) işlemi yerine bu gibi durumlarda gerçekleştirmek.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

