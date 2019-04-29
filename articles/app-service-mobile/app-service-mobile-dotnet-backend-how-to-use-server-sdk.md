---
title: Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma | Microsoft Docs
description: Azure App Service Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışmayı öğrenin.
keywords: App service, azure app service, mobil uygulama, mobil hizmet, Ölçek, ölçeklenebilir, uygulama dağıtımı, azure uygulama dağıtımı
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
ms.openlocfilehash: 195a2dd88f443120f337ba441358389f0dc290f8
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119531"
---
# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Azure Mobile Apps için .NET arka uç sunucu SDK’sı ile çalışma
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Bu konuda, Azure App Service Mobile Apps senaryoları .NET arka uç sunucu SDK'sı kullanma gösterilmektedir. Azure Mobile Apps SDK'sını ASP.NET uygulamanıza mobil istemciler çalışmanıza yardımcı olur.

> [!TIP]
> [.NET sunucu SDK'sı için Azure Mobile Apps] [ 2] github'da açık kaynaktır. Tüm sunucu SDK'sı birim test paketi ve bazı örnek projeleri dahil olmak üzere tüm kaynak kodu deposu içerir.
>
>

## <a name="reference-documentation"></a>Başvuru belgeleri
Sunucu SDK'sı için başvuru belgeleri şuradan ulaşabilirsiniz: [Azure Mobile Apps .NET başvurusu][1].

## <a name="create-app"></a>Nasıl Yapılır: .NET Mobil uygulama arka ucu oluşturma
Yeni bir proje başlatıyorsanız herhangi birini kullanarak bir App Service uygulama oluşturabileceğiniz [Azure portal] veya Visual Studio. App Service uygulamayı yerel olarak çalıştırmak veya bulut tabanlı App Service mobil uygulamanıza projesini yayımlayın.

Mevcut bir projeyi mobil özellikleri ekliyorsanız, bkz. [indirin ve SDK'sını başlatmak](#install-sdk) bölümü.

### <a name="create-a-net-backend-using-the-azure-portal"></a>Azure portalını kullanarak bir .NET arka ucu oluşturma
Bir App Service mobil arka ucu oluşturmak için aşağıdakilerden birini izleyin [hızlı başlangıç öğreticisinde] [ 3] ya da şu adımları izleyin:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Geri *başlama* dikey altında **tablo API'si Oluştur**, seçin **C#** olarak, **arka uç dili**. Tıklayın **indirme**, sıkıştırılmış proje dosyalarını yerel bilgisayarınıza ayıklayın ve Visual Studio içinde çözümü açın.

### <a name="create-a-net-backend-using-visual-studio-2017"></a>Visual Studio 2017'yi kullanarak bir .NET arka ucu oluşturma

Azure Mobile Apps projeye Visual Studio'dan yayımlamak için Visual Studio yükleyicisi aracılığıyla Azure iş yükünü yükleyin. SDK'yı yükledikten sonra aşağıdaki adımları kullanarak bir ASP.NET uygulaması oluşturun:

1. Açık **yeni proje** iletişim (gelen **dosya** > **yeni** > **proje...** ).
2. Genişletin **Visual C#** seçip **Web**.
3. Seçin **ASP.NET Web uygulaması (.NET Framework)**.
4. Proje adını girin. Daha sonra, **Tamam**'a tıklayın.
5. Seçin **Azure mobil uygulaması** şablonları listesinden.
6. Tıklayın **Tamam** çözümü oluşturmak için.
7. Projeye sağ tıklayarak **Çözüm Gezgini** ve **Yayımla...** , ardından **App Service** yayımlama hedefi olarak.
8. Yeni veya mevcut Azure uygulama yayımlamak için hizmet seçin ve kimlik doğrulamasını yapmak için yönergeleri izleyin.

### <a name="create-a-net-backend-using-visual-studio-2015"></a>Visual Studio 2015'i kullanarak bir .NET arka ucu oluşturma

Yükleme [.NET için Azure SDK'sı] [ 4] (sürüm 2.9.0'da veya üzeri) Visual Studio'da bir Azure Mobile Apps proje oluşturmaktır. SDK'yı yükledikten sonra aşağıdaki adımları kullanarak bir ASP.NET uygulaması oluşturun:

1. Açık **yeni proje** iletişim (gelen **dosya** > **yeni** > **proje...** ).
2. Genişletin **şablonları** > **Visual C#** seçip **Web**.
3. **ASP.NET Web Uygulaması**'nı seçin.
4. Proje adını girin. Daha sonra, **Tamam**'a tıklayın.
5. Altında *ASP.NET 4.5.2 şablonları*seçin **Azure mobil uygulaması**. Denetleme **bulutta Barındır** bu proje yayımlama bulutta bir mobil arka ucu oluşturmak için.
6. **Tamam** düğmesine tıklayın.

## <a name="install-sdk"></a>Nasıl Yapılır: İndirin ve SDK'sını başlatmak
SDK'sı kullanılabilir [NuGet.org]. Bu paket, SDK'sı ile çalışmaya başlamak için gerekli temel işlevselliğini içerir. SDK'sını başlatmak için bunlar üzerinde eylem gerçekleştirebileceğini gerekir **HttpConfiguration** nesne.

### <a name="install-the-sdk"></a>SDK yükle
SDK yüklemek için sunucu projesi Visual Studio'da seçim sağ **NuGet paketlerini Yönet**, arama [Microsoft.Azure.Mobile.Server] paketini'a tıklayın **yükleyin** .

### <a name="server-project-setup"></a> Sunucu projesi başlatın
Bir .NET arka uç sunucu projesi diğer ASP.NET projeleri için benzer bir OWIN başlangıç sınıfı ekleyerek başlatılır. NuGet paketini başvurulan olun `Microsoft.Owin.Host.SystemWeb`. Visual Studio'da bu sınıf eklemek için sunucu projenizde sağ tıklayıp **Ekle** >
**yeni öğe**, ardından **Web**  >  ** Genel** > **OWIN başlangıç sınıfı**.  Bir sınıf, aşağıdaki öznitelik ile oluşturulur:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

İçinde `Configuration()` yöntemi OWIN başlangıç sınıfınızın, kullanım bir **HttpConfiguration** Azure Mobile Apps ortamı yapılandırmak için nesne.
Aşağıdaki örnek, hiçbir ek özelliklerle sunucu projesi başlatır:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Tek tek özellikleri etkinleştirmek için genişletme yöntemleri üzerinde çağırmalıdır **MobileAppConfiguration** çağırmadan önce nesne **ApplyTo**. Örneğin, aşağıdaki kod özniteliğine sahip tüm APİ'si denetleyicilerinin varsayılan yollar ekler `[MobileAppController]` başlatma sırasında:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

Sunucu hızlı başlangıç Azure portal çağrılarının **UseDefaultConfiguration()**. Aşağıdaki Kurulum bu eşdeğerdir:

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

* `AddMobileAppHomeController()` Varsayılan Azure Mobile Apps giriş sayfasını sağlar.
* `MapApiControllers()` Webapı denetleyicisi ile donatılmış özel API özellikleri sağlanır `[MobileAppController]` özniteliği.
* `AddTables()` ilişkin bir eşleme sağlar `/tables` tablo denetleyicilerine uç noktaları.
* `AddTablesWithEntityFramework()` bir kısayol eşlemesi için olan `/tables` Entity Framework kullanan uç noktaları temel denetleyicileri.
* `AddPushNotifications()` Bildirim hub'ları için cihazları kaydetme çok basit bir yöntemini sağlar.
* `MapLegacyCrossDomainController()` yerel geliştirme için standart CORS üst bilgileri sağlar.

### <a name="sdk-extensions"></a>SDK uzantıları
Aşağıdaki NuGet tabanlı uzantı paketleri, uygulamanız tarafından kullanılan çeşitli mobil özellikler sağlar. Uzantıları kullanarak başlatma sırasında etkinleştirmek **MobileAppConfiguration** nesne.

* [Microsoft.Azure.Mobile.Server.Quickstart] temel Mobile Apps Kurulum destekler. Çağırarak yapılandırmaya eklenmiş **UseDefaultConfiguration** başlatma sırasında genişletme yöntemi. Bu uzantı, aşağıdaki uzantılar içerir: Bildirimler, kimlik doğrulaması, varlık, tablolar, etki alanları arası ve giriş paketleri. Bu paket, Mobile Apps hızlı başlangıç Azure portalında kullanılabilir tarafından kullanılır.
* [Microsoft.Azure.Mobile.Server.Home](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) varsayılan uygulayan *bu mobil uygulamayı çalışır duruma sayfa* web sitesi kök. Çağırarak yapılandırmaya ekleyin **AddMobileAppHomeController** genişletme yöntemi.
* [Microsoft.Azure.Mobile.Server.Tables](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) verilerle çalışmak için sınıflar içerir ve veri işlem hattı ayarlar artırma. Çağırarak yapılandırmaya ekleyin **AddTables** genişletme yöntemi.
* [Microsoft.Azure.Mobile.Server.Entity](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) Entity Framework SQL veritabanındaki verilere erişim sağlar. Çağırarak yapılandırmaya ekleyin **AddTablesWithEntityFramework** genişletme yöntemi.
* [Microsoft.Azure.Mobile.Server.Authentication] kimlik doğrulaması sağlar ve ayarlar yukarı belirteçleri doğrulamak için kullanılan OWIN ara yazılımı. Çağırarak yapılandırmaya ekleyin **AddAppServiceAuthentication** ve **Iappbuilder**. **UseAppServiceAuthentication** genişletme yöntemleri.
* [Microsoft.Azure.Mobile.Server.Notifications] anında iletme bildirimleri ve tanımlar bir anında iletme kayıt uç noktası sağlar. Çağırarak yapılandırmaya ekleyin **AddPushNotifications** genişletme yöntemi.
* [Microsoft.Azure.Mobile.Server.CrossDomain](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) Creates a controller that serves data to legacy web browsers from your Mobile App. Çağırarak yapılandırmaya ekleyin **MapLegacyCrossDomainController** genişletme yöntemi.
* [Microsoft.Azure.Mobile.Server.Login] özel kimlik doğrulama senaryoları sırasında kullanılan statik bir yöntemi olan AppServiceLoginHandler.CreateToken() yöntemi sağlar.

## <a name="publish-server-project"></a>Nasıl Yapılır: Sunucu projesini yayımlayın
Bu bölümde, .NET arka uç projeniz Visual Studio'dan yayımlama işlemini göstermektedir. Arka uç kullanarak projenize dağıtabilirsiniz [Git](../app-service/deploy-local-git.md) veya diğer yöntemler kullanılabilir vardır.

1. Visual Studio'da NuGet paketlerini geri yüklemek için projeyi yeniden derleyin.
2. Çözüm Gezgini'nde projeye sağ tıklayın, **Yayımla**. İlk kez yayımladığınızda, yayımlama profilini tanımlamak gerekir. Tanımlı bir profil zaten varsa, onu seçin ve tıklayın **Yayımla**.
3. Yayımlama hedefi seçmek için isteyip istemediğiniz sorulursa tıklayın **Microsoft Azure App Service** > **sonraki**, sonra (gerekirse), Azure kimlik bilgileri ile oturum açın.
   Visual Studio indirmeleri ve güvenli bir şekilde depolar, ayarlarınızı doğrudan azure'dan yayımlayın.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. Seçin, **abonelik**seçin **kaynak türü** gelen **görünümü**, genişletme **mobil uygulama**ve mobil uygulama arka ucunuzu'ı tıklatın **Tamam**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. Yayımlama profili bilgilerini doğrulayın ve tıklayın **Yayımla**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Mobil uygulama arka ucunuzu yayımlandı başarıyı gösteren bir giriş sayfasını görürsünüz.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <a name="define-table-controller"></a> Nasıl Yapılır: Bir tablo denetleyicisi tanımlayın
Mobil istemciler için bir SQL tablosunu kullanıma sunmak için bir tablo denetleyicisi tanımlayın.  Bir tablo denetleyicisi yapılandırma üç adımı gerektirir:

1. Veri aktarımı nesnesi (DTO) bir sınıf oluşturun.
2. Bir tablo başvurusu mobil DbContext sınıfı yapılandırın.
3. Bir tablo denetleyicisi oluşturun.

Bir veri aktarım nesnesini (DTO) öğesinden devralınan bir C# nesnesi olduğu `EntityData`.  Örneğin:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

DTO, SQL veritabanı içinde tablo tanımlamak için kullanılır.  Veritabanı girişi oluşturmak için bir `DbSet<>` kullanmakta olduğunuz DbContext özelliği.  Azure Mobile Apps varsayılan proje şablonunda DbContext çağrılır `Models\MobileServiceContext.cs`:

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

Azure SDK'ın yüklü varsa, artık bir şablon tablo denetleyicisi gibi oluşturabilirsiniz:

1. Denetleyicileri klasörü sağ tıklatın ve seçin **Ekle** > **denetleyicisi...** .
2. Seçin **Azure Mobile Apps tablo denetleyicisi** seçeneğini belirleyin, ardından tıklayın **Ekle**.
3. İçinde **denetleyici Ekle** iletişim:
   * İçinde **Model sınıfı** açılır listesinde, yeni bir DTO seçin.
   * İçinde **DbContext** açılır listesinde, mobil hizmet DbContext sınıfını seçin.
   * Denetleyici adı sizin için oluşturulur.
4. **Ekle**'ye tıklayın.

Bir örnek için basit bir hızlı başlangıç sunucu projesi içeren **TodoItemController**.

### <a name="adjust-pagesize"></a>Nasıl Yapılır: Tablo disk belleği boyutunu ayarlama
Varsayılan olarak, Azure Mobile Apps istek başına 50 kayıtları döndürür.  İstemci, UI iş parçacığı veya çok uzun, sunucunun yukarı iyi bir kullanıcı deneyimi sağlamaya tie değil, disk belleği sağlar. Tablo disk belleği boyutunu değiştirmek için "izin verilen sorgu boyutu" sunucu tarafı artırmak ve istemci tarafı sayfa boyutu, sunucu tarafı "izin verilen sorgu boyutu" olarak ayarlandı kullanarak `EnableQuery` özniteliği:

    [EnableQuery(PageSize = 500)]

PageSize aynı olduğundan emin olun veya istemci tarafından istenen boyuttan daha büyük.  Belirli istemci istemci sayfa boyutunu değiştirme hakkında bilgiler için nasıl yapılır belgelerini inceleyin.

## <a name="how-to-define-a-custom-api-controller"></a>Nasıl yapılır: Özel bir API denetleyicisi tanımlayın
Özel API denetleyicisi, bir uç nokta göstererek, mobil uygulama arka ucu için en temel işlevlerini sağlar. [MobileAppController] özniteliğini kullanarak bir mobile özgü API denetleyicisi kaydedebilirsiniz. `MobileAppController` Özniteliği bir rota kaydeder, Mobile Apps JSON serileştirici ayarlar ve açar [istemci sürüm denetimi](app-service-mobile-client-and-server-versioning.md).

1. Visual Studio'da denetleyicileri klasörü sağ tıklatın ve ardından **Ekle** > **denetleyicisi**seçin **Web API 2 denetleyicisi&mdash;boş** ve tıklayın **Ekle**.
2. Tedarik bir **Denetleyici adı**, gibi `CustomController`, tıklatıp **Ekle**.
3. Yeni denetleyici sınıfı dosyasına aşağıdakileri ekleyin using deyimi:

        using Microsoft.Azure.Mobile.Server.Config;
4. Uygulama **[MobileAppController]** özniteliği aşağıdaki örnekte olduğu gibi bir API denetleyicisi sınıfı tanımı için:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. App_Start/Startup.MobileApp.cs dosyasında bir çağrı ekleyin **MapApiControllers** aşağıdaki örnekteki gibi bir genişletme yöntemi:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Ayrıca `UseDefaultConfiguration()` genişletme yöntemi yerine `MapApiControllers()`. Sahip olmayan herhangi bir denetleyicisi **MobileAppControllerAttribute** uygulanan hala istemcileri tarafından erişilebilen, ancak bunu doğru şekilde herhangi bir mobil uygulama istemci SDK'sını kullanan istemciler tarafından tüketilebilir değil.

## <a name="how-to-work-with-authentication"></a>Nasıl yapılır: Kimlik doğrulaması ile çalışma
Azure Mobile Apps kullanan App Service kimlik doğrulaması / yetkilendirme mobil arka ucunuzdaki güvenliğini sağlamak için.  Bu bölümde, .NET arka uç sunucu projenizi kimlik doğrulamayla ilgili aşağıdaki görevleri gerçekleştirmek nasıl gösterir:

* [Nasıl yapılır: Kimlik doğrulaması için bir sunucu projesi ekleme](#add-auth)
* [Nasıl yapılır: Uygulamanız için özel kimlik doğrulaması kullan](#custom-auth)
* [Nasıl yapılır: Kimliği doğrulanmış kullanıcı bilgilerini alma](#user-info)
* [Nasıl yapılır: Yetkili kullanıcıların veri erişimini kısıtlamak](#authorize)

### <a name="add-auth"></a>Nasıl Yapılır: Kimlik doğrulaması için bir sunucu projesi ekleme
Kimlik doğrulama sunucu projenizi genişleterek ekleyebileceğiniz **MobileAppConfiguration** nesne ve OWIN ara yazılımını yapılandırma. Yüklediğinizde [Microsoft.Azure.Mobile.Server.Quickstart] paket ve çağrı **UseDefaultConfiguration** genişletme yöntemi, 3. adımına atlayabilirsiniz.

1. Visual Studio'da yükleme [Microsoft.Azure.Mobile.Server.Authentication] paket.
2. Startup.cs proje dosyasında, aşağıdaki kod satırının başında ekleme **yapılandırma** yöntemi:

        app.UseAppServiceAuthentication(config);

    Bu OWIN ara yazılım bileşeni, ilişkili App Service ağ geçidi tarafından verilen belirteçlere doğrular.
3. Ekleme `[Authorize]` öznitelik herhangi bir denetleyici veya kimlik doğrulaması gerektiren yöntemi.

Mobile Apps arka ucunuzu istemcilerin kimliğini doğrulamak hakkında bilgi edinmek için bkz: [uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Nasıl Yapılır: Uygulamanız için özel kimlik doğrulaması kullan
> [!IMPORTANT]
> Özel kimlik doğrulamasını etkinleştirmek için önce Azure portalında uygulama hizmetiniz için bir sağlayıcı seçmeden App Service kimlik doğrulaması etkinleştirmeniz gerekir. Bu, barındırıldığında WEBSITE_AUTH_SIGNING_KEY ortam değişkeni olanak sağlar.
> 
> 
> App Service kimlik doğrulama/yetkilendirme sağlayıcılardan birini kullanmak istemiyorsanız, kendi oturum açma sistemi uygulayabilirsiniz. Yükleme [Microsoft.Azure.Mobile.Server.Login] ile kimlik doğrulaması belirteci oluşturma yardımcı olmak üzere paket.  Kullanıcı kimlik bilgilerini doğrulamak için kendi kodunuzu sağlar. Örneğin, bir veritabanında salted ve karma parolaları karşı denetleyebilir. Aşağıdaki örnekte `isValidAssertion()` yöntemi (başka yerde tanımlanmış), bu denetimler için sorumludur.

Özel kimlik doğrulama bir ApiController oluşturma ve gösterme kullanıma sunulduğunu `register` ve `login` eylemler. İstemcinin kullanıcıdan bilgi toplamak için özel bir kullanıcı Arabirimi kullanmanız gerekir.  Bilgileri standart bir HTTP POST çağrısına API'SİYLE sonra gönderilir. Onaylama işlemi sunucuyu doğrular, sonra bir belirteç kullanarak verilir `AppServiceLoginHandler.CreateToken()` yöntemi.  ApiController **barındırmamalıdır** kullanın `[MobileAppController]` özniteliği.

Bir örnek `login` eylem:

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

Önceki örnekte LoginResult ve LoginResultUser gerekli özellikleri kullanıma sunan seri hale getirilebilir nesneleridir. İstemci, oturum açma yanıt biçiminde JSON nesnesi döndürülecek bekliyor:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

`AppServiceLoginHandler.CreateToken()` Yöntemi içeren bir *İzleyici* ve *veren* parametresi. Bu parametrelerin her ikisi de HTTPS düzenini kullanarak URL'ye, uygulamanızın kök ayarlanır. Benzer şekilde ayarlamalısınız *secretKey* projenin imzalama anahtar değeri, uygulamanızın olması. Anahtarları Naneli ve kullanıcıların kimliğine bürünmek için kullanılabilir olarak imzalama anahtarı bir istemcide dağıtmayın. İmzalama anahtarı başvurarak, App Service'te barındırılan sırada edinebilirsiniz *Web sitesi\_AUTH\_imzalama\_anahtar* ortam değişkeni. Yerel hata ayıklama bağlamda gerekirse bölümündeki yönergeleri [kimlik doğrulaması ile yerel hata ayıklama](#local-debug) bölüm anahtarı almak ve bir uygulama ayarı olarak depolamak için.

Verilen belirteç, diğer talepler ve sona erme tarihini de içerebilir.  En düşük düzeyde, verilen belirtecin bir konu içermelidir (**alt**) talep.

Standart bir istemci destekleyebilir `loginAsync()` kimlik doğrulaması rota aşırı yükleme yöntemi.  İstemci çağırırsa `client.loginAsync('custom');` oturum açmak için rotanız olmalıdır `/.auth/login/custom`.  Özel kimlik doğrulama kullanarak denetleyici için rota ayarlayabilirsiniz `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> Kullanarak `loginAsync()` yaklaşımı, her sonraki çağrı hizmete kimlik doğrulaması belirteci iliştirilmiş sağlar.
>
>

### <a name="user-info"></a>Nasıl Yapılır: Kimliği doğrulanmış kullanıcı bilgilerini alma
App Service tarafından bir kullanıcının kimliği doğrulandığında, .NET arka uç kodunuzun atanan kullanıcı kimliği ve diğer bilgilere erişebilirsiniz. Kullanıcı bilgilerini, arka uçtaki yetkilendirme kararları için kullanılabilir. Aşağıdaki kod, bir istekle ilişkili kullanıcı kimliği alır:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

SID, sağlayıcıya özgü kullanıcı ID'den türetilmiş ve belirli kullanıcı ve oturum açma sağlayıcısı için statiktir.  SID için geçersiz kimlik doğrulama belirteci null olur.

App Service Ayrıca, oturum açma sağlayıcısından belirli talep sağlar. Her bir kimlik sağlayıcısı kimlik sağlayıcısına SDK'sını kullanarak daha fazla bilgi sağlayabilir.  Örneğin, Facebook Graph API'si için arkadaş bilgilerini kullanabilirsiniz.  Azure portalında sağlayıcısı dikey penceresinde, istenen talep belirtebilirsiniz. Bazı talepler, kimlik sağlayıcısı ile ek yapılandırma gerektirir.

Aşağıdaki kod çağrıları **GetAppServiceIdentityAsync** erişim içeren oturum açma kimlik bilgileri, belirteci almak için genişletme yöntemi gerekli Facebook Graph API'si karşı isteğinde bulunmak:

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

Kullanarak bir ekleme deyimini `System.Security.Principal` sağlamak **GetAppServiceIdentityAsync** genişletme yöntemi.

### <a name="authorize"></a>Nasıl Yapılır: Yetkili kullanıcıların veri erişimini kısıtlamak
Önceki bölümde, biz kimliği doğrulanmış bir kullanıcının kullanıcı kimliği almak nasıl gösterilmiştir. Veri ve bu değere göre diğer kaynaklara erişimi kısıtlayabilirsiniz. Örneğin, bir kullanıcı kimliği sütunu tablolarına ekleme ve sorgu sonuçlarını kullanıcı Kimliğine göre filtreleme yalnızca yetkili kullanıcılar için döndürülen verileri sınırlamak için bir basit yoludur. Aşağıdaki kod, yalnızca SID değeri Todoıtem tablosundaki UserID sütununa eşleştiğinde veri satırları döndürür:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

`Query()` Yöntemi döndürür bir `IQueryable` filtreleme işlemek için LINQ tarafından yönetilebilir.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Nasıl yapılır: Bir sunucu projesi için anında iletme bildirimleri ekleme
Anında iletme bildirimleri sunucu projenizi genişleterek ekleme **MobileAppConfiguration** nesne ve Notification Hubs istemcisi oluşturma.

1. Visual Studio'da sunucu projeye sağ tıklayın ve **NuGet paketlerini Yönet**, arama `Microsoft.Azure.Mobile.Server.Notifications`, ardından **yükleme**.
2. Yüklemek için bu adımı yineleyin `Microsoft.Azure.NotificationHubs` paketini Notification hubs'ı istemci kitaplığı içerir.
3. App_Start/Startup.MobileApp.cs içinde ve bir çağrı ekleyin **AddPushNotifications()** başlatma sırasında genişletme yöntemi:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. Notification hubs'ı istemci oluşturan aşağıdaki kodu ekleyin:

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

Kayıtlı cihazlara anında iletme bildirimleri göndermek için Notification hubs'ı istemci artık kullanabilirsiniz. Daha fazla bilgi için [uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-ios-get-started-push.md). Notification Hubs hakkında daha fazla bilgi için bkz. [Notification Hubs'a genel bakış](../notification-hubs/notification-hubs-push-notification-overview.md).

## <a name="tags"></a>Nasıl Yapılır: Etiketleri kullanarak hedefe yönelik anında iletmeleri etkinleştir
Bildirim hub'ları, etiketleri kullanarak belirli kayıtları için hedeflenmiş bildirimler gönderin olanak sağlar. Birkaç etiketi otomatik olarak oluşturulur:

* Belirli bir cihaz için yükleme kimliği tanımlar.
* Belirli bir kullanıcı kimliği doğrulanmış SID'lerine bağlı kullanıcı kimliği tanımlar.

Kimliği erişilebilir yükleme **InstallationID** özelliği **MobileServiceClient**.  Aşağıdaki örnek, Notification hubs'ı belirli bir kurulumda bir etiket eklemek için bir yükleme kimliği kullanmayı gösterir:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Anında iletme bildirimi kaydı sırasında istemci tarafından sağlanan herhangi bir etiket arka uç tarafından yükleme oluştururken göz ardı edilir. Yükleme etiket eklemek bir istemci etkinleştirmek için yukarıdaki desen kullanarak etiketleri ekleyen özel API oluşturmanız gerekir.

Bkz: [istemci eklendiğinde anında iletme bildirimi etiketleri] [ 5] App Service Mobile Apps tamamlanmış hızlı başlangıç örnek olarak.

## <a name="push-user"></a>Nasıl Yapılır: Kimliği doğrulanmış bir kullanıcıya anında iletme bildirimleri gönderme
Kimliği doğrulanmış bir kullanıcı için anında iletme bildirimleri kaydettiğinde, bir kullanıcı kimliği etiketi kayıt için otomatik olarak eklenir. Bu etiket kullanarak, bu kişi tarafından kaydedilen tüm cihazlara anında iletme bildirimleri gönderebilirsiniz. Aşağıdaki kod, isteği yapan kullanıcı SID'si alır ve şablon anında iletme bildirimi için her bir cihaz kaydı için söz konusu kişinin gönderir:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Kimliği doğrulanmış bir istemci anında iletme bildirimleri için kaydı sırasında kayıt denemeden önce söz konusu kimlik doğrulamasını tam olduğundan emin olun. Daha fazla bilgi için [kullanıcılara anında iletme] [ 6] .NET arka ucu için App Service Mobile Apps tamamlanmış hızlı başlangıç örnek içinde.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Nasıl yapılır: Hata ayıklama ve sorun giderme .NET sunucu SDK'sı
Azure App Service çeşitli hata ayıklama ve sorun giderme teknikleri ASP.NET uygulamaları için sağlar:

* [Bir Azure App Service'ı izleme](../app-service/web-sites-monitor.md)
* [Azure App Service'te tanılama günlüğünü etkinleştirme](../app-service/troubleshoot-diagnostic-logs.md)
* [Visual Studio'da Azure App Service'e giderme](../app-service/troubleshoot-dotnet-visual-studio.md)

### <a name="logging"></a>Günlüğe kaydetme
App Service tanılama günlükleri için standart ASP.NET izleme yazılmasını kullanarak yazabilirsiniz. Tanılama günlüklerine yazabilmesi için önce Mobile App arka ucunuzu etkinleştirmeniz gerekir.

Tanılamayı etkinleştirerek ve günlüklere yazılır için:

1. Bağlantısındaki [tanılamayı etkinleştirme](../app-service/troubleshoot-diagnostic-logs.md#enablediag).
2. Aşağıdaki kod dosyanıza using deyimi:

        using System.Web.Http.Tracing;
3. .NET arka ucundan tanılama günlükleri gibi yazmak için izleme yazıcısı oluşturun:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. Sunucu projenizi yeniden yayımlamanız ve günlük ile kod yolu yürütmek için mobil uygulama arka ucu erişebilirsiniz.
5. İndirin ve günlükleri açıklandığı gibi değerlendir [nasıl yapılır: Günlükleri indirme](../app-service/troubleshoot-diagnostic-logs.md#download).

### <a name="local-debug"></a>Yerel kimlik doğrulaması ile hata ayıklama
Uygulamanızı buluta yayımlamadan önce değişiklikleri yerel olarak test çalıştırabilirsiniz. Çoğu Azure Mobile Apps arka uçları için basın *F5* çalışırken Visual Studio'da. Ancak, kimlik doğrulaması kullanırken bazı ek hususlar vardır.

Bir bulut tabanlı mobil uygulaması ile App Service kimlik doğrulaması/yapılandırılmış yetkilendirme olması ve istemci alternatif bir oturum açma konağı olarak belirttiğiniz bulut uç noktasına sahip olması gerekir. İstemci platformunuzu gereken belirli adımlar için belgelerine bakın.

Mobil arka ucunuzdaki olduğundan emin olun [Microsoft.Azure.Mobile.Server.Authentication] yüklü. Ardından sonra aşağıdaki komutu uygulamanızın OWIN başlangıç sınıfı ekleyin `MobileAppConfiguration` uygulandıktan, `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

Önceki örnekte, yapılandırmanız gereken *authAudience* ve *authIssuer* , Web.config uygulama ayarları dosyası her olması, uygulamanızın kök URL'sini HTTPS düzenini kullanarak. Benzer şekilde ayarlamalısınız *authSigningKey* projenin imzalama anahtar değeri, uygulamanızın olması.
İmzalama anahtarı edinmek için:

1. Uygulamanızda gidin [Azure portal]
2. Tıklayın **Araçları**, **Kudu**, **Git**.
3. Kudu yönetim sitede tıklayın **ortam**.
4. Değeri Bul *Web sitesi\_AUTH\_imzalama\_anahtar*.

İmzalama anahtarı kullanmak *authSigningKey* , yerel uygulama yapılandırma parametresi.  Mobil arka ucunuzdaki istemci bulut tabanlı uç noktasından belirteç alır. yerel olarak çalıştırılırken belirteçleri doğrulamak için artık bulunur.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure portal]: https://portal.azure.com
[NuGet.org]: https://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx
