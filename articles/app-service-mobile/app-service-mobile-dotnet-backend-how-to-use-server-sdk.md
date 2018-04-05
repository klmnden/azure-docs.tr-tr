---
title: Mobil uygulamalar için .NET arka uç sunucusu SDK ile çalışmaya nasıl | Microsoft Docs
description: Azure App Service Mobile Apps için .NET arka uç sunucusu SDK ile çalışmayı öğrenin.
keywords: uygulama hizmeti, azure uygulama hizmeti, mobil uygulama, mobil hizmet, Ölçek, ölçeklenebilir, uygulama dağıtımı, azure uygulama dağıtımı
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 0620554f-9590-40a8-9f47-61c48c21076b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: a1a29d87864bff8cb2ecda70d8a0a7833c70d481
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Azure Mobile Apps için .NET arka uç sunucu SDK’sı ile çalışma
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Bu konuda anahtar Azure App Service Mobile Apps senaryolarda .NET arka uç sunucusu SDK kullanmayı gösterir. Azure Mobile Apps SDK'sı, ASP.NET uygulamanız mobil istemcilerden çalışmanıza yardımcı olur.

> [!TIP]
> [.NET server için Azure Mobile Apps SDK] [ 2] github'da açık bir kaynaktır. Depo tüm sunucu SDK birim test paketi ve bazı örnek projeler de dahil olmak üzere tüm kaynak kodunu içerir.
>
>

## <a name="reference-documentation"></a>Başvuru belgeleri
SDK sunucusu için başvuru belgeleri burada bulunur: [Azure Mobile Apps .NET başvurusu][1].

## <a name="create-app"></a>Nasıl yapılır: bir .NET Mobil uygulama arka ucu oluşturma
Yeni bir proje başlıyorsanız, kullanarak bir uygulama hizmeti uygulaması oluşturabilir [Azure portal] veya Visual Studio. Uygulama hizmeti uygulamayı yerel olarak çalıştırın veya bulut tabanlı uygulama hizmeti mobil uygulamanıza proje yayımlayın.

Varolan bir projeye mobil özelliklerini ekliyorsanız, bkz: [indirin ve SDK'sını başlatma](#install-sdk) bölümü.

### <a name="create-a-net-backend-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir .NET arka ucu oluşturma
Bir uygulama hizmeti mobil arka uç oluşturmak için aşağıdakilerden birini izleyin [hızlı başlangıç Öğreticisi] [ 3] veya şu adımları izleyin:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Geri *başlama* dikey altında **tablo API Oluştur**, seçin **C#** olarak, **arka uç dilinizi**. Tıklatın **karşıdan**sıkıştırılmış proje dosyalarını yerel bilgisayarınıza ayıklayın ve Visual Studio çözümü açın.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Visual Studio 2013 ve Visual Studio 2015 kullanarak bir .NET arka ucu oluşturma
Yükleme [.NET için Azure SDK] [ 4] (sürüm 2.9.0 veya sonrası) Visual Studio'da bir Azure Mobile Apps projesi oluşturmak için. SDK'yı yükledikten sonra aşağıdaki adımları kullanarak bir ASP.NET uygulaması oluşturun:

1. Açık **yeni proje** iletişim (gelen *dosya* > **yeni** > **proje...** ).
2. Genişletme **şablonları** > **Visual C#**seçip **Web**.
3. Seçin **ASP.NET Web uygulaması**.
4. Proje adı girin. Daha sonra, **Tamam**'a tıklayın.
5. Altında *ASP.NET 4.5.2 şablonları*seçin **Azure mobil uygulaması**. Denetleme **bulutta Barındır** mobil arka uç bu proje yayımlamak bulutta oluşturmak için.
6. **Tamam**’a tıklayın.

## <a name="install-sdk"></a>Nasıl yapılır: indirme ve SDK'sını başlatma
SDK kullanılabilir [NuGet.org]. Bu paket için SDK'sını kullanmaya başlamak için gerekli temel işlevselliğini içerir. SDK'yı başlatmak için üzerinde eylemler gerçekleştirmek gerekir **HttpConfiguration** nesnesi.

### <a name="install-the-sdk"></a>SDK yükle
SDK yüklemek için sunucu projesi Visual Studio'da seçin sağ tıklayın **NuGet paketlerini Yönet**, arama [Microsoft.Azure.Mobile.Server] paketini ve ardından **yükleme**.

### <a name="server-project-setup"></a> Sunucu projesi başlatma
Bir .NET arka uç sunucu projesi benzer diğer ASP.NET projeleri için OWIN başlangıç sınıfı dahil ederek başlatıldı. NuGet paketi başvurulan olun `Microsoft.Owin.Host.SystemWeb`. Visual Studio'da bu sınıf eklemek için sunucu projenize sağ tıklayın ve seçin **Ekle** >
**yeni öğe**, ardından **Web** > **genel** > **OWIN başlangıç sınıfı**.  Bir sınıf aşağıdaki öznitelik oluşturulur:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

İçinde `Configuration()` yöntemi, OWIN başlangıç sınıfı, kullanım, bir **HttpConfiguration** Azure Mobile Apps ortamını yapılandırma nesnesi.
Aşağıdaki örnek, hiçbir ek özellikler içeren sunucu projesi başlatır:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Tek tek özellikleri etkinleştirmek için genişletme yöntemleri üzerinde çağırmalısınız **MobileAppConfiguration** nesne çağırmadan önce **ApplyTo**. Örneğin, aşağıdaki kod özniteliğine sahip tüm API denetleyicilerinin varsayılan yolların ekler `[MobileAppController]` başlatma sırasında:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

Azure portal aramalardan server Hızlı Başlangıç **UseDefaultConfiguration()**. Aşağıdaki Kurulum bu eşdeğerdir:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

Kullanılan genişletme yöntemleri şunlardır:

* `AddMobileAppHomeController()` Varsayılan Azure Mobile Apps giriş sayfası sağlar.
* `MapApiControllers()` Webapı denetleyicileri ile donatılmış özel API yetenekleri sağlar `[MobileAppController]` özniteliği.
* `AddTables()` bir eşlenmesini sağlar `/tables` tablo denetleyicilerine uç noktaları.
* `AddTablesWithEntityFramework()` bir kısaltılmış eşlemesi için olan `/tables` Entity Framework kullanarak uç noktaları alarak denetleyicileri.
* `AddPushNotifications()` Bildirim hub'ları için cihazları kaydetme, basit bir yöntem sağlar.
* `MapLegacyCrossDomainController()` Standart CORS üstbilgilerini yerel geliştirme için sağlar.

### <a name="sdk-extensions"></a>SDK uzantıları
Aşağıdaki NuGet tabanlı uzantısı paketleri, uygulamanız tarafından kullanılabilen çeşitli mobil özellikleri sağlar. Kullanarak başlatma sırasında uzantılarını etkinleştirme **MobileAppConfiguration** nesnesi.

* [Microsoft.Azure.Mobile.Server.Quickstart] Supports the basic Mobile Apps setup. Çağırarak yapılandırmaya eklenmiş **UseDefaultConfiguration** başlatma sırasında genişletme yöntemi. Bu uzantı uzantıları aşağıdaki içerir: bildirimleri, kimlik doğrulama, varlık, tablolar, etki alanları arası ve giriş paketleri. Bu paket, Azure portalında kullanılabilir Mobile Apps Quickstart tarafından kullanılır.
* [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) varsayılan uygulayan *bu mobil uygulamayı çalışır durumda olduğundan sayfa* bir web sitesi kök. Yapılandırmaya çağırarak eklemek **AddMobileAppHomeController** genişletme yöntemi.
* [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) verilerle çalışmak için sınıflar içerir ve veri ardışık kümeleri yukarı. Yapılandırmaya çağırarak eklemek **AddTables** genişletme yöntemi.
* [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) Enables the Entity Framework to access data in the SQL Database. Yapılandırmaya çağırarak eklemek **AddTablesWithEntityFramework** genişletme yöntemi.
* [Microsoft.Azure.Mobile.Server.Authentication] Enables authentication and sets-up the OWIN middleware used to validate tokens. Yapılandırmaya çağırarak eklemek **AddAppServiceAuthentication** ve **Iappbuilder**. **UseAppServiceAuthentication** genişletme yöntemleri.
* [Microsoft.Azure.Mobile.Server.Notifications] Enables push notifications and defines a push registration endpoint. Yapılandırmaya çağırarak eklemek **AddPushNotifications** genişletme yöntemi.
* [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) Creates a controller that serves data to legacy web browsers from your Mobile App. Yapılandırmaya çağırarak eklemek **MapLegacyCrossDomainController** genişletme yöntemi.
* [Microsoft.Azure.Mobile.Server.Login] özel kimlik doğrulama senaryoları sırasında kullanılan bir statik yöntem AppServiceLoginHandler.CreateToken() yöntemi sağlar.

## <a name="publish-server-project"></a>Nasıl yapılır: Sunucu projesi yayımlama
Bu bölümde, .NET arka uç projeniz Visual Studio'dan yayımlamak nasıl gösterir. Ayrıca, arka uç projesi kullanarak dağıtabilirsiniz [Git](../app-service/app-service-deploy-local-git.md) veya diğer yöntemler kullanılabilir vardır.

1. Visual Studio'da NuGet paketlerini geri yüklemek için projeyi yeniden oluşturun.
2. Çözüm Gezgini'nde projeye sağ tıklayın, **Yayımla**. İlk yayımladığınızda, yayımlama profili tanımlamanız gerekir. Önceden tanımlı bir profili varsa, seçin ve tıklatın **Yayımla**.
3. Yayımlama hedefi seçmek için sorulursa tıklatın **Microsoft Azure App Service** > **sonraki**, sonra (gerekirse) ile Azure kimlik bilgilerinizle oturum açın.
   Visual Studio indirmeleri ve güvenli bir şekilde depolar, doğrudan Azure ayarlarını yayımlayın.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. Seçin, **abonelik**seçin **kaynak türü** gelen **Görünüm**, genişletin **mobil uygulama**ve mobil uygulama arka tıklayın ve ardından **Tamam**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. Yayımlama profili bilgilerini doğrulayın ve tıklatın **Yayımla**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Mobil uygulama arka başarıyla yayımladı, başarıyı gösteren bir giriş sayfasına bakın.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <a name="define-table-controller"></a> Nasıl yapılır: bir tablo denetleyicisi tanımlayın
Mobil istemcilerin bir SQL tablosuna kullanıma sunmak için bir tablo denetleyicisi tanımlayın.  Bir tablo denetleyicisi yapılandırma üç adımı gerektirir:

1. Bir veri aktarım nesnesini (DTO) sınıf oluşturun.
2. Bir tablo başvurusu mobil DbContext sınıfında yapılandırın.
3. Bir tablo denetleyicisi oluşturun.

Bir veri aktarım nesnesini (DTO) öğesinden devralınan bir düz C# nesnesidir `EntityData`.  Örneğin:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

DTO tablo SQL veritabanı içinde tanımlamak için kullanılır.  Veritabanı girişi oluşturmak için Ekle bir `DbSet<>` kullanmakta olduğunuz DbContext özelliğine.  Azure Mobile Apps için varsayılan proje şablonu DbContext adlı `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Azure SDK'sı yüklü varsa, artık bir şablon tablo denetleyicisi aşağıdaki gibi oluşturabilirsiniz:

1. Denetleyicileri klasörü sağ tıklatın ve seçin **Ekle** > **denetleyicisi...** .
2. Seçin **Azure Mobile Apps tablo denetleyicisi** seçeneğini ve ardından **Ekle**.
3. İçinde **denetleyici Ekle** iletişim:
   * İçinde **Model sınıfı** açılan listesinde, yeni DTO seçin.
   * İçinde **DbContext** açılan listesinde, mobil hizmet DbContext sınıfı seçin.
   * Denetleyici adı sizin için oluşturulur.
4. **Ekle**'ye tıklayın.

Basit bir örneğin hızlı başlangıç sunucu projesi içeren **TodoItemController**.

### <a name="adjust-pagesize"></a>Nasıl yapılır: Tablo disk belleği boyutunu ayarlama
Varsayılan olarak, Azure Mobile Apps istek başına 50 kayıt döndürür.  Disk belleği, istemci kendi kullanıcı Arabirimi iş parçacığı veya çok uzun bir süre için sunucunun yukarı iyi bir kullanıcı deneyimi sağlayarak tie değil, sağlar. Tablo disk belleği boyutunu değiştirmek için "izin verilen sorgu boyutu" sunucu tarafı artırmak ve istemci tarafı sayfa boyutu sunucu tarafı "sorgu boyutu izin verilen" olarak ayarlandı kullanarak `EnableQuery` özniteliği:

    [EnableQuery(PageSize = 500)]

PageSize aynı olduğundan emin olun veya istemci tarafından istenilen boyuttan büyük.  Belirli istemci istemci sayfa boyutunu değiştirme hakkında ayrıntılar için nasıl yapılır belgelerine bakın.

## <a name="how-to-define-a-custom-api-controller"></a>Nasıl yapılır: özel bir API denetleyicisi tanımlayın
Özel API denetleyicisi bir uç nokta göstererek, mobil uygulamanızın arka ucuna en temel işlevsellik sağlar. [MobileAppController] özniteliği kullanılarak mobile özgü API denetleyicisi kaydedebilirsiniz. `MobileAppController` Özniteliği rota kaydeder, Mobile Apps JSON seri hale getirici ayarlar ve açar [istemci sürüm denetimi](app-service-mobile-client-and-server-versioning.md).

1. Visual Studio'da denetleyicileri klasörüne sağ tıklayın ve ardından **Ekle** > **denetleyicisi**seçin **Web API 2 denetleyicisi&mdash;boş** tıklatıp **Ekle**.
2. Tedarik bir **Denetleyici adı**, gibi `CustomController`, tıklatıp **Ekle**.
3. Yeni denetleyici sınıf dosyasında, aşağıdaki ekleme deyimini kullanarak:

        using Microsoft.Azure.Mobile.Server.Config;
4. Uygulama **[MobileAppController]** özniteliği aşağıdaki örnekte olduğu gibi API denetleyicisi sınıfı tanımı:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. App_Start/Startup.MobileApp.cs dosyasında bir çağrı ekleyin **MapApiControllers** genişletme yöntemi, aşağıdaki örnekteki gibi:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Aynı zamanda `UseDefaultConfiguration()` genişletme yöntemi yerine `MapApiControllers()`. Sahip olmadığı herhangi bir denetleyicisi **MobileAppControllerAttribute** uygulanan hala istemcileri tarafından erişilebilen, ancak bunu doğru şekilde herhangi bir mobil uygulama istemci SDK kullanan istemciler tarafından tüketilmeyen.

## <a name="how-to-work-with-authentication"></a>Nasıl yapılır: kimlik doğrulaması ile çalışma
Azure Mobile Apps kullanan App Service kimlik doğrulama / yetkilendirme, mobil arka uç güvenli hale getirmek için.  Bu bölümde aşağıdaki kimlik doğrulama ile ilgili görevlerin .NET arka uç sunucu projenizi nasıl gerçekleştirileceğini gösterir:

* [Nasıl yapılır: bir sunucu projesi için kimlik doğrulaması ekleme](#add-auth)
* [Nasıl yapılır: uygulamanız için özel kimlik doğrulamasını kullan](#custom-auth)
* [Nasıl yapılır: alma kimliği doğrulanmış kullanıcı bilgileri](#user-info)
* [Nasıl yapılır: yetkili kullanıcıların veri erişimini kısıtlamak](#authorize)

### <a name="add-auth"></a>Nasıl yapılır: bir sunucu projesi için kimlik doğrulaması ekleme
Kimlik doğrulama sunucu projenizi genişleterek ekleyebileceğiniz **MobileAppConfiguration** nesne ve OWIN ara yazılımı yapılandırma. Yüklediğinizde [Microsoft.Azure.Mobile.Server.Quickstart] paket ve çağrısı **UseDefaultConfiguration** genişletme yöntemi, 3. adıma atlayabilirsiniz.

1. Visual Studio'da yükleme [Microsoft.Azure.Mobile.Server.Authentication] paket.
2. Haline proje dosyasında başında aşağıdaki kod satırını ekleyin **yapılandırma** yöntemi:

        app.UseAppServiceAuthentication(config);

    Bu OWIN ara yazılım bileşeni ilişkili uygulama hizmeti ağ geçidi tarafından yayınlanan belirteçleri doğrular.
3. Ekleme `[Authorize]` özniteliği herhangi bir denetleyici veya kimlik doğrulama gerektiren yöntemi.

Mobile Apps arka istemcilerin kimliğini doğrulamak nasıl hakkında bilgi edinmek için [uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Nasıl yapılır: uygulamanız için özel kimlik doğrulamasını kullan
> [!IMPORTANT]
> Özel kimlik doğrulamasını etkinleştirmek için önce Azure portalında uygulama hizmetiniz için bir sağlayıcı seçmeden App Service kimlik doğrulamasını etkinleştirmeniz gerekir. Bu barındırıldığında WEBSITE_AUTH_SIGNING_KEY ortam değişkeni olanak tanır.
> 
> 
App Service kimlik doğrulama/yetkilendirme sağlayıcılardan biri kullanmak istemiyorsanız, kendi oturum açma sistem uygulayabilirsiniz. Yükleme [Microsoft.Azure.Mobile.Server.Login] ile kimlik doğrulama belirteci oluşturma yardımcı olmak üzere paket.  Kullanıcı kimlik bilgilerini doğrulamak için kendi kodunuzu girin. Örneğin, güvenlik ve karma parolaları bir veritabanında karşı denetleyebilirsiniz. Aşağıdaki örnekte `isValidAssertion()` yöntemi (başka bir yerde tanımlanır) için bu denetimleri sorumlu.

Özel kimlik doğrulama bir ApiController oluşturma ve gösterme sunulan `register` ve `login` eylemler. İstemci, kullanıcıdan bilgi toplamak için özel bir kullanıcı Arabirimi kullanmanız gerekir.  Bilgi, standart HTTP POST çağrısıyla API sonra gönderilir. Onaylama işlemi sunucuyu doğrular sonra bir belirteç kullanarak verilen `AppServiceLoginHandler.CreateToken()` yöntemi.  ApiController **vermemelisiniz** kullanmak `[MobileAppController]` özniteliği.

Örnek `login` eylem:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

Önceki örnekte LoginResult ve LoginResultUser gerekli özellikleri gösterme seri hale getirilebilir nesneleridir. İstemci oturum açma yanıt biçiminde JSON nesneler olarak döndürülecek bekler:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

`AppServiceLoginHandler.CreateToken()` Yöntemi içeren bir *İzleyici* ve bir *veren* parametresi. Bu parametrelerin her ikisini de HTTPS şeması kullanarak URL'ye, uygulamanızın kök ayarlanır. Benzer şekilde ayarlamalısınız *secretKey* uygulamanızı değerini anahtar imzalama kullanıcının olması. Anahtarları Naneli ve kullanıcıları taklit etmek için kullanılabilir olarak bir istemci imzalama anahtarı dağıtmayın. App Service içinde başvurarak barındırılan sırada imzalama anahtarı edinebilirsiniz *Web sitesi\_AUTH\_imzalama\_anahtar* ortam değişkeni. Yerel hata ayıklama bağlamda gerekirse'ndaki yönergeleri izleyin [yerel kimlik doğrulaması ile hata ayıklama](#local-debug) anahtarı almak ve bir uygulama ayarı olarak depolamak için bölüm.

Verilen belirteç, ayrıca diğer talepleri ve sona erme tarihi içerebilir.  Verilen belirteç en azından bir konu içermelidir (**alt**) talep.

Standart istemci destekleyebilir `loginAsync()` tarafından kimlik doğrulaması rota aşırı yükleme yöntemi.  İstemci çağırırsa `client.loginAsync('custom');` oturum açmak için yönlendirme olmalı `/.auth/login/custom`.  Özel kimlik doğrulama kullanarak denetleyici için rota ayarlayabilirsiniz `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> Kullanarak `loginAsync()` yaklaşım sağlar sonraki her çağrı için hizmet kimlik doğrulama belirteci eklenir.
>
>

### <a name="user-info"></a>Nasıl yapılır: alma kimliği doğrulanmış kullanıcı bilgileri
App Service tarafından bir kullanıcı kimlik doğrulaması yapıldığında .NET arka uç kodunuzun atanan kullanıcı Kimliğini ve diğer bilgilere erişebilirsiniz. Kullanıcı bilgilerini arka yetkilendirme kararları kullanılabilir. Aşağıdaki kod, bir istekle ilişkili kullanıcı kimliği alır:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

SID, sağlayıcıya özgü kullanıcı ID'den türetilmiş ve verilen kullanıcı ve oturum açma sağlayıcısı için statiktir.  SID için geçersiz kimlik doğrulama belirteçleri null şeklindedir.

Uygulama Hizmeti ayrıca oturum açma sağlayıcınızdan belirli talep istemenize olanak tanır. Her bir kimlik sağlayıcısı kimlik sağlayıcısı SDK kullanarak daha fazla bilgi sağlayabilir.  Örneğin, Facebook grafik API'si arkadaş bilgilerini kullanabilirsiniz.  Azure portalında sağlayıcısı dikey penceresinde istenen taleplerin belirtebilirsiniz. Bazı talep kimlik sağlayıcısı ile ek yapılandırma gerektirir.

Aşağıdaki kod çağrıları **GetAppServiceIdentityAsync** erişim dahil oturum açma kimlik bilgileri belirteci almak için genişletme yöntemi gerekli Facebook grafik API'si istekler yapmak:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Kullanarak bir ekleme deyimi için `System.Security.Principal` sağlamak için **GetAppServiceIdentityAsync** genişletme yöntemi.

### <a name="authorize"></a>Nasıl yapılır: yetkili kullanıcıların veri erişimini kısıtlamak
Önceki bölümde biz kimliği doğrulanmış bir kullanıcı kullanıcı Kimliğini almak nasıl oluşturulacağını gösterir. Veri ve bu değere göre diğer kaynaklara erişimi kısıtlayabilirsiniz. Örneğin, bir kullanıcı kimliği sütunu tablolarına ekleme ve kullanıcı Kimliğine göre sorgu sonuçlarını filtreleme döndürülen verilerin yalnızca yetkili kullanıcılara sınırlamak için bir basit yoludur. Yalnızca SID Todoıtem tablosu üzerinde UserID sütunundaki değeri eşleştiğinde aşağıdaki kod veri satırlarını döndürür:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

`Query()` Yöntemi döndürür bir `IQueryable` filtreleme işlemek için LINQ tarafından yönetilebilir.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Nasıl yapılır: anında iletme eklemek sunucu projesi bildirimleri
Genişleterek sunucu projenizi anında iletme bildirimleri ekleme **MobileAppConfiguration** nesne ve bildirim hub'ları istemci oluşturma.

1. Visual Studio'da sunucu projesi sağ tıklatın ve **NuGet paketlerini Yönet**, arama `Microsoft.Azure.Mobile.Server.Notifications`, ardından **yükleme**.
2. Yüklemek için bu adımı yineleyin `Microsoft.Azure.NotificationHubs` bildirim hub'ları istemci kitaplığı içeren paket.
3. App_Start/Startup.MobileApp.cs içinde ve bir çağrı ekleyin **AddPushNotifications()** genişletme yöntemi başlatma sırasında:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. Bildirim hub'ları istemci oluşturur aşağıdaki kodu ekleyin:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Kayıtlı cihazlara anında iletme bildirimleri göndermek için Notification Hubs istemci artık kullanabilirsiniz. Daha fazla bilgi için bkz: [uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-ios-get-started-push.md). Notification Hubs hakkında daha fazla bilgi için bkz: [Notification Hubs'a genel bakış](../notification-hubs/notification-hubs-push-notification-overview.md).

## <a name="tags"></a>Nasıl yapılır: etkinleştir hedeflenen etiketleri kullanarak anında iletme
Bildirim hub'ları etiketleri kullanarak belirli kayıtlar için hedeflenen bildirimleri göndermenize olanak sağlar. Birkaç etiket otomatik olarak oluşturulur:

* Belirli bir aygıt yükleme kimliği tanımlar.
* Belirli bir kullanıcı kimliği doğrulanmış SID tabanlı kullanıcı kimliği tanımlar.

Kimliği erişilebilir gelen yükleme **InstallationID** özelliği **MobileServiceClient**.  Aşağıdaki örnek, bildirim hub'ları belirli bir yüklemede bir etiket eklemek için bir yükleme kimliği kullanmayı gösterir:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Anında iletme bildirimi kaydı sırasında istemci tarafından sağlanan herhangi bir etiket arka ucu tarafından yükleme oluştururken göz ardı edilir. Etiketler yüklemesine eklemek bir istemci etkinleştirmek için önceki desenini kullanarak etiketleri ekler özel bir API oluşturmanız gerekir.

Bkz: [istemci eklenen anında iletme bildirimi etiketleri] [ 5] App Service Mobile Apps tamamlanmış hızlı başlangıç örnek bir örnek.

## <a name="push-user"></a>Nasıl yapılır: kimliği doğrulanmış bir kullanıcı için anında iletme bildirimleri gönderme
Kimliği doğrulanmış bir kullanıcı için anında iletme bildirimleri kaydolduğunda, bir kullanıcı kimliği etiketi kayıt için otomatik olarak eklenir. Bu etiket kullanarak, söz konusu kişi tarafından kaydedilen tüm cihazlara anında iletme bildirimleri gönderebilirsiniz. Aşağıdaki kod, isteği yapan kullanıcı SID'si alır ve bu kişi için her aygıt kaydı için bir şablon anında iletme bildirimi gönderir:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Kimliği doğrulanmış bir istemci anında iletme bildirimleri için kaydolurken kayıt denemeden önce kimlik doğrulamasının tam olduğundan emin olun. Daha fazla bilgi için bkz: [kullanıcılara anında iletme] [ 6] .NET arka ucu App Service Mobile Apps tamamlanmış hızlı başlangıç örnek.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Nasıl yapılır: hata ayıklama ve .NET sunucusu SDK'sı sorun giderme
Azure uygulama hizmeti birkaç hata ayıklama ve sorun giderme teknikleri ASP.NET uygulamaları için sağlar:

* [Bir Azure uygulama hizmeti izleme](../app-service/web-sites-monitor.md)
* [Azure uygulama hizmetinde tanılama günlük kaydını etkinleştir](../app-service/web-sites-enable-diagnostic-log.md)
* [Visual Studio'da bir Azure uygulama hizmeti sorunlarını giderme](../app-service/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Günlüğe kaydetme
Uygulama hizmeti tanılama günlükleri için standart ASP.NET izleme yazma kullanarak yazabilirsiniz. Günlüklere yazılır önce mobil uygulama arka ucunuzu tanılama etkinleştirmeniz gerekir.

Tanılamayı etkinleştirin ve günlüklere yazılır için:

1. Adımları [tanılama etkinleştirme](../app-service/web-sites-enable-diagnostic-log.md#enablediag).
2. Aşağıdaki kod dosyanızda deyimiyle:

        using System.Web.Http.Tracing;
3. .NET arka ucundan için tanılama günlükleri, aşağıdaki gibi yazmak için bir izleme yazıcısı oluşturun:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. Sunucu projenizi yeniden yayımlamanız ve günlük ile kod yolu yürütmek için mobil uygulama arka ucu erişebilirsiniz.
5. Karşıdan yükle ve günlükleri açıklandığı gibi değerlendirme [nasıl yapılır: günlükleri indirmek](../app-service/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Yerel kimlik doğrulaması ile hata ayıklama
Uygulamanızı yerel olarak değişikliklerini buluta yayımlamadan önce test etmek için çalıştırabilirsiniz. Çoğu Azure Mobile Apps arka uçlarını için basın *F5* while Visual Studio'da. Ancak, kimlik doğrulaması kullanılırken ek bazı noktalar vardır.

Bir bulut tabanlı mobil uygulama ile App Service kimlik doğrulama/yapılandırılmış yetkilendirme olmalıdır ve istemci alternatif oturum açma ana bilgisayar belirtilen bulut uç noktası olmalıdır. İstemci platformunuz için gerekli adımlarla belgelerine bakın.

Mobil arka sahip olduğundan emin olun [Microsoft.Azure.Mobile.Server.Authentication] yüklü. Ardından, sonra aşağıdaki uygulamanızın OWIN başlangıç sınıfı ekleyin `MobileAppConfiguration` uygulandıktan, `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

Önceki örnekte, yapılandırmanız *authAudience* ve *authIssuer* uygulama Web.config içinde dosya ayarları için her HTTPS şeması kullanarak, uygulamanızın kök URL'si olmalıdır. Benzer şekilde ayarlamalısınız *authSigningKey* uygulamanızı değerini anahtar imzalama kullanıcının olması.
İmzalama anahtarı edinmek için:

1. Uygulamanızda gidin [Azure portal]
2. Tıklatın **Araçları**, **Kudu**, **Git**.
3. Kudu Yönetim sitesini'ı tıklatın **ortam**.
4. Değeri Bul *Web sitesi\_AUTH\_imzalama\_anahtar*.

İmzalama anahtarı kullanmak *authSigningKey* , yerel uygulama yapılandırma parametresi.  Mobil arka istemci bulut tabanlı uç noktasından belirteç alan yerel olarak çalıştırırken belirteçleri doğrulamak için şimdi bulunur.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure portal]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx
