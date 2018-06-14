---
title: Azure uygulama hizmeti için Mobile Services'den yükseltme
description: Bir mobil uygulama hizmeti Mobile Services uygulamanıza kolayca yükseltmeyi öğrenin
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 9c0ac353-afb6-462b-ab94-d91b8247322f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 05041637aa5cbb044e6731208825f75edec83352
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32157053"
---
# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Var olan .NET Azure mobil hizmetinizi App Service'e yükseltme
App Service Mobile, Microsoft Azure kullanarak mobil uygulamaları oluşturmak için yeni bir yoludur. Daha fazla bilgi için bkz: [Mobile Apps nedir?].

Bu konuda, var olan .NET arka uç uygulama için yeni bir App Service Mobile Apps Azure Mobile Services yükseltme açıklar. Bu yükseltme gerçekleştirirken mevcut Mobile Services uygulamanız çalışmaya devam edebilirsiniz.   Bir Node.js arka uç uygulaması yükseltme yapmanız oluştuysa, [Node.js Mobile Services yükseltme](app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Azure App Service'e bir mobil arka uç yükseltildiğinde, tüm uygulama hizmeti özelliklerine erişebilir ve göre faturalandırılır [uygulama hizmeti fiyatlandırma], fiyatlandırma değil Mobile Services.

## <a name="migrate-vs-upgrade"></a>Yükseltme geçirme
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Önerilir Bu, [bir geçiş gerçekleştirmek](app-service-mobile-migrating-from-mobile-services.md) yükseltme geçmeden önce. Bu şekilde, aynı uygulama hizmeti planı üzerinde her iki sürümü, uygulamanızın koyun ve ek bir maliyet doğurur.
>
>

### <a name="improvements-in-mobile-apps-net-server-sdk"></a>Mobile Apps .NET sunucusu SDK yenilikleri
Yeni yükseltme [Mobile Apps SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) aşağıdaki avantajları sağlar:

* NuGet bağımlılıkları hakkında daha fazla esneklik sağlar. Artık barındırma ortamı NuGet paketleri, kendi sürümlerini sağlar, alternatif uyumlu sürümlerini kullanabilirsiniz. Ancak, yeni kritik bugfixes veya güvenlik güncelleştirmeleri mobil Server SDK veya bağımlılıkları varsa, hizmetinizi el ile güncelleştirmeniz gerekir.
* Daha fazla esneklik mobil SDK. Yollar, kimlik doğrulama, tablo API'leri ve anında iletme kayıt uç noktasını gibi ayarlanır ve hangi özelliklerin açıkça denetleyebilirsiniz. Daha fazla bilgi için bkz: [Azure mobil uygulamalar için .NET sunucusu SDK kullanmayı](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).
* Diğer ASP.NET proje türleri ve yollar için destek. Şimdi, MVC ve Web API denetleyicilerinin mobil arka uç projeniz aynı projede barındırabilir.
* Web ve mobil uygulamalar arasında ortak bir kimlik doğrulama yapılandırması kullanmanıza olanak sağlayan yeni uygulama hizmeti kimlik doğrulama özellikleri için destek.

## <a name="overview"></a>Temel yükseltmeye genel bakış
Çoğu durumda, yükseltme yeni mobil uygulamalar sunucusu SDK değiştirme ve kodunuzu yeni bir mobil uygulama örneği üzerine yeniden yayımlanması olarak basit olacaktır. Vardır, ancak Gelişmiş kimlik doğrulama senaryoları ve çalışmak gibi bazı ek yapılandırma gerektiren bazı senaryolar zamanlanmış iş. Bunların her biri, sonraki bölümlerde ele alınmıştır.

> [!TIP]
> Okuma ve bu konunun geri kalanında tamamen bir yükseltme işlemine başlamadan önce anlamanız önerilir. Not alın, aşağıda belirtilir tüm özelliklerini kullanabilirsiniz.
>
>

Mobile Services istemci SDK'ları olan **değil** yeni mobil uygulamalar sunucusu SDK ile uyumlu. Uygulamanız için hizmetin sürekliliği sağlamak için değişiklikleri şu anda yayımlanan istemciler hizmet veren bir site yayınlamalıdır değil. Bunun yerine, yinelenen hizmet veren yeni bir mobil uygulama oluşturmanız gerekir. Bu uygulamayı ek finansal ücret oluşmasını önlemek için aynı uygulama hizmeti plan üzerinde koyabilirsiniz.

Uygulamanın iki sürümlerini sonra gerekir: aynı kalır ve hizmet veren bir yayımlanan uygulamalar joker diğer, ardından yükseltin ve yeni bir istemci sürümü ile hedef. Taşıma ve kodunuzu test etmek, hızı, ancak yaptığınız tüm hata düzeltmeleri hem de uygulandığından emin olmanız gerekir. İstediğiniz istemci uygulamaları yazılımların en son sürüme güncelleştirilmemiş istenen sayısını, özgün geçirilen uygulama silebilirsiniz olduğunu düşündüğünüz sonra.

Yükseltme işlemi için tam anahattı aşağıdaki gibidir:

1. Yeni bir mobil uygulaması oluşturma
2. Yeni Sunucu SDK'ları kullanmak için projesini güncelleştirme
3. İstemci uygulamanızı yeni bir sürümü kullanıma
4. (İsteğe bağlı) Özgün geçirilen örneğinizi Sil

## <a name="mobile-app-version"></a>İkinci bir uygulama örneği oluşturma
Yükseltme ilk adımı, uygulamanın yeni sürümü barındıracak mobil uygulama kaynağı oluşturmaktır. Varolan bir mobil hizmet zaten geçiş yaptıysanız, bu sürüm üzerinde aynı barındırma planı oluşturmak isteyeceksiniz. Açık [Azure portal] ve geçirilen uygulamanıza gidin. Uygulama hizmeti planı üzerinde çalıştırıldığı not edin.

Ardından, ikinci uygulama örneğini izleyerek oluşturun [.NET arka ucu oluşturma yönergeleri](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Seçtiğiniz isteyip istemediğiniz sorulduğunda uygulama hizmeti planı ya da "barındırma planı" geçirilen uygulamanızın planı seçin.

Büyük olasılıkla mobil hizmetlerinde olduğu gibi aynı veritabanını ve bildirim hub'ı kullanmak istersiniz. Bu değerleri açarak kopyalayabilirsiniz [Azure portal] ve özgün uygulamaya gezinme, ardından **ayarları** > **uygulama ayarları**. Altında **bağlantı dizeleri**, kopya `MS_NotificationHubConnectionString` ve `MS_TableConnectionString`. Yeni yükseltme sitenize gidin ve tüm var olan değerlerin üzerine yazar, yapıştırın. Başka bir uygulama ayarları için bu işlemi yineleyin uygulama gereksinimlerinizi. Geçirilen hizmet kullanılmıyorsa, bağlantı dizeleri ve uygulama ayarlarını okuyabilirsiniz **yapılandırma** Mobile Services bölümünde sekmesinde [Klasik Azure portalı].

ASP.NET projesi, uygulamanız için bir kopyasını alın ve yeni sitenizi yayımlayın. Yeni URL ile güncelleştirilmiş istemci uygulamanız bir kopyasını kullanarak, her şeyin beklendiği gibi çalıştığını doğrulayın.

## <a name="updating-the-server-project"></a>Sunucu projesi güncelleştiriliyor
Mobile Apps sağlayan yeni bir [mobil uygulama sunucusu SDK'sı] çok Mobile Services çalışma zamanı ile aynı işlevselliği sağlar. İlk olarak, Mobile Services Paketlerine yönelik tüm başvuruları kaldırmalısınız. NuGet Paket Yöneticisi'nde arayın `WindowsAzure.MobileServices.Backend`. Çoğu uygulamalar dahil olmak üzere çeşitli paketler burada görürsünüz `WindowsAzure.MobileServices.Backend.Tables` ve `WindowsAzure.MobileServices.Backend.Entity`. Böyle bir durumda bağımlılığı ağacı düşük paketinde gibi başlatın `Entity`ve bunu kaldırın. İstendiğinde, tüm bağımlı paketler kaldırmayın. Kaldırdığınız kadar bu işlemi yineleyin `WindowsAzure.MobileServices.Backend` kendisi.

Bu noktada artık Mobile Services SDK'ları başvuruda bulunan bir proje sahip olur.

Sonra başvuruları Mobile Apps SDK'sı ekleyeceksiniz. Bu yükseltme, çoğu Geliştirici indirmek ve yüklemek istediğiniz `Microsoft.Azure.Mobile.Server.Quickstart` paketini olarak bu tüm gerekli kümesinde çeker.

SDK'ları arasındaki farklar kaynaklanan oldukça birkaç derleyici hataları olacaktır, ancak bu adrese kolaydır ve bu bölümün geri kalanında ele alınmıştır.

### <a name="base-configuration"></a>Temel yapılandırma
Ardından, WebApiConfig.cs içinde değiştirebilirsiniz:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

with

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

> [!NOTE]
> Yeni .NET sunucusu SDK hakkında daha fazla bilgi ve özellikleri uygulamanızdan eklemek/kaldırmak nasıl bilgi edinmek istiyorsanız lütfen bkz [.NET sunucusu SDK kullanmayı] konu.
>
>

Uygulamanızı yaparsa kimlik doğrulama özelliklerini kullanmak, aynı zamanda bir OWIN ara yazılımını kaydetmeniz gerekir. Bu durumda, yukarıdaki yapılandırma kodu yeni bir OWIN başlangıç sınıfı taşımanız gerekir.

1. NuGet paketini eklemeniz `Microsoft.Owin.Host.SystemWeb` , zaten projenizde dahil değilse.
2. Visual Studio'da, sağ, proje seçin tıklayın ve **Ekle** -> **yeni öğe**. Seçin **Web** -> **genel** -> **OWIN başlangıç sınıfı**.
3. MobileAppConfiguration için yukarıdaki kod taşıma `WebApiConfig.Register()` için `Configuration()` yeni başlangıç sınıfınızın yöntemi.

Emin olun `Configuration()` yöntemi ile biter:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Tam kimlik doğrulaması bölümünde ele alınan kimlik doğrulaması ile ilgili ek değişiklik yoktur.

### <a name="working-with-data"></a>Verilerle çalışma
Mobile Services ' mobil uygulama adı Entity Framework kurulumunda varsayılan şema adı olarak sundu.

Şema, uygulamanız için DbContext ayarlamak için kullanmadan önce gibi aşağıdaki başvurulan aynı şema olmasını sağlamak için:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Lütfen yukarıdaki yaparsanız ayarlamak MS_MobileServiceName olduğundan emin olun. Uygulamanız bu önceden özelleştirdiyseniz, başka bir şema adı da sağlayabilirsiniz.

### <a name="system-properties"></a>Sistem Özellikleri
#### <a name="naming"></a>Adlandırma
Azure Mobile Services sunucusu SDK'da, Sistem özellikleri her zaman bir çift alt çizgi içerebilir (`__`) özellikleri için öneki:

* __createdAt
* __updatedAt
* __deleted
* __version

Mobile Services istemci SDK'ları bu biçimde Sistem özellikleri ayrıştırma için özel bir mantığı vardır.

Azure Mobile Apps, Sistem özellikleri artık özel bir biçime sahip ve aşağıdaki adlara sahip:

* CreatedAt
* updatedAt
* silindi
* sürüm

Mobile Apps istemci SDK'ları, herhangi bir değişiklik için istemci kodu gerekli; bu nedenle yeni Sistem özellikleri adları kullanın. REST çağrılarını hizmetinize doğrudan yapıyorsanız ancak, daha sonra sorgularınızı uygun şekilde değiştirmeniz gerekir.

#### <a name="local-store"></a>Yerel depolama
Sistem özellikleri adlarını yapılan değişiklikler, çevrimdışı eşitleme yerel veritabanı Mobile Services için Mobile Apps ile uyumlu değil anlamına gelir. Mümkünse, uygulamalardan Mobile Services sonra mobil uygulamalara kadar bekleyen değişiklikler sunucuya gönderilen istemci yükseltme kaçınmalısınız. Sonra yükseltilen uygulama yeni bir veritabanı dosya adı kullanmanız gerekir.

İşlemi sırasındaki çevrimdışı, bekleyen değişiklikler varken bir istemci uygulaması mobil uygulamaları için Mobile Services yükseltilmiş ise sistem veritabanı yeni sütun adları kullanmak üzere güncelleştirilmelidir. İos'ta, bu sütun adlarını değiştirmek için basit geçişleri kullanmaya elde edilebilir. Android ve .NET yönetilen istemci veri nesnesi tablolarınızı sütunları yeniden adlandırmak için özel SQL yazmanız gerekir.

İos'ta, çekirdek veri şemanızı aşağıdaki eşleştirmek için veri varlıkları için değiştirmeniz gerekir. Unutmayın özellikleri `createdAt`, `updatedAt` ve `version` artık bir `ms_` öneki:

| Öznitelik | Tür | Not |
| --- | --- | --- |
| id |Dize, gerekli olarak işaretlenmiş |Uzak Depolama birincil anahtar |
| CreatedAt |Tarih |createdAt sistem özelliğine (isteğe bağlı) eşlemeleri |
| updatedAt |Tarih |updatedAt sistem özelliğine (isteğe bağlı) eşlemeleri |
| sürüm |Dize |(isteğe bağlı) çakışmaları, maps sürüme algılamak için kullanılan |

#### <a name="querying-system-properties"></a>Sistem özellikleri sorgulama
Azure Mobile Services ' Sistem özellikleri varsayılan olarak, ancak yalnızca sorgu dizesi kullanılarak istendiklerinde gönderilmez `__systemProperties`. Buna karşılık, Azure Mobile Apps sistemde özelliklerdir **her zaman seçili** server SDK nesne modeli parçası olduğundan.

Bu değişiklik çoğunlukla uzantılarını gibi etki alanı yöneticileri özel uygulamaları etkiler `MappedEntityDomainManager`. Bir istemci hiçbir zaman tüm sistem özelliklerini isterse Mobile Services'de kullanılacak mümkündür bir `MappedEntityDomainManager` gerçekten tüm özellikleri eşleşmiyor. Ancak, Azure Mobile Apps'de eşlenmemiş bu özellikleri GET sorgularda hataya neden olur.

Böylece bunlar devralınmalıdır, DTOs değiştirmek için sorunu gidermek için en kolay yolu olan `ITableData` yerine `EntityData`. Ardından, ekleyin `[NotMapped]` özniteliği alınmamalıdır alanları.

Örneğin, aşağıdaki tanımlar `TodoItem` hiçbir sistem özelliklere sahip:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Not: hataları alırsanız, `NotMapped`, derleme için bir başvuru ekleyin `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS
Mobile Services, ASP.NET CORS çözüm kaydırma tarafından bazı CORS desteği dahil. Doğrudan yararlanabilirsiniz şekilde Geliştirici daha fazla denetim sağlamak için bu kaydırma katman kaldırıldı [ASP.NET CORS desteğini](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

CORS kullanıyorsanız sorun ana alanlarını olan `eTag` ve `Location` üstbilgileri düzgün çalışması için İstemci SDK'ları için sırasıyla izin verilecektir.

### <a name="push-notifications"></a>Anında İletme Bildirimleri 
Alım için sunucu SDK'dan eksik bulabilirsiniz ana öğe PushRegistrationHandler sınıftır. Kayıtlar biraz farklı mobil uygulamalarda işlenir ve tagless kayıtlar varsayılan olarak etkinleştirilir. Etiketler yönetme özel API'lerini kullanarak bunu. Lütfen bakın [için etiketler kaydetme](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) daha fazla bilgi için yönergeler.

### <a name="scheduled-jobs"></a>Zamanlanan İşler
.NET arka ucuna sahip var olan tüm işleri ayrı ayrı yükseltilmesi gerekir zamanlanmış işler mobil uygulamasında yerleşiktir böylece değil. Bir seçenektir zamanlanmış bir oluşturmak için [Web işi] mobil uygulama kodu sitesinde. Ayrıca iş kodunuzu tutan kurma denetleyicisi ve yapılandırma [Azure Scheduler] Bu uç beklenen zamanlamada isabet.

### <a name="miscellaneous-changes"></a>Çeşitli değişiklikler
Mobil istemci tarafından kullanılan tüm ApiControllers şimdi olmalıdır `[MobileAppController]` özniteliği. Bu artık değildir diğer ApiControllers mobil biçimlendiricileri tarafından etkilenmemesini gitmek için varsayılan olarak bulunur.

`ApiServices` Nesnesidir artık SDK'ın bir parçası. Mobil uygulama ayarlarına erişmek için aşağıdakileri kullanabilirsiniz:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Benzer şekilde, günlük şimdi standart ASP.NET izleme yazma kullanılarak gerçekleştirilir:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

## <a name="authentication"></a>Kimlik doğrulama konuları
Mobile Services'ın kimlik doğrulaması bileşenleri şimdi App Service kimlik doğrulama/yetkilendirme özelliğini taşınmıştır. Bu, siteniz için okuyarak etkinleştirme hakkında bilgi edinebilirsiniz [mobil uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-ios-get-started-users.md) konu.

AAD, Facebook ve Google, gibi bazı sağlayıcılar için var olan kaydı kopyalama uygulamanızdan yararlanamaz olmalıdır. Kimlik sağlayıcısının Portalı'na gidin ve yeni bir yönlendirme URL'si için kayıt eklemek yeterlidir. Daha sonra App Service kimlik doğrulama/yetkilendirme, istemci kimliği ve parolası ile yapılandırın.

### <a name="controller-action-authorization"></a>Denetleyici eylem yetkilendirme
Tüm örneklerini `[AuthorizeLevel(AuthorizationLevel.User)]` öznitelik şimdi değiştirilmelidir standart ASP.NET kullanmak için `[Authorize]` özniteliği. Ayrıca, denetleyicileri anonim diğer ASP.NET uygulamaları olduğu gibi varsayılan olarak sunulmuştur.
Lütfen, yönetici veya uygulama gibi diğer AuthorizeLevel seçeneklerden birini kullanıyorsanız bu kaldırılmıştır olduğuna dikkat edin. Bunun yerine, AuthorizationFilters için paylaşılan gizli kod dizeleri ayarlayın veya hizmet-hizmet çağrıları güvenle sağlamak için bir AAD hizmet sorumlusu yapılandırın.

### <a name="getting-additional-user-information"></a>Ek kullanıcı bilgilerini alma
Erişim belirteçleri aracılığıyla ek kullanıcı bilgilerini alabileceğiniz `GetAppServiceIdentityAsync()` yöntemi:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Ayrıca, uygulamanızı bağımlılıkları kimlikleri, kullanıcı, bir veritabanında saklamak gibi alırsa, Mobile Services ve App Service Mobile Apps arasındaki kullanıcı kimlikleri farklı olduğuna dikkat edin önemlidir. Ancak Mobile Services kullanıcı kimliği, elde edebilirsiniz. Tüm ProviderCredentials alt sınıfların, bir kullanıcı kimliği özelliği vardır. Bu nedenle önce örneğindeki etmeden:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Uygulamanızı bağımlılıkları kullanıcı kimlikleri izlerseniz, aynı kayıt bir kimlik sağlayıcısı ile mümkünse yararlanan olduğunu önemlidir. Yeni bir kayıt giriş verilerini kullanıcılara eşleşen sorunları oluşturabilirsiniz için kullanıcı kimlikleri genellikle kullanılan, uygulama kaydı kapsamlıdır.

### <a name="custom-authentication"></a>Özel kimlik doğrulama
Uygulamanızı bir özel kimlik doğrulama çözümü kullanıyorsanız, yükseltilen site sistemine erişimi olduğundan emin olun isteyeceksiniz. Özel kimlik doğrulama için yeni yönergeleri [.NET sunucusu SDK Genel Bakış] çözümünüzü tümleştirme. Özel kimlik doğrulama bileşenleri hala önizlemede olduğunu unutmayın.

## <a name="updating-clients"></a>İstemcileri güncelleştirme
İşletimsel bir mobil uygulama arka ucu olduktan sonra hangi içereceği tükettiği istemci uygulamanızı yeni bir sürümünü çalışabilirsiniz. Mobile Apps istemci SDK ' yeni bir sürümünü de içerir ve benzer sunucu yükseltme için Mobile Apps sürümleri yüklemeden önce Mobile Services SDK'ları tüm başvurularını kaldırmanız gerekir.

Sürümleri arasında ana değişikliklerden birini oluşturucular artık bir uygulama anahtarı gerekmesidir. Artık yalnızca mobil uygulamanızın URL'de geçirdiğiniz. Örneğin, .NET istemcilerde `MobileServiceClient` Oluşturucusu olan şimdi:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Yeni SDK'ye yükleme ve yeni yapısı aşağıdaki bağlantıları üzerinden kullanma hakkında bilgi edinebilirsiniz:

* [iOS sürüm 3.0.0 veya daha yenisi](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) sürüm 2.0.0 veya daha yenisi](app-service-mobile-dotnet-how-to-use-client-library.md)

Olmuştur gibi bazı değişiklikler vardır de uygulamanız anında iletme bildirimleri kullanın, her platform için belirli kayıt yönergeleri not yapıyorsa.

Yeni istemci sürümü hazır olduğunda, yükseltilen sunucu projenizi karşı deneyin. Çalışır durumda olduğunu doğrulandıktan sonra müşterilere, uygulamanızın yeni bir sürüm serbest bırakabilirsiniz. Sonuç olarak, müşterilerinizin bu güncelleştirmeleri almak için bir fırsat beklendiğinden sonra uygulamanızın Mobile Services sürüm silebilirsiniz. Bu noktada, bir mobil en son mobil uygulamalar sunucusu SDK kullanarak uygulama hizmeti için tamamen yükselttiniz.

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Klasik Azure portalı]: https://manage.windowsazure.com/
[Mobile Apps nedir?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[mobil uygulama sunucusu SDK'sı]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web işi]: https://github.com/Azure/azure-webjobs-sdk/wiki
[.NET sunucusu SDK kullanmayı]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[uygulama hizmeti fiyatlandırma]: https://azure.microsoft.com/pricing/details/app-service/
[.NET sunucusu SDK Genel Bakış]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
