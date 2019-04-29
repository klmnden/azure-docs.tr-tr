---
title: Azure App Service'e mobil hizmetler'den yükseltme
description: Bir App Service Mobile Apps, Mobile Services uygulamanıza kolayca yükseltmeyi öğrenin
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
ms.openlocfilehash: f5ffc795e6469971d1eaf335d6683f94d05f0807
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122447"
---
# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Mevcut .NET Azure Mobile Services'ı App Service'a yükseltme
App Service Mobile, Microsoft Azure kullanarak mobil uygulamalar derlemek için yepyeni bir yoludur. Daha fazla bilgi için bkz. [Mobile Apps nedir?].

Bu konu, mevcut bir .NET arka uç uygulamasını yeni bir App Service Mobile Apps için Azure Mobile Services yükseltme açıklar. Bu yükseltme gerçekleştirirken, var olan mobil hizmetler uygulamanız çalışmaya devam edebilir.   Node.js arka uç uygulaması yükseltmeniz gerekiyorsa başvurmak [Node.js mobil hizmetlerinizi yükseltme](app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Azure App Service'e bir mobil arka uç yükseltildiğinde, tüm App Service özellikleri erişebilir ve ayarına göre faturalandırılır [App Service fiyatlandırması], olmayan mobil hizmetler fiyatlandırma.

## <a name="migrate-vs-upgrade"></a>Yükseltme geçirme
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Bu önerilir, [geçiş gerçekleştirmek](app-service-mobile-migrating-from-mobile-services.md) yükseltme geçmeden önce. Bu şekilde uygulamanızı her iki sürümü de aynı App Service planı üzerinde yerleştirin ve hiçbir ek ücret uygulanır.
>
>

### <a name="improvements-in-mobile-apps-net-server-sdk"></a>Mobile Apps .NET sunucu SDK'sı yenilikleri
Yeni yükseltme [Mobile Apps SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) aşağıdaki avantajları sağlar:

* NuGet bağımlılıkları hakkında daha fazla esneklik sağlar. Barındırma ortamı artık kendi NuGet paketleri sürümlerini sağlar. böylece alternatif uyumlu sürümlerini kullanabilirsiniz. Ancak, yeni kritik hata düzeltmeleri veya güvenlik güncelleştirmeleri Mobile Sunucu SDK'sı veya bağımlılıkları varsa, hizmetinizi el ile güncelleştirmeniz gerekir.
* Daha fazla esneklik mobil SDK. Hangi özelliklerin açıkça denetleyebilirsiniz ve yollar, kimlik doğrulama, tablo API'leri ve anında iletme kayıt uç noktası gibi ayarlanır. Daha fazla bilgi için bkz. [nasıl Azure Mobile Apps için .NET sunucu SDK'sı kullanılacağını](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).
* Diğer ASP.NET proje türleri ve yolları için destek. Artık mobil arka uç projeniz gibi aynı projede MVC ve Web API denetleyicisi barındırabilirsiniz.
* Web ve mobil uygulamalarınız arasında ortak bir kimlik doğrulama yapılandırması kullanmanıza olanak tanıyan yeni App Service kimlik doğrulama özellikleri için destek.

## <a name="overview"></a>Temel yükseltmeye genel bakış
Çoğu durumda, yükseltme yeni Mobile Apps SDK'sı geçirilmesine ve kodunuzu yeni bir mobil uygulama örneği yeniden yayımlanması kadar basit olacaktır. Vardır, ancak Gelişmiş kimlik doğrulama senaryoları ve çalışmak gibi bazı ek yapılandırma gerektiren bazı senaryolar işleri zamanlanmış. Bunların her biri, sonraki bölümlerde ele alınmıştır.

> [!TIP]
> Okuma ve bir yükseltmeye başlatmadan önce bu konunun geri kalanında tamamen anlamanız önerilir. Not herhangi bir özelliğe sahip, aşağıda belirtilir kullanın.
>
>

Mobile Services istemci SDK'ları olan **değil** yeni Mobile Apps sunucu SDK'sı ile uyumludur. Uygulamanız için hizmet sürekliliği sağlamak için değişiklikleri şu anda yayımlanan istemcilere hizmet veren bir siteye yayımlamalısınız değil. Bunun yerine, bir yineleme hizmet veren yeni bir mobil uygulama oluşturmanız gerekir. Bu uygulama ek maliyet ücretlendirmeden kaçınmak için aynı App Service planı üzerinde koyabilirsiniz.

Ardından uygulamanın iki sürümlerini olacaktır: biri, aynı kalır ve hizmet yayımlanan uygulamaları joker diğer, ardından Yükseltme yapabileceğiniz ve yeni bir istemci sürümü ile hedef. Taşıyabilir ve kendi hızınızda kodunuzu test etmek, ancak her ikisini de yaptığınız herhangi bir hata düzeltmeleri uygulandığından emin olmanız gerekir. Bir kez istediğiniz uygulamaları yazılımların en son sürüme güncelleştirilmiş istemci istenen sayısını, özgün geçirilen uygulamayı silebilirsiniz olduğunu düşünüyor.

Yükseltme işlemi için tam anahat aşağıdaki gibidir:

1. Yeni bir mobil uygulama oluşturun
2. Yeni Sunucu SDK'ları kullanmak için projeyi güncelleştir
3. İstemci uygulamanızın yeni bir sürüm
4. (İsteğe bağlı) Özgün geçirilen örneğinizin Sil

## <a name="mobile-app-version"></a>İkinci bir uygulama örneği oluşturma
Yükseltme sırasında ilk adım, uygulamanızın yeni sürümünü barındıracak mobil uygulama kaynağı oluşturmaktır. Mevcut mobil hizmeti zaten yaptıysanız, bu sürüm üzerinde aynı barındırma planı oluşturmak isteyeceksiniz. Açık [Azure portal] ve geçirilen uygulamanıza gidin. App Service planı üzerinde çalıştırıldığı not edin.

Ardından, ikinci uygulama örneğini oluşturma [.NET arka uç oluşturma yönergeleri](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Seçtiğiniz istendiğinde App Service planı ya da "barındırma planı" geçirilen uygulamanızın planı seçin.

Büyük olasılıkla mobil Hizmetler'de yaptığınız gibi aynı veritabanını ve bildirim hub'ı kullanmak istersiniz. Bu değerleri açarak kopyalayabilirsiniz [Azure portal] ve özgün uygulamasına gidip, ardından **ayarları** > **uygulama ayarları**. Altında **bağlantı dizeleri**, kopyalama `MS_NotificationHubConnectionString` ve `MS_TableConnectionString`. Yeni yükseltme sitenize gidin ve tüm mevcut değerlerin üzerine yazar, yapıştırın. Herhangi bir uygulama ayarları için bu işlemi tekrarlamanız uygulama ihtiyaçlarınızı.

ASP.NET projesi uygulamanız için bir kopyasını oluşturun ve yeni sitenize yayımlayın. Yeni URL ile güncelleştirilmiş istemci uygulamasının bir kopyasını kullanarak, her şeyin beklendiği gibi çalıştığını doğrulayın.

## <a name="updating-the-server-project"></a>Sunucu projesi güncelleştiriliyor
Yeni bir Mobile Apps sağlar [Mobil uygulama sunucusu SDK] Mobile Services çalışma zamanı ile aynı işlevlere çoğunu sağlar. İlk olarak, mobil hizmetler paketleri tüm başvurularını kaldırmanız. NuGet Paket Yöneticisi'nde arayın `WindowsAzure.MobileServices.Backend`. Çoğu uygulama da dahil olmak üzere çeşitli paketler burada görürsünüz `WindowsAzure.MobileServices.Backend.Tables` ve `WindowsAzure.MobileServices.Backend.Entity`. Böyle bir durumda, en düşük paket bağımlılık ağacında gibi başlatın `Entity`ve bunu kaldırın. İstendiğinde, tüm bağımlı paketler kaldırmayın. Kaldırılan kadar bu işlemi yineleyin `WindowsAzure.MobileServices.Backend` kendisi.

Bu noktada, mobil hizmetler SDK'larını artık başvuran bir proje gerekir.

Bundan sonra başvuruları Mobile Apps SDK'sı ekleyeceksiniz. Bu yükseltme için çoğu Geliştirici indirmek ve yüklemek istediğiniz `Microsoft.Azure.Mobile.Server.Quickstart` paketi olarak bu tüm gerekli kümesinde çeker.

SDK'ları arasındaki farklar kaynaklanan oldukça derleyici hataları olacaktır, ancak bunlar da kolayca giderilebilir ve bu bölümün geri kalanında ele alınmaktadır.

### <a name="base-configuration"></a>Temel yapılandırma
Ardından, WebApiConfig.cs içinde değiştirebilirsiniz:

```csharp
// Use this class to set configuration options for your mobile service
ConfigOptions options = new ConfigOptions();

// Use this class to set WebAPI configuration options
HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));
```

ile

```csharp
HttpConfiguration config = new HttpConfiguration();
new MobileAppConfiguration()
    .UseDefaultConfiguration()
.ApplyTo(config);

```

> [!NOTE]
> Nasıl özellikleri uygulamanızdan Ekle/Kaldır ve yeni .NET sunucu SDK'sı hakkında daha fazla bilgi edinmek istiyorsanız, lütfen bkz [.NET sunucu SDK'sını kullanma] konu.
>
>

Uygulamanızı yaparsa kimlik doğrulaması özelliklerini kullanabilirsiniz, ayrıca bir OWIN ara yazılımını kaydetmeniz gerekir. Bu durumda, yukarıdaki yapılandırma kodunu yeni bir OWIN başlangıç sınıfı taşımanız gerekir.

1. NuGet paketi ekleme `Microsoft.Owin.Host.SystemWeb` , projenizde zaten bulunmaz.
2. Visual Studio'da, sağ, proje seçin tıklayın ve **Ekle** -> **yeni öğe**. Seçin **Web** -> **genel** -> **OWIN başlangıç sınıfı**.
3. MobileAppConfiguration için yukarıdaki kodu Taşı `WebApiConfig.Register()` için `Configuration()` yeni başlangıç sınıfınızın yöntemi.

Emin `Configuration()` yöntemi ile sona erecek:

```csharp
app.UseWebApi(config)
app.UseAppServiceAuthentication(config);
```

Tam kimlik doğrulaması bölümünde ele alınan kimlik doğrulaması ile ilgili ek değişiklikler var.

### <a name="working-with-data"></a>Verilerle Çalışma
Mobil Hizmetler'de mobil uygulama adı Entity Framework kurulumunda varsayılan şema adı olarak sunulur.

Şema, uygulamanızın DbContext ayarlamak için kullanmadan önce olarak aşağıdaki başvurulan aynı şemaya sahip olmasını sağlamak için:

```csharp
string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");
```

Lütfen yukarıdaki Bunu verilirse MS_MobileServiceName olduğundan emin olun. Uygulamanız bu daha önce özelleştirdiyseniz, başka bir şema adı sağlayabilirsiniz.

### <a name="system-properties"></a>Sistem Özellikleri
#### <a name="naming"></a>Adlandırma
Azure mobil hizmetler sunucu SDK'sı, Sistem özellikleri her zaman bir çift alt çizgi içermelidir (`__`) özellikleri için ön ek:

* __createdAt
* __updatedAt
* __deleted
* __version

Mobil hizmetler istemci SDK'ları Sistem Özellikleri şu biçimde ayrıştırma özel mantığı vardır.

Azure Mobile Apps, Sistem özellikleri artık özel bir biçime sahip ve aşağıdaki adlara sahiptir:

* createdAt
* updatedAt
* silindi
* version

Mobile Apps istemci SDK'ları için istemci kodu için değişiklik gerekmez yeni sistem özellik adlarını kullanın. REST çağrılarını hizmetinize doğrudan yapıyorsanız ancak, ardından, sorgularınızı uygun şekilde değiştirmeniz gerekir.

#### <a name="local-store"></a>Yerel depo
Sistem özellikleri adlarını yapılan değişiklikler, çevrimdışı eşitleme yerel veritabanı mobil hizmetler için Mobile Apps ile uyumlu değil anlamına gelir. Mümkünse, istemci uygulamaları mobil hizmetlerden sonra Mobile Apps'e kadar bekleyen değişiklikler sunucuya gönderildikten yükseltme kaçınmanız gerekir. Sonra yükseltilen uygulama yeni bir veritabanı dosya adı'nı kullanmanız gerekir.

İşlem kuyrukta bekleyen çevrimdışı değişiklikler varken bir istemci uygulaması Mobile Apps için Mobile Services yükseltilmiş ise, sistem veritabanı yeni sütun adlarını kullanmak üzere güncelleştirilmelidir. İOS, bu sütun adlarını değiştirmek için basit migrations'ı kullanma elde. Android ve .NET yönetilen istemci üzerinde veri nesnesi tablolarınızı sütunlarını yeniden adlandırmak için özel SQL yazmanız.

İOS, temel veri şemanızı için veri varlıklarınızı aşağıdaki eşleşecek şekilde değiştirmeniz gerekir. Unutmayın özelliklerini `createdAt`, `updatedAt` ve `version` artık bir `ms_` öneki:

| Öznitelik | Tür | Not |
| --- | --- | --- |
| id |Dize, gerekli olarak işaretlenmiş |Uzak depoda birincil anahtar |
| createdAt |Tarih |(isteğe bağlı) eşlenir createdAt sistem özelliği |
| updatedAt |Tarih |(isteğe bağlı) eşlenir updatedAt sistem özelliği |
| version |String |(isteğe bağlı) eşlemeleri sürüm çakışmaları algılamak için kullanılan |

#### <a name="querying-system-properties"></a>Sistem özellikleri sorgulama
Azure mobil Hizmetleri'nde Sistem özellikleri varsayılan olarak, ancak yalnızca sorgu dizesi kullanarak istendiklerinde gönderilmez `__systemProperties`. Buna karşılık, Azure Mobile Apps sistemde özelliklerdir **her zaman seçili** sunucu SDK'sı nesne modelinin bir parçası olduğundan.

Bu değişiklik, etki alanı yöneticileri, uzantıları gibi özel uygulamalar çoğunlukla etkiler `MappedEntityDomainManager`. Bir istemci hiçbir zaman herhangi bir sistem özellikleri isterse mobil Hizmetler'de kullanmak mümkün olduğu bir `MappedEntityDomainManager` gerçekten tüm özellikleri eşleşmiyor. Ancak, Azure Mobile Apps'te eşlenmemiş bu özellikler GET sorgularda hataya neden olur.

Böylece devralındığı, Dto'lar değiştirmek için bu sorunu çözmek için en kolay yolu olan `ITableData` yerine `EntityData`. Ardından ekleyin `[NotMapped]` alınmamalıdır alanları için özniteliği.

Örneğin, aşağıdaki tanımlar `TodoItem` hiçbir sistem özellikleri ile:

```csharp
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
```

Not: hataları alırsanız, `NotMapped`, derlemeye bir başvuruda bulunun `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS
Mobile Services, ASP.NET CORS çözüm sarmalama tarafından bazı CORS desteği dahil. Doğrudan faydalanarak Geliştirici daha fazla denetim sağlamak için bu sarmalama katman kaldırıldı [ASP.NET CORS desteğini](https://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

Ana CORS kullanıyorsanız sorunlu alanları olan `eTag` ve `Location` üstbilgileri düzgün çalışması için İstemci SDK'ları için sırada izin verilmelidir.

### <a name="push-notifications"></a>Anında İletme Bildirimleri 
Gönderme için sunucu SDK'sından eksik bulabilirsiniz ana öğeyi PushRegistrationHandler sınıftır. Kayıtları Mobile Apps biraz farklı şekilde ele alınır ve tagless kayıtları varsayılan olarak etkindir. Etiketleri yönetme özel API'leri kullanarak gerçekleştirilmesi. Lütfen [etiketlerini kaydetme](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) daha fazla bilgi için yönergeler.

### <a name="scheduled-jobs"></a>Zamanlanan İşler
.NET arka ucunuzu sahip olduğunuz mevcut tüm işler ayrı ayrı yükseltilmesi gerekecek şekilde zamanlanmış işleri Mobil uygulamalarınızda yerleşik olarak bulunmaz. Zamanlanmış bir oluşturmak için bir seçenek olan [Web işi] mobil uygulama kodu sitesinde. Ayrıca, proje kodunuzun tutan bir denetleyicisi ayarlama ve yapılandırma [Azure Scheduler] uç noktanın beklenen zamanlamaya göre isabet.

### <a name="miscellaneous-changes"></a>Çeşitli değişiklikler
Artık, bir mobil istemci tarafından kullanılan tüm ApiControllers olması gerekir `[MobileAppController]` özniteliği. Artık budur tarafından mobil biçimlendiricileri etkilenmeyen gitmek için diğer ApiControllers varsayılan olarak dahildir.

`ApiServices` Nesnedir artık SDK'ın bir parçası. Mobil uygulama ayarlarına erişmek için aşağıdakileri kullanabilirsiniz:

```csharp
MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
```

Benzer şekilde, günlüğe kaydetme artık standart ASP.NET izleme yazılmasını kullanılarak gerçekleştirilir:

```csharp
ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
traceWriter.Info("Hello, World");  
```

## <a name="authentication"></a>Kimlik doğrulama konuları
Mobil hizmetler kimlik doğrulaması bileşenleri artık App Service kimlik doğrulama/yetkilendirme özelliğini taşınmıştır. Bu, sitenizin okuyarak etkinleştirme hakkında bilgi edinebilirsiniz [mobil uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-ios-get-started-users.md) konu.

AAD ve Facebook, Google gibi bazı sağlayıcılar için mevcut kaydı kopyalama uygulamanızdan yararlanabilmelidir olmalıdır. Kimlik sağlayıcısının portalına gidin ve kayıt için yeni bir yeniden yönlendirme URL'si ekleyin yeterlidir. Ardından App Service kimlik doğrulama/yetkilendirme, istemci kimliği ve parolası ile yapılandırın.

### <a name="controller-action-authorization"></a>Denetleyici eylem yetkilendirme
Tüm örneklerini `[AuthorizeLevel(AuthorizationLevel.User)]` öznitelik şimdi değiştirilmesi gereken standart ASP.NET kullanılacak `[Authorize]` özniteliği. Ayrıca, artık anonim diğer ASP.NET uygulamalarını olduğu gibi varsayılan denetleyicileridir.
Lütfen, yönetici veya uygulama gibi diğer AuthorizeLevel seçeneklerden birini kullandıysanız bu gitmiş olduğuna dikkat edin. Bunun yerine, AuthorizationFilters ' için paylaşılan gizlilikleri ayarlayın veya hizmetten hizmete çağrılar güvenli bir şekilde etkinleştirmek için bir AAD hizmet sorumlusu yapılandırın.

### <a name="getting-additional-user-information"></a>Ek kullanıcı bilgilerini alma
Erişim belirteci aracılığıyla ek kullanıcı bilgilerini alabilirsiniz `GetAppServiceIdentityAsync()` yöntemi:

```csharp
FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();
```

Ayrıca, uygulamanızı bağımlılıkları kimlikleri, kullanıcı, bir veritabanında saklamak gibi alırsa, App Service Mobile Apps ile Mobile Services arasındaki kullanıcı kimliklerinin farklı olduğuna dikkat edin önemlidir. Ancak Mobile Services kullanıcı kimliği, elde edebilirsiniz. Tüm ProviderCredentials alt sınıfların, bir kullanıcı kimliği özelliği vardır. Bu nedenle önce örnekten devam etmesini:

```csharp
string mobileServicesUserId = creds.Provider + ":" + creds.UserId;
```

Uygulamanızı herhangi bir bağımlılığın kullanıcı kimlikleri alırsa, aynı kayıt bir kimlik sağlayıcısı ile mümkünse yararlanın, önemlidir. Yeni bir kayıt ile tanışın eşleşen kullanıcılar verilerine sorunları oluşturabilirsiniz böylece kullanıcı kimlikleri genellikle kullanılan uygulama kaydı kapsamına alınır.

### <a name="custom-authentication"></a>Özel kimlik doğrulama
Uygulamanızı bir özel kimlik doğrulama çözümü kullanıyorsanız, yükseltilen site sistemine erişimi olduğundan emin olmak isteyeceksiniz. Özel kimlik doğrulaması için yeni yönergeleri [.NET sunucu SDK'sı genel bakış] çözümünüzü tümleştirmek için. Özel kimlik doğrulama bileşenleri hala Önizleme aşamasında olduğunu lütfen unutmayın.

## <a name="updating-clients"></a>İstemcileri güncelleştirme
İşletimsel bir mobil uygulama arka ucu oluşturduktan sonra kullandığı istemci uygulamanızın yeni bir sürümü üzerinde çalışabilir. Mobile Apps istemci SDK'ları yeni bir sürümünü de içerir ve benzer sunucu yükseltme için Mobile Apps sürümlerini yüklemeden önce mobil hizmetler SDK'ları tüm başvurularını kaldırmanız gerekir.

Sürümleri arasında ana değişiklikleri oluşturucular artık bir uygulama anahtarı gerektirir biridir. Artık yalnızca mobil uygulamanızın URL'SİNDE geçirin. Örneğin, .NET istemcilerde `MobileServiceClient` Oluşturucusu sunuldu:

```csharp
public static MobileServiceClient MobileService = new MobileServiceClient(
    "https://contoso.azurewebsites.net", // URL of the Mobile App
);
```

Yeni SDK yükleme ve yeni yapıyı aracılığıyla aşağıdaki bağlantıları kullanarak ilgili bilgi edinebilirsiniz:

* [iOS sürüm 3.0.0 veya üzeri](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) 2.0.0 sürümü veya üzeri](app-service-mobile-dotnet-how-to-use-client-library.md)

Olmuştur gibi bazı değişiklikleri de uygulamanızı anında iletme bildirimleri kullanın, her platform için belirli kayıt yönergeleri not yaparsa.

Yeni istemci sürümü hazır olduğunda, yükseltilen sunucu projenizi karşı deneyin. Çalışır durumda olduğunu doğruladıktan sonra müşterilerin uygulamanızı yeni bir sürümü serbest bırakabilirsiniz. Sonuç olarak, müşterilerinizin bu güncelleştirmeleri almak için bir fırsat çalıştırılmış sonra uygulamanızın Mobile Services sürümü silebilirsiniz. Bu noktada, bir App Service mobil en son mobil uygulamalar sunucusu SDK'sı kullanarak uygulama için tamamen yükselttiniz.

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Mobile Apps nedir?]: app-service-mobile-value-prop.md
[Mobil uygulama sunucusu SDK]: https://www.nuget.org/packages/microsoft.azure.mobile.server
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /azure/scheduler/
[Web işi]: https://github.com/Azure/azure-webjobs-sdk/wiki
[.NET sunucu SDK'sını kullanma]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[App Service fiyatlandırması]: https://azure.microsoft.com/pricing/details/app-service/
[.NET sunucu SDK'sı genel bakış]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
