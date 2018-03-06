---
title: "Bir Azure SQL veritabanı ölçeklendirme | Microsoft Docs"
description: "Esnek veritabanı istemci kitaplığı ShardMapManager kullanma"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 0e9d647a-9ba9-4875-aa22-662d01283439
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: ddove
ms.openlocfilehash: fe4c8b7b2a9d199c85faf11fcd35382d586fc009
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="scale-out-databases-with-the-shard-map-manager"></a>Parça eşleme Yöneticisi veritabanlarıyla ölçeğini genişletme
SQL Azure veritabanlarını kolayca ölçeğini bir parça eşleme Yöneticisi'ni kullanın. Parça eşleme Yöneticisi'ni bir parça kümesindeki tüm parça (veritabanları) hakkında genel eşleme bilgilerini tutan özel bir veritabanıdır. Meta veri değeri alınarak doğru veritabanına bağlanmak bir uygulama sağlar **parçalama anahtar**. Ayrıca, yerel verileri izlemek haritalar kümesindeki her parça içerir (olarak bilinen **shardlets**). 

![Parça eşleme Yönetimi](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

Bu eşlemeler nasıl oluşturulur anlamanız parça eşleme yönetimi için önemlidir. Bu yapılır ShardMapManager sınıfını kullanma ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)), bulunan [esnek veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) parça eşlemeleri yönetmek için.  

## <a name="shard-maps-and-shard-mappings"></a>Parça eşlemeleri ve parça eşlemeleri
Her parça için oluşturmak için parça eşleme türü seçmelisiniz. Seçimi veritabanı mimarisine bağlıdır: 

1. Veritabanı başına tek bir kiracı  
2. Birden çok Kiracı veritabanı (iki tür) başına:
   1. Liste eşleme
   2. Aralık eşleme

Tek Kiracı model için oluşturduğunuz bir **listesi eşleme** parça eşleme. Tek kiracılı bir model Kiracı başına tek bir veritabanı atar. Yönetimini basitleştirir gibi SaaS geliştiriciler için etkili bir modeli budur.

![Liste eşleme][1]

Çok kiracılı bir model için tek bir veritabanı çeşitli kiracılar atar (ve kiracılar grupları birden çok veritabanı arasında dağıtabilirsiniz). Bu model, küçük veri gereksinimlerini sağlamak için her bir kiracı beklediğiniz kullanın. Bu modelde, kiracılar aralığını kullanarak bir veritabanı atamak **aralığı eşleme**. 

![Aralık eşleme][2]

Veya, bir çok Kiracı veritabanı modelini kullanarak uygulayabileceğiniz bir *listesi eşleme* tek bir veritabanına birden çok kiracıya atamak için. Örneğin, DB1 Kiracı kimliği 1 ve 5 hakkında bilgi depolamak için kullanılır ve DB2 7 Kiracı ve Kiracı 10 verilerini depolar. 

![Birden çok kiracıya tek DB hakkında][3] 

### <a name="supported-types-for-sharding-keys"></a>Parçalama anahtarları için desteklenen türler
Esnek ölçeklendirme parçalama anahtarları olarak aşağıdaki türlerini destekler:

| .NET | Java |
| --- | --- |
| tamsayı |tamsayı |
| uzun |uzun |
| GUID |UUID |
| Byte]  |Byte] |
| Tarih saat | timestamp |
| TimeSpan | Süre|
| Datetimeoffset |offsetdatetime |

### <a name="list-and-range-shard-maps"></a>Liste ve aralığı parça eşlemeleri
Parça eşlemeleri kullanarak olacak oluşturulan **tek tek parçalama listesi anahtar değerlerini**, veya kullanılarak oluşturulabilir **parçalama aralıklarına anahtar değerlerini**. 

### <a name="list-shard-maps"></a>Liste parça eşlemeleri
**Parça** içeren **shardlets** ve parça shardlets eşleme parça eşleme tarafından korunur. A **listesi parça eşleme** shardlets tanımlayan anahtar değerlerini ayrı ayrı ve parça hizmet veritabanları arasındaki ilişkidir.  **Liste eşlemeleri** açık ve farklı anahtar değerleri aynı veritabanını eşlenebilir. Örneğin, bir veritabanına anahtar 1 eşler ve anahtar değerlerinin 3 ile 6 veritabanı B. başvurusu

| Anahtar | Parça konumu |
| --- | --- |
| 1 |Database_A |
| 3 |Database_B |
| 4 |Database_C |
| 6 |Database_B |
| ... |... |

### <a name="range-shard-maps"></a>Aralık parça eşlemeleri
İçinde bir **aralığı parça eşleme**, anahtar aralığı çifti tarafından açıklanan **[düşük değeri, yüksek değerli)** nerede *düşük değer* aralığında en az bir anahtardır ve *yüksek değerli* aralığından daha yüksek ilk değer. 

Örneğin, **[0, 100)** 0 veya daha büyük ve 100'den küçük tüm tamsayılar içerir. Birden çok aralık aynı veritabanına işaret edebilir ve ayrık aralıkları (örneğin, [100,200) desteklenir unutmayın ve [400,600 her ikisi de noktası veritabanı C için aşağıdaki örnekte.))

| Anahtar | Parça konumu |
| --- | --- |
| [1,50) |Database_A |
| [50,100) |Database_B |
| [100,200) |Database_C |
| [400,600) |Database_C |
| ... |... |

Yukarıda gösterilen tabloların her biri kavramsal bir örneği olan bir **ShardMap** nesnesi. Her satırda tek bir Basitleştirilmiş örneğidir **PointMapping** (için liste parça eşleme) veya **RangeMapping** (için aralığı parça eşleme) nesnesi.

## <a name="shard-map-manager"></a>Parça eşleme Yöneticisi
İstemci Kitaplığı'nda parça eşleme Yöneticisi parça eşlemeleri koleksiyonudur. Tarafından yönetilen veri bir **ShardMapManager** örneği üç yerde tutulur: 

1. **Genel parça eşleme (GSM)**: tüm parça eşlemeleri ve eşlemeler için depo olarak hizmet verecek bir veritabanı belirtin. Özel tabloları ve saklı yordamlar bilgilerini yönetmek için otomatik olarak oluşturulur. Bu genellikle küçük bir veritabanı ve hafifçe erişilen ve diğer uygulama ihtiyaçları için kullanılmamalıdır. Adlı özel bir şemada tablolardır **__ShardManagement**. 
2. **Yerel parça eşleme (LSM)**: bir parça olmasını değişiklik birkaç küçük tablolar ve parça eşleme bilgilerini bu parça belirli yönetmek ve içeren özel saklı yordamlar içerecek şekilde belirttiğiniz her veritabanı. Bu bilgiler GSM bilgilerle yedekli ve önbelleğe alınan parça eşleme bilgisi GSM üzerinde hiçbir yük koymadan doğrulamak uygulama sağlar; Uygulama LSM önbelleğe alınmış bir eşleme hala geçerli olup olmadığını belirlemek için kullanır. Her parça üzerinde LSM karşılık gelen Ayrıca şemada tablolardır **__ShardManagement**.
3. **Uygulama önbelleği**: her uygulama örneği erişen bir **ShardMapManager** nesne eşlemelerini, yerel bir bellek içi önbellek tutar. Son alınan yönlendirme bilgilerini depolar. 

## <a name="constructing-a-shardmapmanager"></a>Bir ShardMapManager oluşturma
A **ShardMapManager** nesne bir Fabrika kullanarak oluşturulur ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager_factory), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory)) deseni. **ShardMapManagerFactory.GetSqlShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager_factory.getsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)) yöntemi (GSM tutan veritabanı adı ve sunucu adı dahil) kimlik bilgilerini alır formun bir **ConnectionString** ve bir örneğini döndüren bir **ShardMapManager**.  

**Lütfen dikkat edin:** **ShardMapManager** bir uygulama için başlatma kodundaki uygulama etki alanı başına yalnızca bir kez örneğinin oluşturulması. Aynı uygulama etki alanında ShardMapManager ek örneklerini oluşturulmasını artırılmış bellek ve CPU kullanımı uygulamanın ile sonuçlanır. A **ShardMapManager** parça eşlemelerinin herhangi bir sayı içerebilir. Bir tek parça eşleme birçok uygulama için yeterli olabilir, ancak veritabanlarının farklı kümeleri farklı şema veya benzersiz amacıyla kullanıldığında zamanlar vardır; Bu gibi durumlarda birden fazla parça eşlemeleri tercih edilebilir. 

Bu kod, bir uygulamanın var olan açmayı denediğinde **ShardMapManager** TryGetSqlShardMapManager ile ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager_factory.trygetsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)) yöntemi.  Varsa bir genel temsil eden nesneler **ShardMapManager** (GSM) sağlamadığı henüz veritabanının var, istemci kitaplığının bunları orada CreateSqlShardMapManager kullanarak oluşturur ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager_factory.createsqlshardmapmanager), [.NET ](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager)) yöntemi.

```Java
// Try to get a reference to the Shard Map Manager in the shardMapManager database.
// If it doesn't already exist, then create it.
ShardMapManager shardMapManager = null;
boolean shardMapManagerExists = ShardMapManagerFactory.tryGetSqlShardMapManager(shardMapManagerConnectionString,ShardMapManagerLoadPolicy.Lazy, refShardMapManager);
shardMapManager = refShardMapManager.argValue;

if (shardMapManagerExists) {
    ConsoleUtils.writeInfo("Shard Map %s already exists", shardMapManager);
}
else {
    // The Shard Map Manager does not exist, so create it
    shardMapManager = ShardMapManagerFactory.createSqlShardMapManager(shardMapManagerConnectionString);
    ConsoleUtils.writeInfo("Created Shard Map %s", shardMapManager);
}
```

```csharp
// Try to get a reference to the Shard Map Manager via the Shard Map Manager database.  
// If it doesn't already exist, then create it. 
ShardMapManager shardMapManager; 
bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString, 
                                        ShardMapManagerLoadPolicy.Lazy, 
                                        out shardMapManager); 

if (shardMapManagerExists) 
{ 
    Console.WriteLine("Shard Map Manager already exists");
} 
else
{
    // Create the Shard Map Manager. 
    ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
    Console.WriteLine("Created SqlShardMapManager"); 

    shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

// The connectionString contains server name, database name, and admin credentials for privileges on both the GSM and the shards themselves.
} 
```

.NET sürüm için yeni bir parça eşleme yönetici oluşturmak için PowerShell'i kullanabilirsiniz. Bir örneği, kullanılabilir [burada](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="get-a-rangeshardmap-or-listshardmap"></a>RangeShardMap veya ListShardMap Al
Bir parça eşleme Yöneticisi oluşturduktan sonra RangeShardMap alabilirsiniz ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807318.aspx)) veya ListShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._list_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807370.aspx)) kullanma TryGetRangeShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetrangeshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx)), TryGetListShardMap ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetlistshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)), ya da (GetShardMap[ Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.getshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx)) yöntemi.

```Java
// Creates a new Range Shard Map with the specified name, or gets the Range Shard Map if it already exists.
static <T> RangeShardMap<T> createOrGetRangeShardMap(ShardMapManager shardMapManager,
            String shardMapName,
            ShardKeyType keyType) {
    // Try to get a reference to the Shard Map.
    ReferenceObjectHelper<RangeShardMap<T>> refRangeShardMap = new ReferenceObjectHelper<>(null);
    boolean isGetSuccess = shardMapManager.tryGetRangeShardMap(shardMapName, keyType, refRangeShardMap);
    RangeShardMap<T> shardMap = refRangeShardMap.argValue;

    if (isGetSuccess && shardMap != null) {
        ConsoleUtils.writeInfo("Shard Map %1$s already exists", shardMap.getName());
    }
    else {
        // The Shard Map does not exist, so create it
        try {
            shardMap = shardMapManager.createRangeShardMap(shardMapName, keyType);
        }
        catch (Exception e) {
            e.printStackTrace();
        }
        ConsoleUtils.writeInfo("Created Shard Map %1$s", shardMap.getName());
    }

    return shardMap;
}
```

```csharp
// Creates a new Range Shard Map with the specified name, or gets the Range Shard Map if it already exists.
public static RangeShardMap<T> CreateOrGetRangeShardMap<T>(ShardMapManager shardMapManager, string shardMapName)
{
    // Try to get a reference to the Shard Map.
    RangeShardMap<T> shardMap;
    bool shardMapExists = shardMapManager.TryGetRangeShardMap(shardMapName, out shardMap);

    if (shardMapExists)
    {
        ConsoleUtils.WriteInfo("Shard Map {0} already exists", shardMap.Name);
    }
    else
    {
        // The Shard Map does not exist, so create it
        shardMap = shardMapManager.CreateRangeShardMap<T>(shardMapName);
        ConsoleUtils.WriteInfo("Created Shard Map {0}", shardMap.Name);
    }

    return shardMap;
} 
```

### <a name="shard-map-administration-credentials"></a>Parça eşleme yönetim kimlik bilgileri
Yönetmek ve parça eşlemeleri işlemek uygulamalar parça haritalar rota bağlantılarına kullananlar farklıdır. 

Parça eşlemeleri yönetmek için (ekleme veya değiştirme parça parça eşlemeleri, parça eşlemeleri, vb.), oluşturmalıdır **ShardMapManager** kullanarak **olan kimlik bilgilerini okuma/yazma ayrıcalıklarına GSM veritabanında ve bir parça hizmet veren her bir veritabanına**. Parça eşleme bilgisi girilen ya da üzerinde yeni parça LSM tablo oluşturma için olduğu gibi değiştirilmiş kimlik bilgilerini GSM ve LSM tablolarda karşı yazmalar için izin vermelidir.  

Bkz: [esnek veritabanı istemci kitaplığına erişmek için kullanılan kimlik bilgileri](sql-database-elastic-scale-manage-credentials.md).

### <a name="only-metadata-affected"></a>Yalnızca etkilenen meta veriler
Doldurma veya değiştirmek için kullanılan yöntemleri **ShardMapManager** veri parça içinde depolanan kullanıcı verilerini alter değil. Örneğin, yöntemler gibi **CreateShard**, **DeleteShard**, **UpdateMapping**, vb. parça eşlemesi meta verileri yalnızca etkiler. Bunlar kaldırmayın, ekleyebilir veya parça bulunan kullanıcı verileri alter. Bunun yerine, bu yöntemleri oluşturmak için gerçekleştirdiğiniz ayrı işlemleri ile birlikte kullanılmak üzere tasarlanmış veya Kaldır gerçek veritabanları veya satır bir parça parçalı bir ortamı yeniden dengelemeniz diğerine taşıyabilirsiniz.  ( **Bölünmüş birleştirme** esnek veritabanı araçları ile içerdiği aracı yapar parça arasında gerçek veri taşıma yönetme yanı sıra bu API'leri kullanın.) Bkz: [esnek veritabanı bölünmüş birleştirme aracını kullanarak ölçeklendirme](sql-database-elastic-scale-overview-split-and-merge.md).

## <a name="data-dependent-routing"></a>Verilere bağımlı yönlendirme
Parça eşleme Yöneticisi, uygulamaya özgü veri işlemleri gerçekleştirmek için veritabanı bağlantıları gerektiren uygulamalarda kullanılır. Bu bağlantılar doğru veritabanı ile ilişkilendirilmiş olması gerekir. Bu olarak bilinir **veri bağımlı yönlendirme**. Bu uygulamalar için GSM veritabanında salt okunur erişime sahip kimlik bilgilerini kullanarak Fabrika parça eşleme Yöneticisi nesnesinden örneği oluşturur. Daha sonraki bağlantılar için tek tek istekleri uygun parça veritabanına bağlanmak için gereken kimlik bilgilerini sağlayın.

Unutmayın bu uygulamaları (kullanarak **ShardMapManager** salt okunur kimlik bilgileriyle açılan) eşlemeleri veya eşlemeleri değişiklik yapılamaz. Bu ihtiyaçları için özel yönetim uygulamalarını ya da daha önce bahsedildiği gibi daha yüksek ayrıcalıklı kimlik bilgilerini sağlamanız PowerShell komut dosyaları oluşturun. Bkz: [esnek veritabanı istemci kitaplığına erişmek için kullanılan kimlik bilgileri](sql-database-elastic-scale-manage-credentials.md).

Daha fazla bilgi için bkz: [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md). 

## <a name="modifying-a-shard-map"></a>Parça eşlemeyi değiştirme
Parça eşleme farklı şekilde değiştirilebilir. Aşağıdaki yöntemlerden tümünde parça ve bu nesnelerin eşlemelerini açıklayan meta verilerini değiştirmek ancak fiziksel olarak veri parça içinde değiştirmeyin ve bunlar oluşturmak veya gerçek veritabanlarını silin.  Bazı işlemlerin aşağıda açıklanan parça harita üzerinde fiziksel olarak veri taşıma veya ekleyin ve parça hizmet veren veritabanlarını kaldırmanız yönetim işlemlerini koordine olmanız gerekebilir.

Bu yöntemler, genel dağıtım parçalı veritabanı ortamınızda veri değiştirmek için kullanılabilir yapı taşları olarak birlikte çalışır.  

* Eklemek veya parça kaldırmak için: kullanmak **CreateShard** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.createshard), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx)) ve **DeleteShard** ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map.deleteshard), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard.aspx)) Shardmap, ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._shard_map), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.aspx)) sınıfı. 
  
    Bu işlemleri yürütmek sunucuyu ve veritabanını hedef parça temsil eden varolmalıdır. Bu yöntemlerin herhangi bir etki veritabanlarına kendileri yalnızca meta veri parça eşlemesindeki üzerinde sahip değilsiniz.
* Oluşturmak veya noktaları veya parça eşlenen aralıklar kaldırmak için: kullanmak **CreateRangeMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.createrangemapping), [.NET](https://msdn.microsoft.com/library/azure/dn841993.aspx)), **DeleteMapping** () [Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.deletemapping), [.NET](https://msdn.microsoft.com/library/azure/dn824200.aspx)) RangeShardMapping, ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807318.aspx)) sınıfı ve **CreatePointMapping**  ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._list_shard_map.createpointmapping), [.NET](https://msdn.microsoft.com/library/azure/dn807218.aspx)) ListShardMap, ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._list_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn842123.aspx)) sınıfı.
  
    Birçok farklı noktaları veya aralıklar aynı parça eşlenebilir. Bu yöntemler, yalnızca meta veri etkiler - zaten parça içinde bulunabilecek herhangi bir veri etkilemez. Verileri veritabanından ile tutarlı olması için kaldırılması gerekiyorsa **DeleteMapping** işlemleri ayrı olarak, ancak bu yöntemleri kullanarak ile birlikte bu işlemleri.  
* Varolan aralıkları ikiye böl ya da bitişik aralıkları birine birleştirmek için: kullanmak **SplitMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.splitmapping), [.NET](https://msdn.microsoft.com/library/azure/dn824205.aspx)) ve **MergeMappings** () [Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.mergemappings), [.NET](https://msdn.microsoft.com/library/azure/dn824201.aspx)).  
  
    Bölme ve birleştirme işlemlerinin unutmayın **anahtar değerleri eşlendi parça değiştirmeyin**. Bölme iki bölüme var olan bir aralığı keser, ancak her ikisi de aynı parça eşlenmiş olarak bırakır. Bir birleştirme için tek bir aralığa birleştirmesi aynı parça zaten eşlenmiş iki bitişik aralıkları çalıştırır.  Noktaları veya aralıklar kendilerini parça arasında hareketini kullanarak Eşgüdümlü gerekiyor **UpdateMapping** gerçek veri taşıma birlikte.  Kullanabileceğiniz **bölünmüş/Merge** hizmet taşıması gerektiğinde parça eşleme veri taşıma değişikliklerle koordine etmek için esnek veritabanı araçlarını parçası. 
* Yeniden eşleme (veya taşımak için) tek tek noktaları ya da farklı parça aralıkları: kullanmak **UpdateMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.updatemapping), [.NET](https://msdn.microsoft.com/library/azure/dn824207.aspx)).  
  
    Verileri bir parça diğerine ile tutarlı olması için taşınacak gerekebileceği **UpdateMapping** işlemleri, o taşıma ayrı olarak, ancak bu yöntemleri kullanarak ile birlikte gerçekleştirmeniz gerekir.
* Eşlemeleri çevrimiçi ve çevrimdışı yapılacak: kullanmak **MarkMappingOffline** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.markmappingoffline), [.NET](https://msdn.microsoft.com/library/azure/dn824202.aspx)) ve **MarkMappingOnline** ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map.markmappingonline), [.NET](https://msdn.microsoft.com/library/azure/dn807225.aspx)) bir eşleme çevrimiçi durumunu denetlemek için. 
  
    Bir eşleme "Çevrimdışı" bir durumda olduğunda parça eşlemeleri üzerindeki belirli işlemlerin yalnızca verilir dahil olmak üzere **UpdateMapping** ve **DeleteMapping**. Bir eşleme çevrimdışı olduğunda, o eşlemeye dahil bir anahtarı temel alan bir veri bağımlı istek bir hata döndürür. Bir aralık önce çevrimdışı duruma getirildiğinde, ayrıca, etkilenen parça tüm bağlantıları otomatik olarak değiştirilmesini aralıkları karşı yönlendirilmiş sorgular için tutarsız veya tamamlanmamış sonuçları önlemek için sonlandırıldı. 

Eşlemeleri .net içinde değişmez nesneleridir.  Tüm eşlemelerini değiştirmek yukarıdaki yöntemlerden birini de kodunuzda bunları tüm başvuruları geçersiz. Operations zincirleme şekilde sıraları eşleme 's durumunu değiştirme işlemleri gerçekleştirmek kolaylaştırmak için tüm bir eşleme değiştirme yöntemleri yeni bir eşleme başvurusu döndürür. Örneğin, mevcut bir anahtarı 25 içeren shardmap sm içinde eşleme silmek için aşağıdaki yürütebilirsiniz: 

```
    sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));
```

## <a name="adding-a-shard"></a>Bir parça ekleme
Uygulamalar genellikle yeni anahtarlar veya zaten bir parça eşleme için anahtar aralıklarına beklenen verileri işlemek için yeni parça eklemeniz gerekir. Örneğin, Kiracı kimliği tarafından parçalı bir uygulama için yeni bir kiracı yeni bir parça sağlamak gerekebilir veya veri parçalı aylık yeni her ayın başlangıcını önce sağlanan yeni bir parça gerekebilir. 

Yeni anahtar değerleri aralığı zaten varolan bir eşleme parçası değil ve hiçbir veri taşıma gerekli ise, yeni parça eklemek ve yeni bir anahtar ya da bu parça aralığa ilişkilendirmek basit bir işlemdir. Yeni parça ekleme hakkında daha fazla bilgi için bkz: [yeni bir parça eklemek](sql-database-elastic-scale-add-a-shard.md).

Veri taşıma gerektiren senaryolar için gerekli parça eşleme güncelleştirmeleri ile birlikte parça arasında veri taşıma düzenlemek için bölünmüş Birleştirme aracı ancak gereklidir. Bölünmüş birleştirme Aracı'nı kullanma hakkında daha fazla bilgi için bkz [bölünmüş birleştirme genel bakış](sql-database-elastic-scale-overview-split-and-merge.md) 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
