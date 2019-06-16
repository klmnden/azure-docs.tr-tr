---
title: Entity Framework ile esnek veritabanı istemci kitaplığını kullanma | Microsoft Docs
description: Veritabanları kodlama için elastik veritabanı istemci kitaplığı ve Entity Framework kullanma
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
ms.date: 01/04/2019
ms.openlocfilehash: 54890aef8dabfa019a5181c155b6668b1c07cf2c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60331931"
---
# <a name="elastic-database-client-library-with-entity-framework"></a>Entity Framework ile esnek veritabanı istemci kitaplığı

Bu belge, Entity Framework uygulamada tümleştirmek için gereken değişiklikleri gösterir. [esnek veritabanı araçlarını](sql-database-elastic-scale-introduction.md). Buradaki odak noktası olan [parça eşleme Yönetimi](sql-database-elastic-scale-shard-map-management.md) ve [verilere bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) Entity Framework ile **Code First** yaklaşım. [İlk - yeni veritabanı kod](https://msdn.microsoft.com/data/jj193542.aspx) öğretici EF için bu belge boyunca çalışan bir örnek olarak hizmet verir. Bu belge eşlik eden örnek kod, Visual Studio kod örneklerindeki örnekleri kümesi esnek veritabanı araçlarını bir parçası olur.

## <a name="downloading-and-running-the-sample-code"></a>Yükleme ve örnek kodu çalıştırma

Bu makalede kodu indirmek için:

* Visual Studio 2012 veya üzeri gereklidir. 
* İndirme [Azure SQL - Entity Framework tümleştirmesi örneği için esnek veritabanı araçları](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-bae904ba) MSDN'den. Örnek, seçtiğiniz bir konuma ayıklayın.
* Visual Studio’yu çalıştırın. 
* Visual Studio'da dosyayı seçin, proje/çözüm Aç ->. 
* İçinde **Proje Aç** iletişim kutusunda, indirilen örneğe gidin ve seçin **EntityFrameworkCodeFirst.sln** örneği açın. 

Örneği çalıştırmak için Azure SQL veritabanı'nda üç boş veritabanı oluşturma gerekir:

* Parça eşleme Yöneticisi veritabanı
* 1\. parça veritabanı
* 2\. parça veritabanı

Bu veritabanları oluşturduktan sonra yer tutucu doldurun **Program.cs** Azure SQL veritabanı sunucu adınız, veritabanı adları ve veritabanlarına bağlanmak için kimlik bilgilerinizi. Visual Studio çözümü oluşturun. Visual Studio gerekli NuGet paketlerini elastik veritabanı istemci kitaplığı, Entity Framework ve geçici hata işleme yapı işleminin bir parçası olarak indirir. NuGet paketlerini geri yüklemek için çözümünüzün etkinleştirildiğinden emin olun. Visual Studio Çözüm Gezgini'nde çözüme sağ tıklayarak, bu ayarı etkinleştirebilirsiniz. 

## <a name="entity-framework-workflows"></a>Entity Framework iş akışları

Entity Framework geliştiriciler aşağıdaki dört iş akışları uygulamalar oluşturmak ve uygulama nesneleri kalıcılığını sağlamak için birini kullanır:

* **Kod (yeni veritabanı) ilk**: EF Geliştirici uygulama kodunda modeli oluşturur ve ardından EF veritabanında ondan oluşturur. 
* **Kod (var olan veritabanı) ilk**: Geliştirici EF modeli uygulama kodunu varolan bir veritabanından oluşturmak olanak tanır.
* **Model ilk**: Geliştirici model EF Tasarımcısı'nda ve ardından EF veritabanı modeli oluşturur.
* **İlk veritabanı**: Geliştirici, varolan bir veritabanını modelden çıkarsanacak tooling EF kullanır. 

Veritabanı bağlantıları ve veritabanı şeması bir uygulama için şeffaf bir şekilde yönetmek için DbContext sınıfı bu yaklaşımların dayanır. Bağlantı oluşturma, veritabanı önyükleme ve şema oluşturma denetime farklı düzeylerde farklı oluşturucularda DbContext temel sınıf sağlar. Zorlukları öncelikli olarak sağlanan verilere bağımlı Yönlendirme Arabirimleri bağlantı yönetimi özellikleriyle EF tarafından sağlanan veritabanı bağlantı yönetimi kesişip gerçeği elastik veritabanı istemci kitaplığı tarafından durumlardan kaynaklanır. 

## <a name="elastic-database-tools-assumptions"></a>Esnek veritabanı araçları varsayımlar

Terim tanımları için bkz: [esnek veritabanı araçları sözlüğü](sql-database-elastic-scale-glossary.md).

Elastik veritabanı istemci kitaplığı ile uygulama verilerinizi parçacıklara adlı bölümlerini tanımlar. Parçacıklara bir parçalama anahtarı tarafından tanımlanır ve belirli veritabanlarına eşlenir. Bir uygulama gereken sayıda veritabanını ve yeterli kapasite veya geçerli iş gereksinimlerini verilen performans sağlamak üzere parçacıklara dağıtmak olabilirsiniz. Parçalama anahtarı değerleri veritabanlarına eşleme, elastik veritabanı istemci API'leri tarafından sağlanan bir parça eşlemesi tarafından depolanır. Bu özellik adında **parça eşleme Yönetimi**, veya kısaca SMM. Parça eşlemesi, ayrıca bir parçalama anahtarı taşıma istekler için veritabanı bağlantı aracısı olarak işlev görür. Bu özellik olarak da bilinen **verilere bağımlı yönlendirme**. 

Parça eşleme Yöneticisi kullanıcıları tutarsız görünümleri eşzamanlı parçacık yönetimi işlemleri (örneğin, verileri bir parçadan veri diğerine yeniden konumlandırma) oluşmasını gerektiğinde oluşabilecek parçacık verilerini korur. Bunu yapmak için parça eşlemesi veritabanı bağlantıları bir uygulama için İstemci Kitaplığı Aracısı tarafından yönetiliyor. Bu bağlantı için oluşturulan parçacık parça yönetim işlemlerini etkileyebilir, veritabanı bağlantısı otomatik olarak sonlandırmak parça Haritası işlevini sağlar. Bu yaklaşım bazı veritabanı varlığını denetlemek için var olan bir bilgisayardan yeni bağlantıları oluşturma gibi EF'ın işlevselliğini tümleştirmek gerekir. Genel olarak, bizim gözlem standart DbContext oluşturucular için güvenilir bir şekilde EF için güvenli bir şekilde kopyalanabilir kapalı veritabanı bağlantıları yalnızca iş çalıştığını olmuştur. Elastik veritabanı tasarım ilkesini bunun yerine yalnızca açık bağlantılar aracı sağlamaktır. Bir istemci kitaplığı tarafından için EF DbContext vermekten önce aracılı bağlantı kapatma bu sorunu çözebilir düşünebilirsiniz. Ancak, bağlantı kapatma ve yeniden açmak için EF üzerinde bağlı olan bir kitaplık tarafından gerçekleştirilen doğrulama ve tutarlılık denetimleri sağlamlığın gerisinde kalır. EF, geçişler işlevleri, ancak temel alınan veritabanı şemasını uygulamaya saydam bir şekilde yönetmek için bu bağlantıları kullanır. İdeal olarak, korur ve tüm bu özellikler hem elastik veritabanı istemci kitaplığı, hem de EF aynı uygulamada birleştirin. Aşağıdaki bölümde, bu özellikleri ve gereksinimleri bölümünde daha ayrıntılı açıklanır. 

## <a name="requirements"></a>Gereksinimler

Elastik veritabanı istemci kitaplığı ve Entity Framework API'ları ile çalışırken, aşağıdaki özellikleri korumak istediğiniz: 

* **Ölçek genişletme**: Veritabanları kapasite gereksinimlerini karşılamak için gerektiği şekilde parçalı uygulama uygulamanın veri katmanından ekleyip için. Bu, oluşturma ve silme veritabanlarının ve veritabanları ve parçacıklarda, eşlemeleri yönetmek için elastik veritabanı parça eşleme Yöneticisi API'leri kullanarak bir denetime anlamına gelir. 
* **Tutarlılık**: Uygulama parçalama kullanır ve istemci Kitaplığı'nın verilere bağımlı yönlendirme özelliklerini kullanır. Bozulma ya da yanlış sorgu sonuçları önlemek için parça eşleme Yöneticisi aracılı bağlantılar. Bu ayrıca doğrulama ve tutarlılığı korur.
* **Code First**: EF'ın kod ilk paradigma kolaylık korumak için. Code First, uygulamadaki sınıfları saydam bir şekilde temel alınan veritabanı yapılarına yönelik eşlenir. Uygulama kodu, temel alınan veritabanı işleme dahil çoğu yönünü maske DbSets ile etkileşim kurar.
* **Şema**: Entity Framework, ilk veritabanı şema oluşturma ve geçişler aracılığıyla sonraki şema evrimi işler. Veri geliştikçe koruyarak bu özellikler, uygulamanızı uyarlama kolaydır. 

Aşağıdaki yönergeler, esnek veritabanı araçlarını kullanma Code First uygulamalar için bu gereksinimleri karşılamak nasıl bildirir. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Verilere bağımlı yönlendirme EF DbContext kullanma

Entity Framework veritabanı bağlantılarıyla genellikle alt sınıflarından birini yönetilen **DbContext**. Bu alt sınıflarından türetilen oluşturma **DbContext**. Burada tanımlarsınız, **DbSets** uygulamanız için CLR nesnelerin veritabanı tarafından desteklenen koleksiyonları uygulayın. Verilere bağımlı yönlendirme bağlamında, diğer EF kodu ilk uygulama senaryoları için mutlaka tutmayın birkaç faydalı özellikler belirleyebilirsiniz: 

* Veritabanı zaten var ve esnek veritabanı parça eşlemesinde kaydedildi. 
* Uygulama şeması (aşağıda açıklanmıştır) veritabanı için zaten dağıtıldı. 
* Verilere bağımlı yönlendirme veritabanı bağlantılarını parça eşlemesinde aracılı. 

Tümleştirme **DbContexts** ölçek genişletme için verilere bağımlı yönlendirme ile:

1. Parça eşleme Yöneticisi, elastik veritabanı istemci arabirimleri aracılığıyla fiziksel veritabanı bağlantıları oluşturun 
2. Bağlantı ile sarmalamak **DbContext** öğesinin alt sınıfı
3. Bağlantı içine geçirmek **DbContext** temel EF tarafında tüm işleme olacağını da emin olmak için sınıflar. 

Aşağıdaki kod örneği, bu yaklaşımı gösterir. (Ayrıca bu kodu eşlik eden Visual Studio projesinde '.)

```csharp
public class ElasticScaleContext<T> : DbContext
{
public DbSet<Blog> Blogs { get; set; }
...

    // C'tor for data-dependent routing. This call opens a validated connection 
    // routed to the proper shard by the shard map manager. 
    // Note that the base class c'tor call fails for an open connection
    // if migrations need to be done and SQL credentials are used. This is the reason for the 
    // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
    public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
        : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
        true /* contextOwnsConnection */)
    {
    }

    // Only static methods are allowed in calls into base class c'tors.
    private static DbConnection CreateDDRConnection(
    ShardMap shardMap, 
    T shardingKey, 
    string connectionStr)
    {
        // No initialization
        Database.SetInitializer<ElasticScaleContext<T>>(null);

        // Ask shard map to broker a validated connection for the given key
        SqlConnection conn = shardMap.OpenConnectionForKey<T>
                            (shardingKey, connectionStr, ConnectionOptions.Validate);
        return conn;
    }
```

## <a name="main-points"></a>Ana noktaları

* Yeni bir oluşturucu DbContext alt varsayılan oluşturucuyu değiştirir. 
* Yeni oluşturucuyu, elastik veritabanı istemci kitaplığı verilere bağımlı yönlendirme için gerekli olan bir bağımsız değişken bulunur:
  
  * verilere bağımlı yönlendirme arabirimlerini erişmek için parça eşlemesi
  * Parçacık tanımlamak için parçalama anahtarı
  * verilere bağımlı Yönlendirme bağlantı parça için kimlik bilgileri ile bir bağlantı dizesi. 
* Temel sınıf oluşturucu çağrısı verilere bağımlı yönlendirme için gerekli tüm adımları gerçekleştirdiği bir statik yönteme bir sapma alır. 
  
  * Elastik veritabanı istemci arabirimlerini OpenConnectionForKey çağrısı, açık bir bağlantı kurmak için parça eşlemesinde kullanır.
  * Bağlantı için belirtilen parçalama anahtarı parçacık tutan parçaya parça eşlemesi oluşturur.
  * Bu bağlantı DbContext Bu bağlantı tarafından EF EF otomatik olarak yeni bir bağlantı oluşturmanız yapmasına izin vermek yerine kullanılmak üzere olduğunu belirtmek için temel sınıf oluşturucusuna geçirilir. Parça eşleme yönetimi işlemleri altında tutarlılığı garanti edebilir, böylece bu şekilde bağlantı elastik veritabanı istemci API etiketlendi.

Varsayılan Oluşturucu, kodunuzda yerine, DbContext öğesinin alt sınıfı için yeni bir oluşturucu kullanın. Örnek aşağıda verilmiştir: 

```csharp
// Create and save a new blog.

Console.Write("Enter a name for a new blog: "); 
var name = Console.ReadLine(); 

using (var db = new ElasticScaleContext<int>( 
                        sharding.ShardMap,  
                        tenantId1,  
                        connStrBldr.ConnectionString)) 
{ 
    var blog = new Blog { Name = name }; 
    db.Blogs.Add(blog); 
    db.SaveChanges(); 

    // Display all Blogs for tenant 1 
    var query = from b in db.Blogs 
                orderby b.Name 
                select b; 
    … 
}
```

Yeni oluşturucuyu değeri tarafından tanımlanan parçacık verileri tutan parça bağlantı açar **tenantid1**. Kodda **kullanarak** blok erişimi değişmeden kalır **olan DB** EF için parça kullanarak bloglar için **tenantid1**. Kullanarak kod engellemek için gibi tüm veritabanı işlemleri artık bir parçaya kapsamındaki bu semantiğini değiştirir. burada **tenantid1** tutulur. Örneği için bir LINQ sorgusu blogları üzerinden **olan DB** yalnızca geçerli parça üzerinde depolanan blogları, ancak diğer parçalara üzerinde depolanan olanları döndürür.  

#### <a name="transient-faults-handling"></a>Geçici hataları işleme

Microsoft Patterns ve uygulamalar ekibi yayımlanan [geçici hata işleme uygulama bloğu](https://msdn.microsoft.com/library/dn440719.aspx). Kitaplığı ile esnek ölçek istemcisi kitaplığını EF ile birlikte kullanılır. Ancak, geçici bir özel durumla noktadır. Burada, geçici bir hata sonra herhangi bir yeni bağlantı denemesi, tweaked oluşturucu kullanılarak oluşturulur ve böylece yeni oluşturucuyu kullanıldığını sağlayabilirsiniz döndürdüğünden emin olun. Aksi takdirde doğru parça bağlantı garanti edilmez ve parça eşlemesine değişiklikler oldukça bağlantının korunacağı süreyi hiçbir Güvenceleri vardır. 

Aşağıdaki kod örneği nasıl bir SQL yeniden deneme ilkesi yeni geçici olarak kullanılabileceğini gösterir **DbContext** alt sınıf oluşturucuları: 

```csharp
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{ 
    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
        { 
                var blog = new Blog { Name = name }; 
                db.Blogs.Add(blog); 
                db.SaveChanges(); 
        … 
        } 
    }); 
```

**SqlDatabaseUtils.SqlRetryPolicy** Yukarıdaki kod olarak tanımlanan bir **SqlDatabaseTransientErrorDetectionStrategy** 10 ve 5 saniye ile bir yeniden deneme sayısı, yeniden denemeler arasındaki süre bekleyin. Bu yaklaşım EF ve kullanıcı tarafından başlatılan işlemler için yönergeler benzer (bkz [sınırlamalar (EF6 sonrası) yeniden deneme yürütme stratejileri ile](https://msdn.microsoft.com/data/dn307226). Her iki durumda uygulama programı geçici özel durum döndüren kapsamı denetimleri gerektirir: esnek veritabanı istemci kitaplığı kullanan işlem yeniden veya uygun Oluşturucusu bağlamdan (görüldüğü gibi) yeniden oluşturun.

Burada geçici özel durumlar bize kapsam içinde olması denetleme ihtiyacı yerleşik kullanımını da ışığının **SqlAzureExecutionStrategy** EF ile birlikte gelir. **SqlAzureExecutionStrategy** ancak bir bağlantıyı yeniden kullanma **OpenConnectionForKey** ve bu nedenle bir parçası olarak gerçekleştirilen tüm doğrulama atlama **OpenConnectionForKey**çağırın. Bunun yerine, kod örneği, yerleşik kullanır **DefaultExecutionStrategy** EF ile de sunulur. Başlangıcı yerine sonundan **SqlAzureExecutionStrategy**, düzgün şekilde geçici hata işleme yeniden deneme ilkesi ile birlikte çalışır. Yürütme ilkesini ayarlama **ElasticScaleDbConfiguration** sınıfı. Unutmayın, biz kullanmamaya karar **DefaultSqlExecutionStrategy** kullanılması öneriliyor beri **SqlAzureExecutionStrategy** geçici özel durumlar oluşursa - hangi neden yanlış davranışa açıklandığı gibi. EF ve farklı yeniden deneme ilkeleri hakkında daha fazla bilgi için bkz. [bağlantı dayanıklılığı EF içinde](https://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Oluşturucu taşıyabilmenizi sağlar

Yukarıdaki kod örnekleri, Entity Framework ile verilere bağımlı yönlendirme kullanmak için uygulamanız için gereken varsayılan oluşturucu yeniden yazar göstermektedir. Aşağıdaki tabloda, bu yaklaşım diğer oluşturucular genelleştirir. 

| Geçerli Oluşturucusu | Veriler için yeniden Oluşturucusu | Temel oluşturucu | Notlar |
| --- | --- | --- | --- |
| MyContext() |ElasticScaleContext(ShardMap, TKey) |DbContext (DbConnection, Boole) |Verilere bağımlı yönlendirme anahtarı ve parça eşlemesi bir işlev bağlantısı gerekir. Bunun yerine Bağlantı Aracısı için aralık parça eşlemesi kullanma ve EF intranetlerinden otomatik bağlantı oluşturması gerekir. |
| MyContext(string) |ElasticScaleContext(ShardMap, TKey) |DbContext (DbConnection, Boole) |Bağlantı, parça eşlemesi ve verilere bağımlı yönlendirme anahtarı için kullanılan bir işlevdir. Bir sabit veritabanı adı veya bağlantı dizesi olarak çalışmıyor parça eşlemesi tarafından intranetlerinden doğrulama. |
| MyContext(DbCompiledModel) |ElasticScaleContext(ShardMap, TKey, DbCompiledModel) |DbContext(DbConnection, DbCompiledModel, bool) |Bağlantı verilen parça Haritası ve parçalama anahtarı için sağlanan modeli kullanılarak oluşturulmuş. Derlenmiş modeli için temel c'tor aktarılır. |
| MyContext (DbConnection, Boole) |ElasticScaleContext(ShardMap, TKey, bool) |DbContext (DbConnection, Boole) |Parça eşleme ve anahtar çıkarılan bağlantı gerekir. (Bu girişi zaten parça eşlemesi ve anahtarı kullanarak olduğu sürece) bir giriş olarak sağlanamaz. Boolean aktarılır. |
| MyContext (dize, DbCompiledModel) |ElasticScaleContext(ShardMap, TKey, DbCompiledModel) |DbContext(DbConnection, DbCompiledModel, bool) |Parça eşleme ve anahtar çıkarılan bağlantı gerekir. (Giriş parça eşlemesi ve anahtarı kullanarak olduğu sürece) bir giriş olarak sağlanamaz. Derlenmiş modeline aktarılır. |
| MyContext (ObjectContext, Boole) |ElasticScaleContext(ShardMap, TKey, ObjectContext, bool) |DbContext (ObjectContext, Boole) |Girdi olarak geçirilen objectcontext'te herhangi bir bağlantı için esnek ölçeklendirme tarafından yönetilen bir bağlantı yeniden yönlendirilmiş olduğundan emin olmak yeni Oluşturucu gerekir. Bu belgenin kapsamı dışındadır ObjectContexts hakkında ayrıntılı bilgi var. |
| MyContext (DbConnection, DbCompiledModel, Boole) |ElasticScaleContext(ShardMap, TKey, DbCompiledModel, bool) |DbContext (DbConnection, DbCompiledModel, Boole); |Parça eşleme ve anahtar çıkarılan bağlantı gerekir. (Bu girişi zaten parça eşlemesi ve anahtarı kullanarak olduğu sürece) bağlantı girdi olarak sağlanamaz. Model ve Boolean temel sınıf oluşturucuya geçirilir. |

## <a name="shard-schema-deployment-through-ef-migrations"></a>EF geçişleri üzerinden parça şema dağıtımı

Otomatik şema yönetimi, Entity Framework tarafından sağlanan bir kolaylığıdır. Esnek veritabanı araçlarını kullanarak uygulamalar bağlamında veritabanlarını parçalı uygulamaya eklendiğinde otomatik olarak yeni oluşturulan parçalara şema sağlamak için bu özelliği korumak ister. EF kullanan parçalı uygulamalar için veri katmanında kapasitesini artırmak için birincil olarak kullanıldığı durumdur. Şema Yönetimi için EF'ın özellikleri güvenmek EF üzerinde oluşturulmuş parçalı bir uygulama ile veritabanı yönetim çaba azaltır. 

EF geçişleri üzerinden şema dağıtımı en iyi şekilde çalışır **açılmamış bağlantıları**. Elastik veritabanı istemci API'si tarafından sağlanan açılan bağlantı dayanan verilere bağımlı yönlendirme senaryosu aksine budur. Başka bir farktır tutarlılık gereksinimi: Eş zamanlı parça eşlemesi işleme karşı korumak tüm verilere bağımlı yönlendirme bağlantıları için tutarlılık sağlamak için daha fazla tercih parça eşlemesinde kayıtlı değil henüz ve henüz sahip yeni bir veritabanına ilk şemasını dağıtım ile bir sorun değildir parçacıklara tutmak için ayrılmış. Bu nedenle verilere bağımlı yönlendirme bu senaryo için normal veritabanı bağlantıları güvenebilirsiniz.  

Bu, bir yaklaşım burada şema dağıtımı EF geçişleri üzerinden yeni bir veritabanı kaydını ile uygulamanın parça eşlemesindeki bir parça olarak sıkıca yol açar. Bu, aşağıdaki önkoşullar kullanır: 

* Veritabanı zaten oluşturuldu. 
* Veritabanı boş - kullanıcı bir şeması yok ve hiçbir kullanıcı verilerini tutar.
* Veritabanı henüz elastik veritabanı istemci API'leri verilere bağımlı yönlendirme için erişilemez durumda. 

Bu önkoşulları yerine getirilince, bir normal oluşturabilirsiniz açılmamış **SqlConnection** için şema dağıtımı için EF geçişleri hız kazandırın. Aşağıdaki kod örneği, bu yaklaşımı gösterir. 

```csharp
// Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
// and kick off EF initialization of the database to deploy schema 

public void RegisterNewShard(string server, string database, string connStr, int key) 
{ 

    Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

    SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
    connStrBldr.DataSource = server; 
    connStrBldr.InitialCatalog = database; 

    // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
    // This requires an un-opened connection. 
    using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
    { 
        // Run a query to engage EF migrations 
        (from b in db.Blogs 
            select b).Count(); 
    } 

    // Register the mapping of the tenant to the shard in the shard map. 
    // After this step, data-dependent routing on the shard map can be used 

    this.ShardMap.CreatePointMapping(key, shard); 
} 
```

Bu örnek, bir yöntemi gösterir **RegisterNewShard** , parça parça eşlemesinde kaydeder, şema EF geçişleri aracılığıyla dağıtır ve parça için bir parçalama anahtarı bir eşleme depolar. Bir oluşturucu üzerinde dayanır **DbContext** alt (**ElasticScaleContext** örnekteki), bir SQL bağlantı dizesi girdi olarak alır. Bu oluşturucu aşağıdaki örnekte gösterildiği gibi rahatça, koddur: 

```csharp
// C'tor to deploy schema and migrations to a new shard 
protected internal ElasticScaleContext(string connectionString) 
    : base(SetInitializerForConnection(connectionString)) 
{ 
} 

// Only static methods are allowed in calls into base class c'tors 
private static string SetInitializerForConnection(string connectionString) 
{ 
    // You want existence checks so that the schema can get deployed 
    Database.SetInitializer<ElasticScaleContext<T>>( 
new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

    return connectionString; 
} 
```

Bir temel sınıftan devralınan Oluşturucusu sürümünü kullanmış olabilirsiniz. Ancak EF için varsayılan Başlatıcı bağlanırken kullanıldığından emin olmak kodu gerekiyor. Bu nedenle kısa sapma içine bağlantı dizesiyle temel sınıf oluşturucusunu çağırma önce statik yöntem. Parçalar kaydını farklı uygulama etki alanı veya EF Başlatıcı ayarlarını çakışmasını emin olmak için işlemini çalışması gerektiğini unutmayın. 

## <a name="limitations"></a>Sınırlamalar

Bu belgede özetlenen yaklaşımları birkaç sınırlama dahildir: 

* EF kullanan uygulamalar **LocalDb** ilk normal bir SQL Server veritabanı için elastik veritabanı istemci kitaplığı kullanmadan önce geçirmeniz gerekiyor. Parçalama üzerinden bir uygulama ile esnek ölçek genişletme mümkün değil **LocalDb**. Geliştirme hala kullanabileceğinizi unutmayın **LocalDb**. 
* EF geçişleri tüm parçalar üzerinde Git veritabanı şema değişikliklerinin yaptığından değişiklikleri uygulamaya gerekir. Bu belge için örnek kod, bunun nasıl yapılacağı göstermemiz gerekmez. Veritabanını Güncelleştir tüm parçalar yinelemek için bir bağlantı dizesi parametreyle kullanmayı düşünün; ya da bir veritabanını güncelleştir kullanarak bekleyen geçiş için T-SQL betiği Ayıkla komut dosyası seçeneği ve T-SQL betiği, parçaları için geçerlidir.  
* Bir istek alındığında, tüm veritabanı işlemesi yer alan, tek bir parçanın içinde istek tarafından sağlanan parçalama anahtarı tarafından tanımlandığı gibi varsayılır. Ancak, bu varsayımı her zaman true tutmaz. Örneğin, ne zaman, bir parçalama anahtarı kullanılabilir hale getirmek mümkün değildir. Bunu ele almak için istemci kitaplığı sağlar **MultiShardQuery** birden çok parça sorgulama için bir bağlantı Özet uygulayan sınıf. Kullanmayı öğrenme **MultiShardQuery** EF ile birlikte bu belgenin kapsamı dışında olan

## <a name="conclusion"></a>Sonuç

Bu belgede özetlenen adımları oluşturucular Düzenleyicisi tarafından verilere bağımlı yönlendirme için EF uygulamaların elastik veritabanı istemci Kitaplığı'nın özellik kullanmaya **DbContext** EF uygulamada kullanılan alt sınıflar. Bu sınırlar için bu yerlerin gerekli değişiklikler burada **DbContext** sınıflar zaten mevcut. Ayrıca, EF uygulamaları otomatik şema dağıtımından kaydı ile gerekli EF geçişleri parça eşlemesindeki eşlemeleri ve yeni parçalara çağırma adımları birleştirerek yararlanmak devam edebilir. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
