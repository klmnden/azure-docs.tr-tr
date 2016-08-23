<properties 
    pageTitle="Redis Önbelleği ile Web Uygulamaları oluşturma | Microsoft Azure" 
    description="Redis Önbelleği ile Web Uygulaması oluşturmayı öğrenin" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="07/22/2016" 
    ms.author="sdanie"/>

# Redis Önbelleği ile Web Uygulaması oluşturma

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Bu öğreticide, ASP.NET web uygulamasının nasıl oluşturulacağını ve Visual Studio 2015 kullanılarak Azure App Service’teki bir web uygulamasına dağıtılacağı gösterilmektedir. Örnek uygulama bir veritabanındaki ekip istatistiklerinin listesini görüntüler ve önbellekten veri depolama ve almaya yönelik Azure Redis Önbelleği’ni kullanmak için farklı yollar gösterir. Öğreticiyi tamamladığınızda, Azure Redis Önbelleği ile en iyi hale getirilmiş ve Azure’da barındırılan, bir veritabanını okuyan ve yazan çalışan bir web uygulamasına sahip olacaksınız.

Şunları öğreneceksiniz:

-   Visual Studio’da ASP.NET MVC 5 web uygulaması oluşturma.
-   Entity Framework’ü kullanarak bir veritabanındaki verilere erişme.
-   Azure Redis Önbelleği’ni kullanarak veri depolayarak ve alarak veri işlemeyi iyileştirme ve veritabanı yükünü azaltma.
-   En iyi 5 ekibi almak için bir Redis sıralanmış kümesi kullanma.
-   ARM şablonunu kullanarak uygulama için Azure kaynaklarını hazırlama.
-   Visual Studio kullanarak uygulamayı yayımlama.

## Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşullara sahip olmanız gerekir.

-   [Azure hesabı](#azure-account)
-   [.NET için Windows Azure SDK içeren Visual Studio 2015](#visual-studio-2015-with-the-azure-sdk-for-net)

### Azure hesabı

Öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Şunları yapabilirsiniz:

* [Ücretsiz bir Azure hesabı açın](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Ücretli Azure hizmetlerini denemek için kullanabileceğiniz krediler alırsınız. Krediler bitmiş olsa bile hesabı sürdürebilir ve ücretsiz Azure hizmet ve özelliklerinden faydalanabilirsiniz.
* [Visual Studio abone avantajları etkinleştirin](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). MSDN aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay size kredi verir.

### .NET için Windows Azure SDK içeren Visual Studio 2015

Bu öğretici, [.NET için Azure SDK](../dotnet-sdk.md) 2.8.2 veya sonraki bir sürümünü içeren Visual Studio 2015 için hazırlanmıştır. [Visual Studio 2015 için en son Azure SDK’sını buradan indirin](http://go.microsoft.com/fwlink/?linkid=518003). Visual Studio’nuz yoksa, SDK ile otomatik olarak yüklenir.

Visual Studio 2013’ünüz varsa, [Visual Studio 2013 için en son Azure SDK'sını indirebilirsiniz](http://go.microsoft.com/fwlink/?LinkID=324322). Bu öğreticideki bazı ekranlar gösterilenlerden farklı görünebilir.

>[AZURE.NOTE] Makinenizde zaten bulunan SDK bağımlılığı sayısına bağlı olarak, SDK’nin yüklenmesi birkaç dakikadan yarım saate uzanacak şekilde biraz uzun sürebilir.

## Visual Studio projesini oluşturma

1. Visual Studio’yu açın ve **Dosya**, **Yeni**, **Proje**’yi tıklayın.

2. **Şablonlar** listesindeki **Visual C#** öğesini genişletin, **Bulut**’u seçin ve **ASP.NET Web Uygulaması**’na tıklayın. **.NET Framework 4.5.2** sürümünün seçili olduğundan emin olun.  **Ad** metin kutusunda **ContosoTeamStats** yazın ve **Tamam**’a tıklayın.
 
    ![Proje oluşturma][cache-create-project]

3. Proje türü olarak **MVC**’yi seçin. **Buluttaki konak** onay kutusunun işaretini kaldırın. Öğreticinin sonraki adımlarında [Azure kaynaklarını hazırlayacak](#provision-the-azure-resources) ve [uygulamayı Azure’a yayımlayacaksınız](#publish-the-application-to-azure). **Buluttaki konak** öğesini işaretli bırakarak Visual Studio’dan bir App Service web uygulaması hazırlama örneği için, bkz. [ASP.NET ve Visual Studio kullanarak Azure App Service’deki Web Uygulamalarını kullanmaya başlama](../app-service-web/web-sites-dotnet-get-started.md).

    ![Proje şablonu seçme][cache-select-template]

4. Projeyi oluşturmak için **Tamam**'a tıklayın.

## 4. Adım: ASP.NET MVC uygulamasını oluşturma

Öğreticinin bu bölümünde, bir veritabanındaki ekip istatistiklerini okuyan ve görüntüleyen temel uygulamayı oluşturacaksınız.

-   [Modeli ekleme](#add-the-model)
-   [Denetleyiciyi ekleme](#add-the-controller)
-   [Görünümleri yapılandırma](#configure-the-views)

### Modeli ekleme

1. **Çözüm Gezgini**’nde **Modeller**’e sağ tıklayın ve **Ekle**, **Sınıf**’ı seçin. 

    ![Model ekleme][cache-model-add-class]

2. Sınıf adı için `Team` girin ve **Ekle**’ye tıklayın.

    ![Model sınıfı ekleme][cache-model-add-class-dialog]

3. `Team.cs` dosyasının üst tarafındaki `using` deyimini aşağıdaki using deyimleri ile değiştirin.


        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


4. `Team` sınıfının tanımını, bazı diğer Entity Framework yardımcı sınıflarının yanı sıra güncelleştirilmiş `Team` sınıf tanımını içeren aşağıdaki kod parçacığı ile değiştirin. Bu öğreticide kullanılan Entity Framework için ilk kod yaklaşımı hakkında daha fazla bilgi için, bkz. [Yeni bir veritabanına ilk kod](https://msdn.microsoft.com/data/jj193542).


        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }
        
            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();
        
                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }
        
        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }
        
            public DbSet<Team> Teams { get; set; }
        }
        
        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };
        
                Team.PlayGames(teams);
        
                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }
        
        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


2. **Çözüm Gezgini**’nde, **web.config**’i açmak için sağ tıklayın.

    ![Web.config][cache-web-config]

3.  Aşağıdaki bağlantı dizesini `connectionStrings` bölümüne ekleyin. Bağlantı dizesinin adını Entity Framework veritabanı bağlamı sınıfının adı olan `TeamContext` ile eşleşmelidir.

        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />


    Bunu ekledikten sonra, `connectionStrings` bölümü aşağıdaki örnek gibi görünmelidir.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"  providerName="System.Data.SqlClient" />
        </connectionStrings>

### Denetleyiciyi ekleme

1. Projeyi derlemek için **F6**’ya basın. 
2. **Çözüm Gezgini**'nde **Denetleyiciler** klasörüne sağ tıklayın ve **Ekle**, **Denetleyici**'yi seçin.

    ![Denetleyici ekleme][cache-add-controller]

3. **Görünümlere sahip MVC 5 Denetleyici, Entity Framework kullanarak** öğesini seçin ve **Ekle**’ye tıklayın. **Ekle**’ye tıkladıktan sonra herhangi bir hata alırsanız, önce projeyi oluşturduğunuzdan emin olun.

    ![Denetleyici sınıfı ekleme][cache-add-controller-class]

5. **Model sınıfı** açılır listesinden **Ekip (ContosoTeamStats.Models)** öğesini seçin. **Veri bağlamı** açılır listesinden **TeamContext (ContosoTeamStats.Models)** öğesini seçin. **Denetleyici** adı metin kutusuna `TeamsController` yazın (otomatik olarak doldurulmamışsa). Denetleyici sınıfını oluşturmak ve varsayılan görünümleri eklemek için **Ekle**’ye tıklayın.

    ![Denetleyici yapılandırma][cache-configure-controller]

4. **Çözüm Gezgini**’nde, **Global.asax** öğesini genişletin ve **Global.asax.cs**’yi açmak için çift tıklayın.

    ![Global.asax.cs][cache-global-asax]

5. Aşağıdaki iki using deyimini dosyanın üst tarafındaki diğer using deyimlerinin altına ekleyin.


        using System.Data.Entity;
        using ContosoTeamStats.Models;


6. `Application_Start` yönteminin sonuna aşağıdaki kod satırını ekleyin.


        Database.SetInitializer<TeamContext>(new TeamInitializer());


7. **Çözüm Gezgini**’nde, `App_Start` öğesini genişletin ve `RouteConfig.cs` öğesine çift tıklayın.

    ![RouteConfig.cs][cache-RouteConfig-cs]

8. Aşağıdaki örnekte gösterildiği gibi `controller = "Home"` öğesini `RegisterRoutes` yöntemindeki kod `controller = "Teams"` ile değiştirin.


        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### Görünümleri yapılandırma

1. **Çözüm Gezgini**’nde, **Görünümler** klasörünü ve ardından **Paylaşılan** klasörünü genişletin ve **_Layout.cshtml** öğesine çift tıklayın. 

    ![_Layout.cshtml][cache-layout-cshtml]

2. `title` öğesinin içeriğini değiştirin ve aşağıdaki örnekte gösterildiği gibi `My ASP.NET Application` öğesini `Contoso Team Stats` ile değiştirin.


        <title>@ViewBag.Title - Contoso Team Stats</title>


3. `body` bölümünde, ilk `Html.ActionLink` deyimini güncelleştirin ve `Application name` öğesini `Contoso Team Stats` ile ve `Home` öğesini `Teams` ile değiştirin.
    -   Önce: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
    -   Sonra: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`

    ![Kod değişiklikleri][cache-layout-cshtml-code]

4. Uygulamayı derleyip çalıştırmak için **Ctrl+F5**'e basın. Uygulamasının bu sürümü, sonuçları doğrudan veritabanından okur. **Yeni Oluştur**, **Düzenle**, **Ayrıntılar** ve **Sil** eylemlerinin **Görünümlere sahip MVC 5 Denetleyici, Entity Framework kullanarak** iskelesi tarafından otomatik olarak uygulamaya eklendiğini unutmayın. Öğreticinin sonraki bölümünde, veri erişimini iyileştirmek ve uygulamaya ek özellikler sağlamak için Redis Önbelleği ekleyeceksiniz.

![Başlangıç uygulaması][cache-starter-application]

## Redis Önbelleğini kullanmak için uygulamayı yapılandırma

Öğreticinin bu bölümünde, [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) önbellek istemcisini kullanarak bir Azure Redis Önbelleği’nden Contoso ekip istatistiklerini depolamak ve almak için örnek uygulamayı yapılandıracaksınız.

-   [StackExchange.Redis kullanmak için uygulamayı yapılandırma](#configure-the-application-to-use-stackexchangeredis)
-   [Önbellek veya veritabanından sonuçları döndürmek için TeamsController sınıfını güncelleştirme](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
-   [Önbellek ile çalışacak şekilde Oluştur, Düzenle ve Sil yöntemlerini güncelleştirme](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
-   [Önbellek ile çalışacak şekilde Ekipler Dizini görünümünü güncelleştirme](#update-the-teams-index-view-to-work-with-the-cache)


### StackExchange.Redis kullanmak için uygulamayı yapılandırma

1. Visual Studio’da StackExchange.Redis NuGet paketi kullanarak bir istemci uygulamasını yapılandırmak için, **Çözüm Gezgini**’nde sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin. 

    ![NuGet paketlerini yönetme][redis-cache-manage-nuget-menu]

2. Arama metin kutusuna **StackExchange.Redis** yazın, sonuçlardan istediğiniz sürümü seçin ve **Yükle**’ye tıklayın.

    ![StackExchange.Redis NuGet paketi][redis-cache-stack-exchange-nuget]

    NuGet paketi, StackExchange.Redis önbellek istemcisiyle Azure Redis Önbelleğine erişmek üzere istemci uygulamanız için gerekli derleme başvurularını ekler. **StackExchange.Redis** istemci kitaplığının kesin adlandırılmış bir sürümünü kullanmak isterseniz, **StackExchange.Redis.StrongName** seçeneğin seçin; istemezseniz **StackExchange.Redis** seçeneğin seçin.

3. **Çözüm Gezgini**’nde, **Denetleyiciler** klasörünü genişletin ve **TeamsController.cs** öğesini açmak için çift tıklayın.

    ![Ekip denetleyicisi][cache-teamscontroller]

4. **TeamsController.cs** deyimlerini kullanarak aşağıdaki iki öğeyi ekleyin.

        using System.Configuration;
        using StackExchange.Redis;

5. Aşağıdaki iki özelliği `TeamsController` sınıfına ekleyin.

        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
  
1. Bilgisayarınızda `WebAppPlusCacheAppSecrets.config` adlı bir dosya oluşturun ve örnek karar içinde uygulamanızın kaynak kodu ile denetlenmeyecek bir konuma yerleştirin, başka bir yerde denetlemeyi seçmelisiniz. Bu örnekte, `AppSettingsSecrets.config` dosyası `C:\AppSecrets\WebAppPlusCacheAppSecrets.config` konumunda bulunur.

    `WebAppPlusCacheAppSecrets.config` dosyasını düzenleyin ve aşağıdaki içerikleri ekleyin. Uygulamayı yerel olarak çalıştırırsanız, Azure Redis Önbelleği örneğinize bağlanmak için bu bilgiler kullanılır. Öğreticide daha sonra bir Azure Redis Önbelleği örneği hazırlayacak ve önbellek adı ve parolasını güncelleştireceksiniz. Örnek uygulamayı yerel olarak çalıştırmayı düşünmüyorsanız, Azure’a dağıtırken uygulama Web Uygulaması için önbellek bağlantı bilgilerini bu dosya yerine uygulama ayarlarından aldığı için bu dosyayı oluşturma ve sonraki adımları atlayabilirsiniz. `WebAppPlusCacheAppSecrets.config` öğesi uygulamanızla birlikte Azure’a dağıtılmadığı için, uygulamayı yerel olarak çalıştırmayacağınız sürece ihtiyacınız olmayacaktır.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


2. **Çözüm Gezgini**’nde, **web.config**’i açmak için sağ tıklayın.

    ![Web.config][cache-web-config]

3. Aşağıdaki `file` özniteliğini `appSettings` öğesine ekleyin. Farklı bir dosya adı veya konumu kullandıysanız, örnekte gösterilenlerin yerine bu değerleri koyun.
    -   Önce: `<appSettings>`
    -   Sonra: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`

    ASP.NET çalışma zamanı, `<appSettings>` öğesindeki biçimlendirmeye sahip harici dosyasının içeriğini birleştirir. Belirtilen dosya bulunamazsa, çalışma zamanı dosya özniteliğini yok sayar. Gizli anahtarlarınız (önbelleğinize bağlantı dizisi) uygulamanız için kaynak kodun bir parçası olarak dahil edilmez. Web uygulamanızı Azure’a dağıtırken, `WebAppPlusCacheAppSecrests.config` dosyası dağıtılmaz (istediğiniz gibi). Bu gizli anahtarları Azure’da belirtmenin birkaç yolu vardır ve bir sonraki öğretici adımında [Azure kaynaklarını hazırlarken](#provision-the-azure-resources) sizin için otomatik olarak yapılandırılır. Azure'daki gizli anahtarlarla çalışma hakkında daha fazla bilgi için, bkz. [Parolaları ve diğer hassas verileri ASP.NET ve Azure App Service’e dağıtmak için en iyi yöntemler](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).


### Önbellek veya veritabanından sonuçları döndürmek için TeamsController sınıfını güncelleştirme

Bu örnekte, ekip istatistikleri veritabanı veya önbellekten alınabilir. Ekip istatistikleri seri hale getirilmiş bir `List<Team>` ve ayrıca, Redis veri türleri kullanılarak sıralanmış bir küme olarak veritabanında depolanır. Bir sıralanmış kümeden öğeleri alırken, belirli öğeler için bazı, tümü veya sorgu alabilirsiniz. Bu örnekte, kazanma sayısına göre sıralanan en iyi 5 ekip için sıralanmış kümeyi sorgulayacaksınız.

>[AZURE.NOTE] Azure Redis Önbelleği’ni kullanabilmek için ekip istatistiklerini önbellekte çoklu biçimlerde depolamak gerekli değildir. Bu öğretici, verileri önbelleğe almak için kullanabileceğiniz farklı yol ve farklı veri türlerinin bazılarını göstermek için birden çok biçim kullanır.



1. Aşağıdaki using deyimlerini `TeamsController.cs` dosyasının üst tarafındaki diğer using deyimleri ile değiştirin.

        using System.Diagnostics;
        using Newtonsoft.Json;

2. Geçerli `public ActionResult Index()` yöntemini aşağıdaki uygulama ile değiştirin.


        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;
        
            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;
        
                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;
        
                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }
        
            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();
        
            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;
        
                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;
        
                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;
        
                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }
        
            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();
        
            return View(teams);
        }


3. Önceki kod parçacığında eklenen switch deyiminden `playGames`, `clearCache` ve `rebuildDB` eylem türlerini uygulamak için aşağıdaki üç yöntemi `TeamsController` sınıfına ekleyin.

    `PlayGames` yöntemi, oyun sezonunu taklit ederek ekip istatistiklerini güncelleştirir, sonuçları veritabanına kaydeder ve artık güncel olmayan verileri veritabanından temizler.


        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;
    
            Team.PlayGames(teams);
    
            db.SaveChanges();
    
            // Clear any cached results
            ClearCachedTeams();
        }


    `RebuildDB` yöntemi, varsayılan ekip kümesine sahip veritabanını yeniden başlatır, bunlar için istatistikler oluşturur ve artık güncel olmayan verileri veritabanından temizler.
    
        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


     `ClearCachedTeams` yöntemi önbelleğe alınan tüm ekip istatistiklerini önbellekten kaldırır.

    
        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


4. Önbellek ve veritabanından ekip istatistiklerini almanın çeşitli yollarını uygulamak için aşağıdaki dört yöntemi `TeamsController` sınıfına ekleyin. Bu yöntemlerin her biri daha sonra görünüm tarafından görüntülenen bir `List<Team>` döndürür.

    `GetFromDB` yöntemi veritabanından ekip istatistiklerini okur.

        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
    
            return results.ToList<Team>();
        }


    `GetFromList` yöntemi önbellekteki ekip istatistiklerini seri hale getirilmiş bir `List<Team>` olarak okur. Önbellek isabetsizliği varsa, ekip istatistikleri veritabanından okunur ve ardından gelecek sefer için önbellekte depolanır. Bu örnekte, önbelleğe veya önbellekten .NET nesnelerini seri hale getirmek için JSON.NEY serileştirmeyi kullanıyoruz. Daha fazla bilgi için, bkz. [Azure Redis Önbelleği’nde .NET nesneleri ile çalışma](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    `GetFromSortedSet` yöntemi önbelleğe alınan bir sıralanmış kümeden ekip istatistiklerini okur. Önbellek isabetsizliği varsa, ekip istatistikleri veritabanından okunur ve ardından bir sıralanmış küme olarak önbellekte depolanır.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";
    
                // Read from DB
                teams = GetFromDB();
    
                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    `GetFromSortedSetTop5` yöntemi önbelleğe alınan sıralanmış kümesinden en iyi 5 ekibi okur. Bu, `teamsSortedSet` anahtarının varlığı için önbelleği denetleyerek başlar. Bu anahtar yoksa, ekip istatistikleri okumak ve bunları önbellekte depolamak için `GetFromSortedSet` yöntemi çağrılır. Daha sonra önbelleğe alınan sıralanmış küme, döndürülen en iyi 5 ekip için sorgulanır.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### Önbellek ile çalışacak şekilde Oluştur, Düzenle ve Sil yöntemlerini güncelleştirme

Bu örneğin bir parçası olarak oluşturulan iskele kurma kodu ekip ekleme, düzenleme ve silme yöntemlerini içerir. Bir ekip her eklendiğinde, düzenlendiğinde veya kaldırıldığında önbellekteki veriler güncel olmayan hale gelir. Bu bölümde, önbelleğin veritabanı ile eşitlenmemiş olmaması için önbelleğe alınan ekipleri temizlemek üzere bu üç yöntemi değiştireceksiniz.

1. `TeamsController` sınıfındaki `Create(Team team)` yöntemine göz atın. Aşağıdaki örnekte gösterildiği gibi `ClearCachedTeams` yöntemine bir çağrı ekleyin.


        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
    
            return View(team);
        }


2. `TeamsController` sınıfındaki `Edit(Team team)` yöntemine göz atın. Aşağıdaki örnekte gösterildiği gibi `ClearCachedTeams` yöntemine bir çağrı ekleyin.


        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


3. `TeamsController` sınıfındaki `DeleteConfirmed(int id)` yöntemine göz atın. Aşağıdaki örnekte gösterildiği gibi `ClearCachedTeams` yöntemine bir çağrı ekleyin.


        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### Önbellek ile çalışacak şekilde Ekipler Dizini görünümünü güncelleştirme

1. **Çözüm Gezgini**’nde, **Görünümler** klasörünü ve ardından **Ekipler** klasörünü genişletin ve **Index.cshtml** öğesine çift tıklayın.

    ![Index.cshtml][cache-views-teams-index-cshtml]

2. Dosyanın en üstüne yakın bir yerde, aşağıdaki paragraf öğesini arayın.

    ![Eylem tablosu][cache-teams-index-table]

    Bu, yeni bir ekip oluşturma bağlantısıdır. Paragraf öğesini aşağıdaki tablo ile değiştirin. Bu tabloda yeni bir ekip oluşturmak, yeni bir oyun sezonu oynama, önbelleği temizleme, önbellekten ekipleri çeşitli biçimlerde alma, veritabanından ekipleri alma ve yeni örnek veriler ile veritabanını yeniden oluşturma eylemlerinin bağlantılarını içermektedir.


        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


3. **Index.cshtml** dosyasının aşağısına kaydırın ve dosyada bulunan son tablodaki son satır olması için aşağıdaki `tr` öğesini ekleyin.

        <tr><td colspan="5">@ViewBag.Msg</td></tr>

    Bu satır, önceki adımdaki eylem bağlantılarından birine tıkladığınızda ayarlanan geçerli işlem hakkında bir durum raporu içeren `ViewBag.Msg` öğesinin değerini görüntüler.   

    ![Durum iletisi][cache-status-message]

4. Projeyi derlemek için **F6**’ya basın.

## Azure kaynaklarını hazırlama

Uygulamanızı Azure’da barındırmak için önce uygulamanızın gerektirdiği Azure hizmetlerini hazırlamanız gerekir. Bu öğreticideki örnek uygulama aşağıdaki Azure hizmetlerini kullanır.

-   Azure Redis Önbelleği
-   App Service Web Uygulaması
-   SQL Database

Bu hizmetleri yeni veya seçtiğiniz mevcut bir kaynak grubuna dağıtmak için, aşağıdaki **Azure’a Dağıt** düğmesine tıklayın.

[![Azure’a Dağıt][deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Bu **Azure’a Dağıt** düğmesi, bu hizmetleri hazırlamak ve SQL Database için bağlantı dizesini ve Azure Redis Önbelleği bağlantı dizesi için uygulama ayarlarını belirlemek için [Web Uygulaması oluşturma artı Redis Önbelleği artı SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Hızlı Başlangıç](https://github.com/Azure/azure-quickstart-templates) şablonunu kullanır.

>[AZURE.NOTE] Bir Azure hesabınız yoksa, yalnızca birkaç dakika içinde [ücretsiz bir Azure hesabı oluşturabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).

**Azure’a Dağıt** düğmesine tıkladığınızda sizi Azure portalına götürür ve şablon tarafından açıklanan kaynakların oluşturma işlemini başlatır.

![Azure’a Dağıt][cache-deploy-to-azure-step-1]

1. **Özel dağıtım** dikey penceresinde, kullanılacak Azure aboneliğini ve mevcut bir kaynak grubu seçin veya yeni bir tane oluşturun ve kaynak grubu konumunu belirtin.
2. **Parametreler** dikey penceresinde, bir yönetici hesabı adı (**ADMINISTRATORLOGIN** - **admin** kullanmayın), yönetici oturum açma parolası (**ADMINISTRATORLOGINPASSWORD**), ve veritabanı adı (**DATABASENAME**) belirtin. Diğer parametreler, ücretsiz bir App Service barındırma planı ve ücretsiz katman ile birlikte gelmeyen SQL Database ve Azure Redis Önbelleği için daha düşük maliyetli seçenekler sunmak için yapılandırılır.
3. İsterseniz diğer ayarlardan herhangi birini değiştirin veya varsayılanları tutun ve **Tamam**’a tıklayın.


![Azure’a Dağıt][cache-deploy-to-azure-step-2]

1. **Yasal koşulları gözden geçir**’e tıklayın.
2. **Satın Al** dikey penceresindeki koşulları okuyun ve **Satın Al**’a tıklayın.
3. Kaynakları hazırlamaya başlamak için, **Özel dağıtım** dikey penceresinde **Oluştur**’a tıklayın.

Dağıtımınızın ilerlemesini görüntülemek için bildirim simgesine ve **Dağıtım başladı** öğesine tıklayın.

![Dağıtım başladı][cache-deployment-started]

**Microsoft.Template** dikey penceresinde dağıtımınızın durumunu görüntüleyebilirsiniz.

![Azure’a Dağıt][cache-deploy-to-azure-step-3]

Hazırlama işlemi tamamlandığında, uygulamanızı Visual Studio’dan Azure’a yayımlayabilirsiniz.

>[AZURE.NOTE] Sağlama işlemi sırasında oluşabilecek tüm hatalar **Microsoft.Template** dikey penceresinde görüntülenir. Abonelik başına çok fazla SQL Server veya çok fazla Ücretsiz App Service barındırma planı yaygın hatalardır. Tüm sorunları çözümleyin ve **Microsoft.Template** dikey penceresinde **Yeniden Dağıt** veya bu öğreticideki **Azure’a Dağıt** düğmesine tıklayarak işlemi yeniden başlatın.

## Uygulamayı Azure’a yayımlama

Öğreticinin bu adımında, uygulamayı Azure’a yayımlayacak ve bulutta çalıştıracaksınız.

1. Visual Studio’da **ContosoTeamStats** öğesine sağ tıklayın ve **Yayımla**’yı seçin.

    ![Yayımlama][cache-publish-app]

2. **Azure App Service**’e tıklayın.

    ![Yayımlama][cache-publish-to-app-service]

3. Azure kaynakları oluşturulurken kullanılan aboneliği seçin, kaynakları içeren kaynak grubunu genişletin, istediğiniz Web Uygulamasını seçin ve **Tamam**’a tıklayın. **Azure’a Dağıt** düğmesini kullandıysanız Web Uygulaması adınız **webSite** ile başlar ve bazı ek karakterlerle devam eder.

    ![Web Uygulaması Seçme][cache-select-web-app]

4. Ayarlarınızı doğrulamak için **Bağlantıyı Doğrula** ve ardından **Yayımla**’ya tıklayın.

    ![Yayımlama][cache-publish]

    Birkaç dakika sonra yayımlama işlemini tamamlanır ve örnek uygulamayı çalıştırarak bir tarayıcı başlatılır. Doğrularken veya yayımlarken ve henüz tamamlanan uygulama için Azure kaynakları için işlem hazırlarken bir DNS hatası alırsanız, birkaç dakika bekleyin ve tekrar deneyin.

    ![Önbellek eklendi][cache-added-to-application]

Aşağıdaki tablo örnek uygulamadaki her eylem bağlantısını açıklar.

| Eylem                  | Açıklama                                                                                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yeni Oluştur              | Yeni bir Ekip oluşturun.                                                                                                                                               |
| Sezonu Oynat             | Oyun sezonunu oynatın, ekip istatistiklerini güncelleştirin ve veritabanından tüm güncel olmayan ekip verilerini temizleyin.                                                                          |
| Önbelleği Temizle             | Önbellekten ekip istatistiklerini temizleyin.                                                                                                                             |
| Önbellekten Liste         | Önbellekten ekip istatistiklerini alın. Önbellek isabetsizliği varsa, veritabanından istatistikleri yükleyin ve bir sonraki seferde önbelleğe kaydedin.                                        |
| Önbellekten Sıralanmış Küme   | Bir sıralanmış küme kullanarak önbellekten en iyi istatistiklerini alın. Önbellek isabetsizliği varsa, veritabanından istatistikleri yükleyin ve bir sıralanmış küme kullanarak önbelleğe kaydedin.  |
| Önbellekteki En İyi 5 Ekip  | Bir sıralanmış küme kullanarak önbellekten en iyi 5 ekibi alın. Önbellek isabetsizliği varsa, veritabanından istatistikleri yükleyin ve bir sıralanmış küme kullanarak önbelleğe kaydedin. |
| DB’den yükleme            | Veritabanından ekip istatistiklerini alın.                                                                                                                       |
| DB Yeniden Oluşturma              | Veritabanını yeniden oluşturun ve örnek ekip verileri ile yeniden yükleyin.                                                                                                        |
| Düzenle / Ayrıntılar / Sil | Bir ekibi düzenleyin, ekibin ayrıntılarını görüntüleyin, ekibi silin.                                                                                                             |


Eylemlerden bazılarına tıklayın ve farklı kaynaklardan veri alma denemeleri yapın. Veritabanı veya önbellekten veri almanın çeşitli yollarını tamamlamak için gereken zaman içindeki farklılıklar değildir.

## Uygulama ile işiniz bittiğinde kaynakları silme

Örnek öğretici uygulamasıyla işiniz bittiğinde, maliyet ve kaynakları korumak için kullanılan Azure kaynaklarını silebilirsiniz. **Azure kaynaklarını hazırlama** bölümünde [Azure’a Dağıt](#provision-the-azure-resources) düğmesini kullanırsanız ve tüm kaynaklarınız aynı grupta bulunuyorsa, kaynak grubunu silerek bunları tek bir işlemde silebilirsiniz.

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.
2. **Öğeleri filtrele...** metin kutusuna kaynak grubunuzun adını yazın.
3. Kaynak grubunuzun sağındaki **...** öğesine tıklayın.
4. **Sil**'e tıklayın.

    ![Sil][cache-delete-resource-group]

5. Kaynak grubunuzun adını yazın ve **Sil**’e tıklayın.

    ![Silmeyi onayla][cache-delete-confirm]

Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.

>[AZURE.IMPORTANT] Bir kaynak grubunu silme işleminin geri alınamaz olduğunu ve kaynak grubunun ve içindeki tüm kaynakların kalıcı olarak silindiğini unutmayın. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. Bu örneği mevcut bir kaynak grubunda barındırmak için kaynaklar oluşturduysanız, her kaynağı kendi ilgili dikey penceresinden tek tek silebilirsiniz.

## Örnek uygulamayı yerel makinenizde çalıştırma

Uygulamayı makinenizde yerel olarak çalıştırmak için, verilerinizi önbelleğe almak üzere bir Azure Redis Önbelleği örneğine ihtiyacınız olacaktır. 

-   Önceki bölümde açıklandığı gibi Azure uygulamanızı yayımladıysanız, bu adım sırasında sağlanan Azure Redis Önbelleği örneğini kullanabilirsiniz.
-   Mevcut başka bir Azure Redis Önbelleği örneğiniz varsa, bu örneği yerel olarak çalıştırmak için kullanabilirsiniz.
-   Bir Azure Redis Önbelleği örneği oluşturmanız gerekiyorsa, [Önbellek oluşturma](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache) makalesindeki adımları uygulayabilirsiniz.

Kullanılacak önbelleği seçtikten veya oluşturduktan sonra, Azure portalında önbelleğe göz atın ve önbelleğiniz için [konak adı](cache-configure.md#properties) ve [erişim anahtarlarını](cache-configure.md#access-keys) alın. Yönergeler için bkz. [Redis önbelleği ayarlarını yapılandırma](cache-configure.md#configure-redis-cache-settings).

1. İstediğiniz düzenleyiciyi kullanarak bu öğreticinin [Redis Önbelleği’ni kullanmak için uygulamayı yapılandırma](#configure-the-application-to-use-redis-cache) adımında oluşturduğunuz `WebAppPlusCacheAppSecrets.config` dosyasını açın.

2. `value` özniteliğini düzenleyin ve `MyCache.redis.cache.windows.net` öğesini önbelleğinizin [konak adı](cache-configure.md#properties) ile değiştirin ve parola olarak önbelleğinizin [birincil veya ikincil anahtarını](cache-configure.md#access-keys) belirtin.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


3. Uygulamayı çalıştırmak için **Ctrl+F5**'e basın.

>[AZURE.NOTE] Veritabanı da dahil olmak üzere uygulama yerel olarak çalıştığı ve Redis Önbelleği’nin Azure’da barındırıldığı için, önbellek veritabanı altında gerçekleştirmek için görünebileceğini unutmayın. En iyi performans için, istemci uygulaması ve Azure Redis Önbelleği örneği aynı konumda olmalıdır. 

## Sonraki adımlar

-   [ASP.NET](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) sitesinde [ASP.NET MVC 5 ile Çalışmaya Başlama](http://asp.net/) hakkında daha fazla bilgi edinin.
-   App Service’te ASP.NET Web Uygulaması oluşturmaya yönelik daha fazla örnek için [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [tanıtımı](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) içindeki [Azure Uygulama Hizmeti’nde ASP.NET web uygulaması oluşturma ve dağıtma](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) bölümüne bakın.
    -   HealthClinic.biz tanıtımından daha fazla hızlı başlangıç ipuçları için bkz. [Azure Geliştirici Araçları Hızlı Başlangıç İpuçları](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
-   Bu öğreticide kullanılan Entity Framework için [Yeni bir veritabanına ilk kod](https://msdn.microsoft.com/data/jj193542) yaklaşımı hakkında daha fazla bilgi edinin.
-   [Azure App Service’deki web uygulamaları](../app-service-web/app-service-web-overview.md) hakkında daha fazla bilgi edinin.
-   Azure portalındaki önbelleğinizi nasıl [izleyeceğinizi](cache-how-to-monitor.md) öğrenin.

-   Azure Redis Önbelleği premium özelliklerini keşfedin
    -   [Premium Azure Redis Önbelleği için kalıcılığı yapılandırma](cache-how-to-premium-persistence.md)
    -   [Premium Azure Redis Önbelleği için kümeleri yapılandırma](cache-how-to-premium-clustering.md)
    -   [Premium Azure Redis Önbelleği için Virtual Network desteğini yapılandırma](cache-how-to-premium-vnet.md)
    -   Boyut, işleme ve premium önbelleklere sahip bant genişliği hakkında daha fazla bilgi için, bkz. [Azure Redis Önbelleği SSS](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).



<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png




<!--HONumber=Aug16_HO1-->


