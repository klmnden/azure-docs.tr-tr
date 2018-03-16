---
title: "Ölçeği genişletme varolan veritabanlarını geçirme | Microsoft Docs"
description: "Harita Yöneticisi bir parça oluşturarak esnek veritabanı araçlarını kullanmak için parçalı veritabanları Dönüştür"
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 10/24/2016
ms.author: sstein
ms.openlocfilehash: d82994f3ab925fa3ace0d0dbe1631a01dd1df586
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="migrate-existing-databases-to-scale-out"></a>Ölçeği genişletme varolan veritabanlarını geçirme
Azure SQL Database veritabanı araçları kullanarak mevcut ölçeklendirilmiş parçalı veritabanlarınız kolayca yönetmenize (gibi [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md)). Varolan kümesini kullanmak için veritabanlarının dönüştürmeniz [parça eşleme Yöneticisi](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Genel Bakış
Var olan bir parçalı veritabanı geçirmek için: 

1. Hazırlama [parça eşleme Yöneticisi veritabanı](sql-database-elastic-scale-shard-map-management.md).
2. Parça eşleme oluşturun.
3. Tek tek parça hazırlayın.  
4. Eşlemeleri parça eşlemeye ekleyin.

Bu teknikler kullanılarak uygulanabilir [.NET Framework istemci Kitaplığı](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), veya PowerShell komut dosyaları bulunan [Azure SQL veritabanı - esnek veritabanı araçlarını betikleri](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). Burada örnek PowerShell komut dosyalarını kullanın.

ShardMapManager hakkında daha fazla bilgi için bkz: [parça eşleme Yönetim](sql-database-elastic-scale-shard-map-management.md). Esnek veritabanı araçlarını genel bakış için bkz: [esnek veritabanı özelliklere genel bakış](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Parça eşleme manager veritabanını hazırlama
Parça eşleme Yöneticisi ölçeklendirilmiş veritabanlarını yönetmek için verileri içeren özel bir veritabanıdır. Varolan bir veritabanını kullanın veya yeni bir veritabanı oluşturun. Aynı veritabanında bir parça parça eşleme Yöneticisi olarak davranan bir veritabanı olmamalıdır. PowerShell Betiği veritabanını sizin yerinize oluşturmaz. 

## <a name="step-1-create-a-shard-map-manager"></a>1. adım: bir parça eşleme Yöneticisi oluşturma
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Parça eşleme Yöneticisi'ni almak için
Oluşturulduktan sonra Bu cmdlet'i parça eşleme Yöneticisi alabilir. ShardMapManager nesnesini kullanmak gereken her zaman bu adım gereklidir.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-the-shard-map"></a>2. adım: parça eşleme oluşturma
Parça harita oluşturmak için türünü seçin. Seçimi veritabanı mimarisine bağlıdır: 

1. Veritabanı başına tek bir kiracı (koşulları için bkz: [sözlüğü](sql-database-elastic-scale-glossary.md).) 
2. Birden çok Kiracı veritabanı (iki tür) başına:
   1. Liste eşleme
   2. Aralık eşleme

Tek Kiracı model için oluşturduğunuz bir **listesi eşleme** parça eşleme. Tek kiracılı bir model Kiracı başına tek bir veritabanı atar. Yönetimini basitleştirir gibi SaaS geliştiriciler için etkili bir modeli budur.

![Liste eşleme][1]

Çok kiracılı bir model için tek bir veritabanı çeşitli kiracılar atar (ve kiracılar grupları birden çok veritabanı arasında dağıtabilirsiniz). Bu model, küçük veri gereksinimlerini sağlamak için her bir kiracı beklediğiniz kullanın. Bu modelde, kiracılar aralığını kullanarak bir veritabanı atamak **aralığı eşleme**. 

![Aralık eşleme][2]

Veya, bir çok Kiracı veritabanı modelini kullanarak uygulayabileceğiniz bir *listesi eşleme* tek bir veritabanına birden çok kiracıya atamak için. Örneğin, DB1 Kiracı kimliği 1 ve 5 hakkında bilgi depolamak için kullanılır ve DB2 7 Kiracı ve Kiracı 10 verilerini depolar. 

![Birden çok kiracıya tek DB hakkında][3] 

**Seçtiğiniz bağlı olarak, aşağıdaki seçeneklerden birini seçin:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Seçenek 1: bir liste eşlemesi için bir parça eşleme oluşturma
ShardMapManager nesnesi kullanılarak bir parça Haritası oluşturun. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Seçenek 2: bir parça eşlemesi için bir aralığı eşlemesi oluşturma
Bu eşleme deseni kullanmaz, Kiracı kimliği değerleri için sürekli aralıkları olması gerekir ve veritabanları oluşturulurken aralığı atlayarak aralıklardaki boşluk sağlamak için kabul edilebilir.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Seçenek 3: tek bir veritabanı üzerinde eşlemeleri listesi
Bu deseni oluşturan ayarı ayrıca bir liste harita oluşturulmasını adım 2, 1. seçenek gösterildiği gibi gerektirir.

## <a name="step-3-prepare-individual-shards"></a>3. adım: tek tek parça hazırlama
(Veritabanı) her parça parça eşleme Manager'a ekleyin. Bu, tek veritabanlarını eşleme bilgilerini depolamak için hazırlar. Bu yöntem her parça üzerinde yürütün.

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.


## <a name="step-4-add-mappings"></a>4. adım: eşlemeleri ekleme
Eşlemeleri eklenmesi, oluşturduğunuz parça eşleme türüne bağlıdır. Bir liste harita oluşturduysanız, liste eşlemeleri ekleyin. Bir aralık harita oluşturduysanız, aralık eşlemeleri ekleyin.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Seçenek 1: bir liste eşlemesi için verileri eşleme
Verileri her bir kiracı için bir liste eşlemesi ekleyerek eşleyin.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Seçenek 2: bir aralık eşlemesi için verileri eşleme
Tüm Kiracı kimliği aralığı - veritabanı ilişkilendirmeleri aralığı eşlemelerini ekleyin:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Adım 4 seçenek 3: tek bir veritabanı üzerinde birden çok Kiracı için verileri eşleme
Her bir kiracı için Add-ListMapping (1. seçenek) çalıştırın. 

## <a name="checking-the-mappings"></a>Eşlemeleri denetleniyor
Varolan parça ve bunlarla ilişkilendirilmiş eşlemeleri hakkında bilgi komutları kullanarak sorgulanabilir:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Özet
Kurulum tamamlandığında, esnek veritabanı istemci kitaplığını kullanmaya başlayabilirsiniz. Aynı zamanda [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) ve [çok parça sorgu](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Sonraki adımlar
Gelen PowerShell komut dosyalarını almak [Azure SQL DB esnek Veritabanı Araçları komut dosyaları](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

Ayrıca Github'da araçlardır: [Azure/esnek-db-tools](https://github.com/Azure/elastic-db-tools).

Tek kiracılı bir model için çok kiracılı bir model bilgisayardan veya verileri taşımak için bölme birleştirme aracını kullanın. Bkz: [bölünmüş Birleştirme aracı](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Ek kaynaklar
Çok kiracılı hizmet olarak yazılım (SaaS) veritabanı uygulamalarının ortak veri mimarisi düzenlerine ilişkin bilgi için bkz. [Azure SQL Database ile Çok Kiracılı SaaS Uygulamaları için Tasarım Düzenleri](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Sorular ve özellik istekleri
Sorular için kullanmak [SQL veritabanı Forumu](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) ve özellik istekleri için bunları Ekle [SQL veritabanı geri bildirim Forumunda](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

