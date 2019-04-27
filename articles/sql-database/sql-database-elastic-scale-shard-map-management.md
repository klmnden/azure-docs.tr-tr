---
title: Azure SQL veritabanını ölçeklendirme | Microsoft Docs
description: ShardMapManager, elastik veritabanı istemci kitaplığı kullanma
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
ms.openlocfilehash: a9c857ab9e9a3cfc0d1314600b612c4e6293173d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60332338"
---
# <a name="scale-out-databases-with-the-shard-map-manager"></a>Veritabanları parça eşleme Yöneticisi ile ölçeklendirme

SQL Azure veritabanlarında kolayca ölçeğini genişletmek için bir parça eşleme yöneticisini kullanın. Parça eşleme Yöneticisi bir parça kümesindeki tüm parçalar (veritabanları) hakkında genel eşleme bilgilerini tutan özel bir veritabanıdır. Meta veri değeri alınarak doğru veritabanına bağlanmak bir uygulama sağlar **parçalama anahtarı**. Ayrıca, yerel parça verileri izlemek haritalar kümesindeki her parça içeren (olarak bilinen **parçacıklara**).

![Parça eşleme Yönetimi](./media/sql-database-elastic-scale-shard-map-management/glossary.png)

Bu eşlemeler nasıl oluşturulduğu anlama, parça eşleme yönetimi için gereklidir. Bu yapılır ShardMapManager sınıfını kullanarak ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager)bölümüyle [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) parça eşlemeleri yönetmek için.  

## <a name="shard-maps-and-shard-mappings"></a>Parça eşlemeleri ve parça eşlemeleri

Her parça için aralık parça eşlemesi oluşturmak için türü seçmeniz gerekir. Seçtiğiniz veritabanı mimarisine bağlıdır:

1. Veritabanı başına tek Kiracı  
2. Birden fazla Kiracı başına veritabanı (iki tür):
   1. Liste eşlemesi
   2. Aralık eşleme

Tek kiracılı model için oluşturduğunuz bir **liste eşlemesi** parça eşlemesi. Tek kiracılı model, Kiracı başına bir veritabanı atar. Yönetimini basitleştirir gibi SaaS geliştiricileri için etkili bir modeldir budur.

![Liste eşlemesi][1]

Çok kiracılı model için tek bir veritabanının birden çok kiracıyı atar (ve birden fazla veritabanında kiracılar gruplarını dağıtabilirsiniz). Küçük veri gereksinimlerine sahip her bir kiracı beklediğiniz bu modeli kullanın. Bu modelde, kiracıların kullanarak bir veritabanına atama **aralığı eşleme**.

![Aralık eşleme][2]

Veya, bir çok kiracılı veritabanı modeli kullanarak uygulayabileceğiniz bir *liste eşlemesi* birden fazla Kiracı için tek bir veritabanının atamak için. Örneğin, DB1 Kiracı kimliği 1 ve 5 hakkındaki bilgileri depolamak için kullanılır ve DB2 7 Kiracı ve Kiracı 10 verilerini depolar.

![Birden çok kiracının tek DB][3]

### <a name="supported-types-for-sharding-keys"></a>Parçalama anahtarları için desteklenen türler:

Esnek ölçek, parçalama anahtarı olarak aşağıdaki türlerini destekler:

| .NET | Java |
| --- | --- |
| integer |integer |
| uzun |uzun |
| GUID |uuid |
| byte[]  |byte[] |
| datetime | timestamp |
| TimeSpan | süre|
| Datetimeoffset |offsetdatetime |

### <a name="list-and-range-shard-maps"></a>Liste ve aralık parça eşlemesi

Parça eşlemesi kullanma değişkenden oluşan **bireysel parçalama listesi anahtar değerlerini**, veya kullanılarak oluşturulabilir **parçalama aralıklarını anahtar değerlerini**.

### <a name="list-shard-maps"></a>Liste parça eşlemesi

**Parçalar** içeren **parçacıklara** ve Parçacıkların parçalara eşleme parça eşlemesi tarafından korunur. A **liste parça eşlemesi** parçacıklara tanımlamak anahtar değerlerine ve parçalar olarak hizmet veritabanları arasındaki ilişkidir.  **Liste eşlemeleri** açık ve farklı anahtar değerlerini aynı veritabanını eşlenebilir. Örneğin, anahtar değeri 1 her iki veritabanı bir b eşlenir veritabanı bir ve anahtar değerlerini 3 ve 6 eşler

| Anahtar | Parça konumu |
| --- | --- |
| 1 |Database_A |
| 3 |Database_B |
| 4 |Database_C |
| 6 |Database_B |
| ... |... |

### <a name="range-shard-maps"></a>Aralık parça eşlemesi

İçinde bir **aralık parça eşlemesi**, anahtar aralığı bir çifti tarafından tanımlanan **[düşük değeri, yüksek değer)** burada *düşük değer* aralıktaki en düşük anahtarıdır ve *yüksek Değer* aralığından daha yüksek ilk değer.

Örneğin, **[0, 100)** tüm tamsayıların en az 0 ve 100'den küçük içerir. Birden çok aralık aynı veritabanına işaret edebilir ve ayrık aralıkları (örneğin, [100,200) desteklenen unutmayın ve [400,600 hem noktası için veritabanı C aşağıdaki örnekte.))

| Anahtar | Parça konumu |
| --- | --- |
| [1,50) |Database_A |
| [50,100) |Database_B |
| [100,200) |Database_C |
| [400,600) |Database_C |
| ... |... |

Yukarıda gösterilen tabloların her biri kavramsal bir örneği olan bir **ShardMap** nesne. Her satır bir bireyin basitleştirilmiş bir örnektir **PointMapping** (için liste parça eşlemesi) veya **RangeMapping** (için aralık parça eşlemesi) nesne.

## <a name="shard-map-manager"></a>Parça eşleme Yöneticisi

İstemci Kitaplığı'ndaki parça eşleme Yöneticisi, parça eşlemesi bir koleksiyonudur. Tarafından yönetilen veri bir **ShardMapManager** örnek, üç yerde tutulur:

1. **Genel parça eşleme (GSM)**: Tüm parça eşlemesi ve eşlemeler için deposu olarak görev yapacak bir veritabanı belirtin. Özel tablolarına ve depolanmış yordamlarına bilgilerini yönetmek için otomatik olarak oluşturulur. Bu genellikle küçük bir veritabanıdır ve az erişilen ve diğer uygulama gereksinimleri için kullanılmamalıdır. Adlı özel bir şemada tablolarıdır **__ShardManagement**.
2. **Yerel parça eşlemesinin (LSM)**: Bir parça olarak belirttiğiniz her veritabanı, birkaç küçük tablolar ve parça eşlemesi bilgilerini bu parçaya belirli yönetmek ve içeren özel saklı yordamlar içerecek biçimde değiştirilir. Bu bilgileri, bilgilerle gsm'deki gereksizdir ve GSM üzerinde her türlü yük getirmeden önbelleğe alınmış parça eşleme bilgileri doğrulamak için uygulamayı verir; Uygulama LSM önbelleğe alınmış bir eşleme hala geçerli olup olmadığını belirlemek için kullanır. Her parça üzerinde LSM karşılık gelen tablolar da şemasında bulunan **__ShardManagement**.
3. **Uygulama önbelleği**: Her örnek uygulamaya erişirken bir **ShardMapManager** nesnesi, yerel bir bellek içi önbellek, eşlemeleri tutar. Bu, son alınan yönlendirme bilgilerini depolar.

## <a name="constructing-a-shardmapmanager"></a>Bir ShardMapManager oluşturma

A **ShardMapManager** Fabrika kullanarak nesnesi oluşturulduğunda ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory)) deseni. **ShardMapManagerFactory.GetSqlShardMapManager** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.getsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager)) yöntemini alır (GSM tutan veritabanı adı ve sunucu adı dahil) kimlik bilgileri biçiminin bir **ConnectionString** ve örneğini döndüren bir **ShardMapManager**.  

**Lütfen Unutmayın:** **ShardMapManager** içinde bir uygulama başlatma kodunu uygulama etki alanı başına yalnızca bir kez örneği. Aynı uygulama etki alanında ShardMapManager ek örneklerini oluşturulmasını, artan bellek ve CPU kullanımı uygulamanın sonuçlanır. A **ShardMapManager** herhangi bir sayıda parça eşlemesi içerebilir. Tek parça eşlemesi birçok uygulama için yeterli olabilir, ancak farklı bir veritabanları kümesi farklı bir şema veya benzersiz amacıyla kullanıldığında zamanlar vardır; Bu gibi durumlarda birden çok parça eşlemesi tercih edilebilir.

Bu kodda, uygulamanın mevcut bir açmayı dener **ShardMapManager** TryGetSqlShardMapManager ile ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.trygetsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager) yöntemi. Varsa bir Global temsil eden nesneler **ShardMapManager** (GSM) olmayan henüz içinde veritabanı mevcut, istemci kitaplığının CreateSqlShardMapManager kullanarak bunları oluşturur ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanagerfactory.createsqlshardmapmanager), [.NET](/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager)) yöntem.

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

.NET sürüm için yeni bir parça eşleme Yöneticisi oluşturmak için PowerShell kullanabilirsiniz. Bir örnek kullanılabilir [burada](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="get-a-rangeshardmap-or-listshardmap"></a>RangeShardMap veya ListShardMap Al

Parça eşleme Yöneticisi oluşturduktan sonra RangeShardMap alabilirsiniz ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) veya ListShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.listshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.listshardmap-1)) kullanma TryGetRangeShardMap ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.trygetrangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap)), TryGetListShardMap ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.trygetlistshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap)), ya da (GetShardMap[ Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.getshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap)) yöntemi.

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

### <a name="shard-map-administration-credentials"></a>Parça eşleme yönetimi kimlik bilgileri

Parça eşlemeleri yönetmek ve düzenleme uygulamalar rota bağlantılarına parça eşlemesi kullananlar farklıdır.

Parça eşlemeleri yönetmek için (ekleme veya değiştirme parçalar, parça eşlemesi, parça eşlemeleri, vb.), oluşturmalıdır **ShardMapManager** kullanarak **olan kimlik bilgilerini okuma/yazma ayrıcalıkları GSM veritabanında ve her bir parça görev yapan veritabanı**. Parça eşleme bilgisi girilen ya da yeni parçalar üzerinde LSM tablolarında olduğu gibi değiştirilmiş kimlik bilgilerini GSM ve LSM tablolara yönelik yazma işlemleri için izin vermeniz gerekir.  

Bkz: [elastik veritabanı istemci kitaplığı erişmek için kimlik bilgileri kullanılan](sql-database-elastic-scale-manage-credentials.md).

### <a name="only-metadata-affected"></a>Yalnızca etkilenen meta verileri

Doldurma veya değiştirmek için kullanılan yöntemler **ShardMapManager** veri parça içinde depolanmış kullanıcı verilerini alter değil. Örneğin, yöntemler gibi **CreateShard**, **DeleteShard**, **UpdateMapping**, vs. parça eşlemesi meta verileri yalnızca etkiler. Kaldırma, ekleme veya alter parçalarda bulunan kullanıcı verileri. Bunun yerine, bu yöntemler oluşturmak için gerçekleştirdiğiniz ayrı işlemleri ile birlikte kullanılmak üzere tasarlanmış veya remove gerçek veritabanları veya satır bir parçadan veri parçalı bir ortamı yeniden dengelemek için taşıyın.  ( **Bölme-birleştirme** ile esnek veritabanı araçlarını dahil edilen araç haline getirir, parçalar arasında gerçek veri taşıma işlemlerini yanı sıra bu API'leri kullanın.) Bkz: [elastik veritabanı bölme-birleştirme aracını kullanarak ölçeklendirme](sql-database-elastic-scale-overview-split-and-merge.md).

## <a name="data-dependent-routing"></a>Verilere bağımlı yönlendirme

Parça eşleme Yöneticisi, uygulamaya özgü veri işlemlerini gerçekleştirmek üzere veritabanı bağlantıları gerektiren uygulamalarda kullanılır. Bu bağlantılar doğru veritabanı ile ilişkilendirilmesi gerekir. Bu olarak bilinir **verilere bağımlı yönlendirme**. Bu uygulamalar için GSM veritabanı salt okunur erişimi olan kimlik bilgilerini kullanarak Fabrika parça eşleme Yöneticisi nesneden örneği. Daha sonraki bağlantılar için tek tek istekleri uygun parçaya veritabanına bağlanmak için gereken kimlik bilgilerini sağlayın.

Unutmayın bu uygulamaların (kullanarak **ShardMapManager** salt okunur kimlik bilgileriyle açılır) eşlemeleri ve haritalar için değişiklik yapamazsınız. Bu ihtiyaçları için özel yönetim uygulamaları ya da daha önce bahsedildiği gibi daha yüksek ayrıcalıklı kimlik bilgilerini sağlamanız PowerShell betikleri oluşturun. Bkz: [elastik veritabanı istemci kitaplığı erişmek için kimlik bilgileri kullanılan](sql-database-elastic-scale-manage-credentials.md).

Daha fazla bilgi için [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md).

## <a name="modifying-a-shard-map"></a>Bir parça eşlemesi değiştirme

Bir parça eşlemesi, farklı yollarla değiştirilebilir. Aşağıdaki yöntemlerden birini tüm parçalar ve bunların eşlemelerini açıklayan meta verileri değiştirmek ancak fiziksel olarak veri parçalar içinde değiştirmeyin ya da bunlar oluşturmak veya gerçek veritabanlarını silin.  Bazı işlemler aşağıda açıklanan parça eşlemesi eklemek ve parçalar olarak hizmet veren veritabanları kaldırmak, fiziksel olarak veri taşıma veya yönetim eylemlerini koordine olmanız gerekebilir.

Bu yöntemler, genel dağıtım, parçalı veritabanlarını ortamınızda veri değiştirmek için kullanılabilir yapı taşları olarak birlikte çalışır.  

* Ekleme veya kaldırma parçalar: kullanın **CreateShard** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.shardmap.createshard), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard)) ve **DeleteShard** ([Java](https://docs.microsoft.com/java/api/com.microsoft.azure.elasticdb.shard.map.shardmap.deleteshard), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.deleteshard)) shardmap'ın ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.shardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap)) sınıfı.
  
    Bu işlemleri yürütmek sunucuyu ve veritabanını hedef parça temsil eden varolmalıdır. Bu yöntemler veritabanlarında kendileri yalnızca meta verileri parça eşlemesindeki hiçbir etkisi yoktur.
* Oluşturmak veya noktaları veya parçalara eşlenen aralıklar kaldırmak için: kullanın **CreateRangeMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.createrangemapping), [.NET](https://docs.microsoft.com/previous-versions/azure/dn841993(v=azure.100))), **DeleteMapping** () [Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.deletemapping), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) RangeShardMapping'ın ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) sınıfı ve **CreatePointMapping**  ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.listshardmap.createpointmapping), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.listshardmap-1)) ListShardMap'ın ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.listshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.listshardmap-1)) sınıfı.
  
    Birçok farklı noktaları veya aralıklar aynı parçaya eşlenebilir. Bu yöntem, yalnızca meta veri etkiler - zaten bir parça içinde mevcut olabilecek herhangi bir veri etkilemez. Veri ile tutarlı olması için veritabanından kaldırılacak gerekiyorsa **DeleteMapping** işlemleri ayrı ayrı, ancak bu yöntemleri kullanarak ile birlikte bu işlemleri.  
* Mevcut aralıkları ikiye ayırma veya bitişik aralıktaki birleştirmek için: kullanın **SplitMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.splitmapping), [.NET](https://msdn.microsoft.com/library/azure/dn824205.aspx)) ve **MergeMappings** () [Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.mergemappings), [.NET](https://msdn.microsoft.com/library/azure/dn824201.aspx)).  
  
    Bölme ve birleştirme işlemlerinin Not **anahtar değerlerini eşleştirilir parça değiştirmeyin**. Bir bölme var olan bir aralığı iki bölüme keser, ancak her ikisi de aynı parçaya eşlenmiş olarak bırakır. Bunları tek bir aralığa birleşim aynı parçaya zaten eşlenmiş iki bitişik aralıktaki bir birleştirme çalışır.  Kullanarak Eşgüdümlü noktaları veya aralıklar kendilerini parçalar arasında taşınması gereken **UpdateMapping** gerçek veri taşıma ile birlikte okunmalıdır.  Kullanabileceğiniz **ayırma/birleştirme** hizmet taşıma gerektiğinde veri taşıma ile parça eşleme değişikliklerini koordine etmek için esnek veritabanı araçlarını bir parçası.
* Yeniden eşleme (veya taşımak için) tek tek noktaları veya aralıklar farklı parçalara: kullanın **UpdateMapping** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.updatemapping), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)).  
  
    Bir parçadan veri diğerine tutarlı olması için taşınacak veri gerekebileceği **UpdateMapping** işlemleri ayrı ayrı, ancak bu yöntemleri kullanarak ile birlikte bu taşıma işlemini gerçekleştirmek için ihtiyacınız.

* Eşlemeleri çevrimiçi ve çevrimdışı yapılacak: kullanın **MarkMappingOffline** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.markmappingoffline), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) ve **MarkMappingOnline** ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.markmappingonline), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) bir eşleme çevrimiçi durumunu denetlemek için.
  
    Parça eşlemeleri üzerinde belirli işlemlerin, yalnızca bir eşleme "Çevrimdışı" bir durumda olduğunda izin verilir dahil olmak üzere **UpdateMapping** ve **DeleteMapping**. Bir eşleme çevrimdışı olduğunda, o eşlemeye dahil bir anahtara göre bir veri bağımlı istek bir hata döndürür. Bir aralık önce çevrimdışı duruma getirildiğinde, ayrıca, etkilenen parçaya tüm bağlantıları otomatik olarak değiştirilmesini aralıkları karşı yönlendirilmiş sorgular için tutarsız veya tamamlanmamış sonuçları engellemek için sonlandırıldı.

Eşlemeleri,. NET'te sabit nesnelerdir.  Yukarıdaki eşlemelerini değiştirmek yöntemlerin tümü, ayrıca bunları yönelik tüm başvuruları kodunuzda geçersiz. Operations zincirlenmesine, böylece dizileri bir eşlemesinin durumunu değiştirme işlemleri gerçekleştirmek daha kolay hale getirmek için bir eşleme değiştirme yöntemlerin tümü, yeni bir eşleme başvurusu döndürür. Örneğin, mevcut bir anahtarı 25 içeren shardmap sm'deki eşleme silmek için aşağıdaki yürütebilirsiniz:

```
    sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));
```

## <a name="adding-a-shard"></a>Parça ekleme

Uygulamalar genellikle yeni anahtarlar veya zaten bir parça eşlemesi için anahtar aralıklarına beklenen verileri işlemek için yeni parçalara eklemeniz gerekir. Örneğin, Kiracı Kimliğine göre parçalı bir uygulama için yeni bir kiracı yeni bir parça sağlaması gerekebilir veya veri parçalı aylık sağlanan her yeni ayın başlangıcını önce yeni bir parça gerekebilir.

Yeni anahtar değerleri aralığı zaten varolan bir eşleme parçası değil ve hiç veri taşıma gerekli ise, yeni parça eklemek ve yeni bir anahtar ya da bu parçayı aralığa ilişkilendirmek basit bir işlemdir. Yeni parça ekleme hakkında daha fazla bilgi için bkz [yeni bir parça ekleme](sql-database-elastic-scale-add-a-shard.md).

Veri taşıma gerektiren senaryolar için gerekli parça eşleme güncelleştirmeleri ile birlikte parçalar arasında veri taşıma düzenlemek için bölme-birleştirme aracını ancak gereklidir. Ayırma-birleştirme aracını kullanma hakkında daha fazla bilgi için bkz [bölme-birleştirme genel bakış](sql-database-elastic-scale-overview-split-and-merge.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-shard-map-management/listmapping.png
[2]: ./media/sql-database-elastic-scale-shard-map-management/rangemapping.png
[3]: ./media/sql-database-elastic-scale-shard-map-management/multipleonsingledb.png
