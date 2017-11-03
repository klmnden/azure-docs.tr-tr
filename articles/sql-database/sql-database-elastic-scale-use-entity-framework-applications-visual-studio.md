---
title: "Entity Framework ile esnek veritabanı istemci kitaplığı kullanılarak | Microsoft Docs"
description: "Esnek veritabanı istemci kitaplığı ve Entity Framework veritabanları kodlama için kullanın"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: b9c3065b-cb92-41be-aa7f-deba23e7e159
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: torsteng
ms.openlocfilehash: 1fc61657419f1f4581c5c67639d7bc2e4b0d509f
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="elastic-database-client-library-with-entity-framework"></a>Entity Framework ile esnek veritabanı istemci kitaplığı
Bu belge, Entity Framework uygulamada bütünleştirmek için gereken değişiklikleri gösterir. [esnek veritabanı araçlarını](sql-database-elastic-scale-introduction.md). Oluşturma üzerinde odak noktasıdır [parça eşleme Yönetim](sql-database-elastic-scale-shard-map-management.md) ve [veri bağımlı yönlendirme](sql-database-elastic-scale-data-dependent-routing.md) Entity Framework **Code First** yaklaşım. [İlk - yeni veritabanı kod](http://msdn.microsoft.com/data/jj193542.aspx) öğretici EF için bu belge boyunca çalışan bir örnek olarak hizmet verir. Bu belge ile birlikte gelen örnek kod, esnek veritabanı araçlarını Visual Studio kod örnekleri örnekleri kümesi parçasıdır.

## <a name="downloading-and-running-the-sample-code"></a>Yükleme ve örnek kodu çalıştırma
Bu makale için kod karşıdan yüklemek için:

* Visual Studio 2012 veya üzeri gereklidir. 
* Karşıdan [Azure SQL - Entity Framework tümleştirme örneği için esnek DB Araçları](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-bae904ba) MSDN'den. Örnek seçtiğiniz bir konuma ayıklayın.
* Visual Studio’yu çalıştırın. 
* Visual Studio'da Aç proje/çözüm dosyasını seçin ->. 
* İçinde **Proje Aç** iletişim kutusunda, indirdiğiniz örnek gidin ve seçin **EntityFrameworkCodeFirst.sln** örneği açın. 

Örneği çalıştırmak için Azure SQL veritabanı'nda üç boş veritabanları oluşturmanız gerekir:

* Parça eşleme Manager veritabanı
* Parça 1 veritabanı
* Parça 2 veritabanı

Bu veritabanları oluşturduktan sonra yer tutucu doldurun **Program.cs** Azure SQL veritabanı sunucunuzun adını, veritabanı adları ve veritabanlarına bağlanmak için kimlik bilgileriniz ile. Visual Studio çözümü oluşturun. Visual Studio gerekli NuGet paketlerini esnek veritabanı istemci kitaplığı için Entity Framework ve geçici oluşturma işleminin bir parçası olarak işleme hata indirir. NuGet paketleri geri çözümünüz için etkinleştirildiğinden emin olun. Visual Studio Çözüm Gezgini'nde çözüme sağ tıklayarak bu ayarı etkinleştirebilirsiniz. 

## <a name="entity-framework-workflows"></a>Entity Framework iş akışları
Entity Framework geliştiriciler aşağıdaki dört iş uygulamaları geliştirmek ve uygulama nesneleri kalıcılığını sağlamak için birini kullanır: 

* **(Yeni veritabanı) ilk kod**: EF Geliştirici uygulama kodunda modeli oluşturur ve ardından EF veritabanı ondan oluşturur. 
* **İlk (var olan veritabanı) kod**: Geliştirici model uygulama kodunu oluşturmak varolan bir veritabanından EF olanak sağlar.
* **Model ilk**: Geliştirici model EF Designer'da ve ardından EF modelden veritabanı oluşturur.
* **Veritabanı ilk**: varolan bir veritabanını modelden gerçekleştirip tooling EF Geliştirici kullanır. 

Bu yaklaşım veritabanı bağlantılarını ve veritabanı şeması bir uygulama için şeffaf bir şekilde yönetmek için DbContext sınıfını kullanır. DbContext temel sınıfından farklı oluşturucular farklı düzeylerde bağlantı oluşturma, veritabanı önyükleme ve şema oluşturma denetime izin verir. Öncelikle veri bağımlı yönlendirme arabirimlerini bağlantı yönetimi özellikleriyle EF tarafından sağlanan veritabanı bağlantı yönetimi kestiği olgu zorluklar esnek veritabanı istemci kitaplığı tarafından kaynaklanır. 

## <a name="elastic-database-tools-assumptions"></a>Varsayımlar esnek veritabanı araçları
Terim tanımları için bkz: [esnek veritabanı araçlarını sözlüğü](sql-database-elastic-scale-glossary.md).

Esnek veritabanı istemci kitaplığı ile uygulama verilerinizi shardlets adlı Bölüm tanımlayın. Shardlets bir parçalama anahtar tarafından tanımlanır ve belirli veritabanlarına eşleştirilir. Bir uygulama gerekli tüm veritabanı ve yeterli kapasitesi veya geçerli iş gereksinimlerini verilen performans sağlamak için shardlets dağıtmak olabilirsiniz. Parçalama anahtar değerlerin veritabanlarına esnek veritabanı istemci API tarafından sağlanan bir parça eşleme tarafından depolanır. Bu özellik adında **parça eşleme Yönetim**, veya kısaca SMM. Parça eşleme, ayrıca bir parçalama anahtar taşımak istekleri için veritabanı bağlantılarını aracısı olarak görev yapar. Bu özellik olarak bilinen **veri bağımlı yönlendirme**. 

Parça eşleme Yöneticisi kullanıcıları tutarsız görünümleri eşzamanlı shardlet management işlemleri (örneğin, verileri bir parça diğerine yerini değiştirme) gerçekleştiği yüklendiğinde oluşabilecek shardlet verilerini korur. Bunu yapmak için parça eşlemeleri veritabanı bağlantılarını bir uygulama için İstemci Kitaplığı Aracısı tarafından yönetiliyor. Bu bağlantı için oluşturulan shardlet parça yönetim işlemlerini etkileyebilir olduğunda otomatik olarak bir veritabanı bağlantısı sonlandırılamadı parça eşleme işlevselliği sağlar. Mevcut bir veritabanı varlığını denetlemek için yeni bağlantı oluşturma gibi EF'ın işlevlerini bazıları ile tümleştirmek bu yaklaşım gerekir. Genel olarak, bizim gözlem güvenilir bir şekilde EF için güvenli bir şekilde kopyalanıp kapalı veritabanı bağlantıları için yalnızca iş standart DbContext oluşturucular çalıştığını olmuştur. Esnek veritabanı tasarım ilkesini bunun yerine yalnızca açık bağlantıları Aracısı sağlamaktır. Bir istemci kitaplığı tarafından EF DbContext vermekten önce aracılı bağlantı kesiliyor bu sorunu çözebilir düşünebilirsiniz. Ancak, bağlantı kesiliyor ve yeniden EF bağlı olan bir kitaplık tarafından gerçekleştirilen doğrulama ve tutarlılık denetimleri sağlamlığın gerisinde kalır. EF, geçişler işlevi, bu bağlantıları ancak, temel alınan veritabanı şeması uygulamaya saydam bir şekilde yönetmek için kullanır. İdeal olarak, korur ve tüm bu özellikler esnek veritabanı istemci kitaplığı ve EF aynı uygulamada birleştirin. Aşağıdaki bölümde, bu özellikleri ve gereksinimleri daha ayrıntılı açıklanır. 

## <a name="requirements"></a>Gereksinimler
Esnek veritabanı istemci kitaplığı ve Entity Framework API'leri ile çalışırken, aşağıdaki özellikleri korumak istediğiniz: 

* **Genişleme**: eklemek veya uygulamanın parçalı uygulama kapasite gereksinimlerini karşılamak için gerekli olarak veri katmanından veritabanlarını kaldırmak için. Bu denetim oluşturma ve veritabanlarını ve veritabanları ve shardlets eşlemelerini yönetmek için esnek veritabanı parça eşleme manager API'lerini kullanarak silinmesini üzerinden anlamına gelir. 
* **Tutarlılık**: uygulama parçalama kullanır ve istemci kitaplığı veri bağımlı yönlendirme yeteneklerini kullanır. Bozulma veya yanlış sorgu sonuçları önlemek için bağlantılar parça eşleme Yöneticisi aracılığıyla aracılı. Bu ayrıca doğrulama ve tutarlılık korur.
* **İlk kod**: EF'ın kod ilk kip kolaylık korumak için. Code First içinde uygulama sınıflarda saydam temel alınan veritabanı yapılarını eşlenir. Uygulama kodu birçok yönüyle temel alınan veritabanı işleme, söz konusu maske DbSets ile etkileşim kurar.
* **Şema**: Entity Framework ilk veritabanı şeması oluşturma ve sonraki şema evrimi geçişleri üzerinden işler. Veri geliştikçe yeteneklere koruyarak uygulamanızı uyarlama kolaydır. 

Aşağıdaki kılavuz esnek veritabanı araçlarını kullanarak Code First uygulamalar için bu gereksinimleri karşılamak nasıl bildirir. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Veri bağımlı yönlendirme EF DbContext kullanma
Entity Framework veritabanı bağlantılarıyla genellikle alt sınıflarının yönetilen **DbContext**. Bu alt sınıflarından türetme tarafından oluşturma **DbContext**. Burada, tanımlarsınız, **DbSets** CLR nesnesi, uygulamanız için veritabanı yedeği koleksiyonları uygulayın. Veri bağımlı yönlendirme bağlamında, diğer EF kodu ilk uygulama senaryoları için mutlaka tutmayın çeşitli yararlı özelliklerini tanımlayabilirsiniz: 

* Veritabanı zaten var ve esnek veritabanı parça eşlemesinde kaydedildi. 
* Uygulama şeması (aşağıda açıklanmıştır) veritabanı için zaten dağıtılmış. 
* Veri bağımlı yönlendirme bağlantıları veritabanı tarafından parça eşleme aracılı. 

Tümleştirmek için **DbContexts** veri bağımlı genişleme için Yönlendirme:

1. Parça eşleme Yöneticisi'nin esnek veritabanı istemci arabirimleri aracılığıyla fiziksel veritabanı bağlantıları oluşturma, 
2. Bağlantı ile kaydırma **DbContext** alt sınıfı
3. İçine bağlantı geçirmek **DbContext** temel sınıflar EF tarafında tüm işleme olacağını da emin olun. 

Aşağıdaki kod örneği, bu yaklaşım gösterilmektedir. (Ayrıca bu kodu eşlik eden Visual Studio projesinde '.)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

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

## <a name="main-points"></a>Ana noktaları
* Varsayılan Oluşturucu DbContext alt yeni oluşturucuyu değiştirir 
* Esnek veritabanı istemci kitaplığı veri bağımlı yönlendirme için gerekli olan bağımsız değişkenler yeni oluşturucuyu alır:
  
  * veri bağımlı yönlendirme arabirimlerini erişmek için parça eşleme
  * shardlet tanımlamak için parçalama anahtarı
  * Parça veri bağımlı yönlendirme bağlantısı için kimlik bilgileri ile bir bağlantı dizesi. 
* Taban sınıf oluşturucu çağrısı veri bağımlı yönlendirme için gerekli tüm adımları gerçekleştirir statik bir yönteme bir detour alır. 
  
  * Esnek veritabanı istemci arabirimlerin OpenConnectionForKey çağrısı, açık bir bağlantı kurmak için parça haritada kullanır.
  * Parça eşlemesi belirtilen parçalama anahtar shardlet tutan parça açık bağlantısı oluşturur.
  * Bu açık bağlantı geri bu bağlantının otomatik olarak yeni bir bağlantı oluşturmak EF yapmasına izin vermek yerine EF tarafından kullanılacak olduğunu belirtmek için DbContext temel sınıf oluşturucusunun geçirilir. Böylece parça eşleme yönetim işlemleri altında tutarlılığı garanti edebilir bu şekilde bağlantı esnek veritabanı istemci API tarafından etiketlendiği.

Varsayılan Oluşturucu, kodunuzda yerine, bir DbContext alt için yeni bir oluşturucu kullanın. Örnek aşağıda verilmiştir: 

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

Yeni Oluşturucusu değeri tarafından tanımlanan shardlet verilerini tutan parça bağlantı açar **tenantid1**. Kodda **kullanarak** blok erişimi değişmeden kalır **DbSet** için parça EF kullanarak Web günlükleri için **tenantid1**. Kullanarak kod bloğu için gibi tüm veritabanı işlemleri için bir parça şimdi kapsamındaki bu semantiğini değiştirir nerede **tenantid1** tutulur. Örneğin, bir LINQ sorgusu bloglar üzerinden **DbSet** yalnızca üzerinde geçerli parça depolanan bloglar, ancak diğer parça üzerinde depolanan olanları döndürür.  

#### <a name="transient-faults-handling"></a>Geçici hataları işleme
Microsoft Patterns & yöntemler takım yayımlanan [geçici hata işleme uygulama blok](https://msdn.microsoft.com/library/dn440719.aspx). Kitaplık esnek ölçek istemci kitaplığı EF ile birlikte kullanılır. Ancak, geçici bir özel durumla bir yere, böylece herhangi bir yeni bağlantı denemesi, tweaked oluşturucu kullanılarak yapılan yeni Oluşturucusu sonra geçici bir hata kullanılıyor burada sağlayabilirsiniz döndürdüğünden emin olun. Aksi takdirde doğru parça bağlantı garanti edilmez ve parça eşleme değişiklikler oldukça bağlantının korunacağı hiçbir garanti vermediğini vardır. 

Aşağıdaki kod örneği nasıl bir SQL yeniden deneme ilkesi yeni kullanılabileceğini gösterir **DbContext** alt sınıf oluşturucular: 

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

**SqlDatabaseUtils.SqlRetryPolicy** Yukarıdaki kod olarak tanımlanan bir **SqlDatabaseTransientErrorDetectionStrategy** bir yeniden deneme sayısı 10 ve 5 saniye ile yeniden denemeler arasındaki süre bekleyin. Bu yaklaşım EF ve kullanıcı tarafından başlatılan işlemleri için yönergeler benzer (bkz [yeniden deneniyor yürütme stratejileri (veya sonraki sürümleri EF6) kısıtlamalarla](http://msdn.microsoft.com/data/dn307226). Her iki durumlarda uygulama programı geçici özel durumu döndüren kapsam denetimleri gerektirir: esnek veritabanı istemci kitaplığı kullanan işlem yeniden veya (gösterildiği gibi) uygun Oluşturucusu bağlamından yeniden oluşturun.

Burada geçici özel durumlar bize geri kapsamında olması denetlemek için gereken ayrıca yerleşik kullanımını önleyen **SqlAzureExecutionStrategy** EF ile birlikte gelir. **SqlAzureExecutionStrategy** bir bağlantıyı yeniden, ancak kullanmaz **OpenConnectionForKey** ve bu nedenle bir parçası olarak gerçekleştirilen tüm doğrulama atlama **OpenConnectionForKey**çağırın. Bunun yerine, yerleşik kod örneğini kullanan **DefaultExecutionStrategy** EF ile de gelir. Tersine **SqlAzureExecutionStrategy**, doğru geçici hata işleme yeniden deneme ilkesi ile birlikte çalışır. Yürütme ilkesini ayarlama **ElasticScaleDbConfiguration** sınıfı. Kullanmamaya karar Not **DefaultSqlExecutionStrategy** kullanılmasını önerir beri **SqlAzureExecutionStrategy** geçici özel durumlar oluşursa - hangi neden yanlış davranışa açıklandığı gibi. EF ve farklı yeniden deneme ilkeleri hakkında daha fazla bilgi için bkz: [EF bağlantı dayanıklılığı](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Oluşturucu yeniden yazmalar
Yukarıdaki kod örnekleri, Entity Framework ile veri bağımlı yönlendirmeyi kullanmak için uygulamanız için gerekli varsayılan oluşturucusu yeniden yazar gösterilmektedir. Aşağıdaki tabloda bu yaklaşımı diğer oluşturucular genelleştirir. 

| Geçerli Oluşturucusu | Verileri yeniden Oluşturucusu | Temel Oluşturucusu | Notlar |
| --- | --- | --- | --- |
| MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |Bağlantı bir işlev parça eşleme ve veri bağımlı yönlendirme anahtarı olması gerekir. Parça eşleme Bağlantı Aracısı kullanın ve atlama otomatik bağlantı oluşturma EF işlemi yapmanız gerekir. |
| MyContext(string) |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection, bool) |Bağlantı parça eşleme ve veri bağımlı yönlendirme anahtarı bir işlevdir. Sabit veritabanı adı veya bağlantı dizesi bunlar çalışmıyor parça eşleme tarafından atlama doğrulama. |
| MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, Boole) |Bağlantı verilen parça eşleme ve parçalama anahtarı için sağlanan modeliyle oluşturulan. Derlenmiş modeli temel c'tor aktarılır. |
| MyContext (DbConnection, bool) |ElasticScaleContext (ShardMap, TKey, Boole) |DbContext (DbConnection, bool) |Bağlantı parça eşleme ve anahtar olayla gerekiyor. (Bu girişi zaten parça eşleme ve anahtarı kullanmadığı sürece) bir giriş olarak sağlanamaz. Boolean aktarılır. |
| MyContext (dize, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, Boole) |Bağlantı parça eşleme ve anahtar olayla gerekiyor. (Bu giriş parça eşleme ve anahtarı kullanmadığı sürece) bir giriş olarak sağlanamaz. Derlenmiş modeli aktarılır. |
| MyContext (ObjectContext, bool) |ElasticScaleContext (ShardMap, TKey, ObjectContext, bool) |DbContext (ObjectContext, bool) |Herhangi bir giriş olarak geçirilen ObjectContext bağlantısında esnek ölçek tarafından yönetilen bir bağlantıya yeniden yönlendirilmiş olduğundan emin olmak yeni Oluşturucusu gerekir. ObjectContexts hakkında ayrıntılı bilgi, bu belgenin kapsamında değildir. |
| MyContext (DbConnection, DbCompiledModel, Boole) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel, bool) |DbContext (DbConnection, DbCompiledModel, bool); |Bağlantı parça eşleme ve anahtar olayla gerekiyor. (Bu girişi zaten parça eşleme ve anahtarı kullanmadığı sürece) bir giriş olarak bağlantı sağlanamaz. Model ve Boolean temel sınıf oluşturucu geçirilir. |

## <a name="shard-schema-deployment-through-ef-migrations"></a>EF geçişler aracılığıyla parça şema dağıtımı
Otomatik şema, Entity Framework tarafından sağlanan bir kolaylık yönetimidir. Yeni oluşturulan parça şemaya veritabanları parçalı uygulamaya eklendiğinde otomatik olarak sağlamak üzere bu özellikten korumak istediğiniz esnek veritabanı araçlarını kullanarak uygulamaları bağlamında. EF kullanan parçalı uygulamalar için veri katmanı kapasitesini artırmak için birincil kullanım durumdur. Şema Yönetimi için EF'in özellikleri güvenmek EF üzerinde oluşturulmuş parçalı bir uygulama ile veritabanı yönetim çaba azaltır. 

Şema dağıtımı EF geçişler aracılığıyla en iyi şekilde çalışır **açılmamış bağlantıları**. Esnek veritabanı istemci API tarafından sağlanan bağlantısı açıldı dayanan veri bağımlı yönlendirme senaryosu aksine budur. Başka bir fark tutarlılık gereksinimdir: eşzamanlı parça eşleme işleme karşı korumak tüm veri bağımlı yönlendirme bağlantılarında tutarlılığını sağlamak için daha fazla tercih sırasında bu ilk ile ilgili bir sorun değildir şema dağıtım yeni bir veritabanı Parça eşlemesinde kayıtlı değil henüz ve shardlets tutmak için ayrılmamış henüz vardır. Bu nedenle veri bağımlı yönlendirme bu senaryo için normal veritabanı bağlantılarını güvenebilirsiniz.  

Bu, bir yaklaşım nerede EF geçişler aracılığıyla şema dağıtımın sıkı şekilde yeni bir veritabanı kaydını ile uygulamanın parça eşlemesindeki bir parça olarak bağlı yol açar. Bu, aşağıdaki önkoşulların üzerinde dayanır: 

* Veritabanı zaten oluşturuldu. 
* Veritabanı boş - kullanıcı şeması yok ve hiçbir kullanıcı verileri tutar.
* Veritabanı, esnek veritabanı istemci API'leri veri bağımlı yönlendirme için henüz erişilemiyor. 

Bu önkoşulları yerine getirilince, bir normal oluşturabilirsiniz açılmamış **SqlConnection** EF geçişler şema dağıtımı için devre dışı tetiklersiniz için. Aşağıdaki kod örneği, bu yaklaşım gösterilmektedir. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

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


Bu örnek yöntemi gösterilir **RegisterNewShard** parça parça eşlemesinde kaydeder, şemanın EF geçişler aracılığıyla dağıtır ve parça parçalama anahtarına eşlemesi depolar. Bir oluşturucusuna dayanır **DbContext** bir alt kümesi (**ElasticScaleContext** örnekteki), bir SQL bağlantı dizesi giriş olarak alır. Bu oluşturucu doğrudan İleri aşağıdaki örnekte gösterildiği gibi koddur: 

        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // You want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 

Bir taban sınıftan devralınan Oluşturucusu sürümünü kullanmış olabilirsiniz. Ancak EF varsayılan Başlatıcı bağlanırken kullanıldığından emin olmak kod gerekiyor. Bu nedenle kısa saptıran statik yöntemiyle bağlantı dizesiyle temel sınıf oluşturucu içine çağırmadan önce. Parça kaydını farklı uygulama etki alanı ya da EF Başlatıcı ayarlarını çakışmasını emin olmak için işlem çalışması gerektiğini unutmayın. 

## <a name="limitations"></a>Sınırlamalar
Bu belgede özetlenen yaklaşımlar birkaç sınırlama oluşturulmasını gerektirir: 

* Kullanan EF uygulamaları **LocalDb** esnek veritabanı istemci kitaplığı kullanmadan önce normal bir SQL Server veritabanına geçirmek önce gerekir. Esnek ölçeklendirme ile parçalama aracılığıyla bir uygulama ölçeğini mümkün değil **LocalDb**. Geliştirme hala kullanabileceğinizi unutmayın **LocalDb**. 
* Veritabanı şema değişiklikleri kapsıyor değişiklikleri uygulamaya tüm parça EF geçişleri geçmeniz. Bu belge için örnek kod, bunun nasıl yapılacağı gösterilmemiştir. Tüm parça yinelemek için ConnectionString parametresiyle Update-Database kullanmayı düşünün; veya Update-Database kullanarak bekleyen geçiş T-SQL komut dosyasını ayıklamak komut dosyası seçeneği ve T-SQL betiği, parça uygulayın.  
* Bir istek göz önüne alındığında, tüm veritabanı işleme içinde yer alır, tek bir parça istek tarafından sağlanan parçalama anahtarı tarafından tanımlandığı gibi varsayılır. Ancak, bu varsayım her zaman true tutmaz. Örneğin, ne zaman bir parçalama anahtarı kullanılabilir hale getirmek mümkündür değil. Bu sorunu çözmek için istemci kitaplığı sağlayan **MultiShardQuery** birkaç parça sorgulama için bir bağlantı Özet uygulayan sınıf. Kullanmayı öğrenme **MultiShardQuery** EF ile birlikte bu belgenin kapsamında değildir

## <a name="conclusion"></a>Sonuç
Bu belgede özetlenen adımları EF uygulamaları esnek veritabanı istemci kitaplığının yetenek Oluşturucular, yeniden düzenleme veri bağımlı yönlendirmek için kullanabilir **DbContext** EF uygulamada kullanılan alt sınıflar. Bu yerlerin gerekli değişiklikler bu sınırlar nerede **DbContext** sınıflar zaten mevcut. Ayrıca, EF uygulamaları yeni parça parça eşlemesindeki eşlemeleri ve gerekli EF geçişler kayıt ile çağırma adımları birleştirerek otomatik şema dağıtımından yararlanmaya devam edebilirsiniz. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
