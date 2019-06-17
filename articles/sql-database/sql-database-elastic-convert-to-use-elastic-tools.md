---
title: Ölçeği genişletmek için mevcut veritabanlarını geçirme | Microsoft Docs
description: Parça eşleme Yöneticisi oluşturarak esnek veritabanı araçlarını kullanmayı parçalı veritabanlarını dönüştürün
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
ms.openlocfilehash: 49686e407b2d733c04bad31706c6c4f315bf28bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61075216"
---
# <a name="migrate-existing-databases-to-scale-out"></a>Ölçeği genişletmek için mevcut veritabanlarını geçirme
Azure SQL veritabanı, veritabanı araçları kullanarak mevcut, ölçeği genişletilen parçalı veritabanlarını kolayca yönetin (gibi [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md)). Var olan bir veritabanı kullanmak için kümesi dönüştürmeniz [parça eşleme Yöneticisi](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Genel Bakış
Mevcut parçalı veritabanını geçirmek için: 

1. Hazırlama [parça eşleme Yöneticisi veritabanını](sql-database-elastic-scale-shard-map-management.md).
2. Parça Haritası oluşturun.
3. Tek parça hazırlayın.  
4. Eşlemeleri parça eşlemesine ekleyin.

Bu teknikler kullanarak uygulanabilir [.NET Framework istemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), veya PowerShell betikleri bulunan [Azure SQL veritabanı - elastik veritabanı araçları betikleri](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). Buradaki örnekler, PowerShell betikleri kullanın.

ShardMapManager hakkında daha fazla bilgi için bkz: [parça eşleme Yönetimi](sql-database-elastic-scale-shard-map-management.md). Esnek veritabanı araçlarını genel bakış için bkz. [elastik veritabanı özelliklerine genel bakış](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Parça eşleme Yöneticisi veritabanını hazırlama
Parça eşleme Yöneticisi, ölçeği genişletilmiş veritabanları yönetmek için gerekli verileri içeren özel bir veritabanıdır. Varolan veritabanını kullan veya yeni bir veritabanı oluşturun. Aynı veritabanında bir parça parça eşleme Yöneticisi görev yapan bir veritabanı olmamalıdır. PowerShell betiğini sizin için veritabanı oluşturmaz. 

## <a name="step-1-create-a-shard-map-manager"></a>1\. adım: bir parça eşleme Yöneticisi oluşturma
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Parça eşleme Yöneticisi almak için
Oluşturulduktan sonra Bu cmdlet ile parça eşleme Yöneticisi alabilirsiniz. ShardMapManager nesnesini kullanmak gereken her zaman bu adım gereklidir.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-the-shard-map"></a>2\. adım: parça eşlemesi oluşturma
Parça eşlemesi oluşturmak için türünü seçin. Seçtiğiniz veritabanı mimarisine bağlıdır: 

1. Veritabanı başına tek Kiracı (koşulları için bkz: [sözlüğü](sql-database-elastic-scale-glossary.md).) 
2. Birden fazla Kiracı başına veritabanı (iki tür):
   1. Liste eşlemesi
   2. Aralık eşleme

Tek kiracılı model için oluşturduğunuz bir **liste eşlemesi** parça eşlemesi. Tek kiracılı model, Kiracı başına bir veritabanı atar. Yönetimini basitleştirir gibi SaaS geliştiricileri için etkili bir modeldir budur.

![Liste eşlemesi][1]

Çok kiracılı model için tek bir veritabanının birden çok kiracıyı atar (ve birden fazla veritabanında kiracılar gruplarını dağıtabilirsiniz). Küçük veri gereksinimlerine sahip her bir kiracı beklediğiniz bu modeli kullanın. Bu modelde, kiracıların kullanarak bir veritabanına atama **aralığı eşleme**. 

![Aralık eşleme][2]

Veya, bir çok kiracılı veritabanı modeli kullanarak uygulayabileceğiniz bir *liste eşlemesi* birden fazla Kiracı için tek bir veritabanının atamak için. Örneğin, DB1 Kiracı kimliği 1 ve 5 hakkındaki bilgileri depolamak için kullanılır ve DB2 7 Kiracı ve Kiracı 10 verilerini depolar. 

![Birden çok kiracının tek DB][3] 

**Seçtiğiniz bağlı olarak, aşağıdaki seçeneklerden birini seçin:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>1\. seçenek: liste eşlemesi için parça Haritası oluşturma
ShardMapManager nesnesini kullanarak bir parça eşlemesi oluşturun. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>2\. seçenek: aralık eşlemesi için parça Haritası oluşturma
Bu eşleme düzeni kullanmaz, Kiracı kimliği değerleri için sürekli aralıkları olması gerekir ve kabul edilebilir aralığın veritabanlarını oluştururken atlayarak aralıklardaki uçurumuna sahip.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-an-individual-database"></a>Seçenek 3: Tek bir veritabanı üzerinde listesi eşlemeleri
Bu deseni oluşturan ayarı Ayrıca liste eşlemesi oluşturulmasını adım 2, 1. seçenek gösterildiği gerektirir.

## <a name="step-3-prepare-individual-shards"></a>3\. adım: Tek parça hazırlama
Her parça (veritabanı) için parça eşleme Yöneticisi ekleyin. Bu, tek veritabanlarını eşleme bilgilerini depolamak için hazırlar. Bu yöntem, her parça üzerinde yürütün.

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.


## <a name="step-4-add-mappings"></a>4\. Adım: Eşleme Ekle
Eşlemeleri eklenmesi, oluşturduğunuz aralık parça eşlemesi türüne bağlıdır. Bir liste eşlemesi oluşturduysanız, liste eşlemelerini ekleyin. Bir aralık harita oluşturduysanız, aralığı eşlemelerini ekleyin.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Seçenek 1: verileri bir liste eşlemesi için eşleme
Veriler, her Kiracı için bir liste eşlemesi ekleyerek eşleyin.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>2\. seçenek: verileri bir aralık eşlemesi için eşleme
Tüm Kiracı kimliği aralığı için - veritabanı ilişkileri aralığı eşlemelerini ekleyin:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-an-individual-database"></a>4\. adım seçenek 3: tek bir veritabanının birden çok kiracının verileri eşleme
Her Kiracı için Add-ListMapping (1. seçenek) çalıştırın. 

## <a name="checking-the-mappings"></a>Eşlemeleri denetleniyor
Var olan parça ve bunlarla ilişkili eşlemeleri hakkında bilgi, aşağıdaki komutları kullanarak sorgulanabilir:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Özet
Kurulumu tamamladıktan sonra elastik veritabanı istemci kitaplığını kullanmaya başlayabilirsiniz. Ayrıca [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) ve [çok parçalı sorgu](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Sonraki adımlar
PowerShell betiklerinden alma [Azure SQL veritabanı elastik veritabanı araçları betikleri](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

Araç, Github'da da vardır: [Azure/elastik-db-tools](https://github.com/Azure/elastic-db-tools).

Ayırma-Birleştirme aracı için bir tek kiracılı model veya çok kiracılı model aracılığıyla veri taşımak için kullanın. Bkz: [bölme-Birleştirme aracı](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Ek kaynaklar
Çok kiracılı hizmet olarak yazılım (SaaS) veritabanı uygulamalarının ortak veri mimarisi düzenlerine ilişkin bilgi için bkz. [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Düzenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Sorular ve özellik istekleri
Sorularınız için kullanmak [SQL veritabanının Forumu](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) ve özellik istekleri için ekleyebilmesi [SQL veritabanı geri bildirim Forumu](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

