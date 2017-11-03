---
title: "Azure uygulama hizmeti - Node.js için Mobile Services'den yükseltme"
description: "Bir mobil uygulama hizmeti Mobile Services uygulamanıza kolayca yükseltmeyi öğrenin"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: yochayk
editor: 
ms.assetid: c58f6df0-5aad-40a3-bddc-319c378218e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 5fc61fed674f0d2fc64bc29c064e7e872b4f2e68
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Varolan Node.js Azure mobil hizmetinizi App Service'e yükseltme
App Service Mobile, Microsoft Azure kullanarak mobil uygulamaları oluşturmak için yeni bir yoludur. Daha fazla bilgi için bkz: [Mobile Apps nedir?].

Bu konu, mevcut Node.js arka uç uygulama için yeni bir App Service Mobile Apps Azure Mobile Services yükseltme açıklar. Bu yükseltme gerçekleştirirken mevcut Mobile Services uygulamanız çalışmaya devam edebilirsiniz.  Bir Node.js arka uç uygulaması yükseltme yapmanız oluştuysa, [.NET Mobile Services yükseltme](app-service-mobile-net-upgrading-from-mobile-services.md).

Azure App Service'e bir mobil arka uç yükseltildiğinde, tüm uygulama hizmeti özelliklerine erişebilir ve göre faturalandırılır [uygulama hizmeti fiyatlandırma], fiyatlandırma değil Mobile Services.

## <a name="migrate-vs-upgrade"></a>Yükseltme geçirme
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Önerilir Bu, [bir geçiş gerçekleştirmek](app-service-mobile-migrating-from-mobile-services.md) yükseltme geçmeden önce. Bu şekilde, aynı uygulama hizmeti planı üzerinde her iki sürümü, uygulamanızın koyun ve ek bir maliyet doğurur.
>
>

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Mobile Apps Node.js sunucusu SDK yenilikleri
Yeni yükseltme [Mobile Apps SDK'sı](https://www.npmjs.com/package/azure-mobile-apps) iyileştirmeler de dahil olmak üzere birçok sağlar:

* Temel [Express framework](http://expressjs.com/en/index.html), yeni bir düğüm SDK hafif ve geldikleri gibi yeni düğümü sürümlerle karşılamak üzere tasarlanmıştır. Express ara yazılımını ile uygulama davranışını özelleştirebilirsiniz.
* Mobile Services SDK'sına göre önemli performans geliştirmeleri.
* Bir Web sitesi ile birlikte mobil arka şimdi barındırabilir; benzer şekilde, tüm mevcut express.v4 uygulamaya Azure Mobile SDK'sı ekleme kolaydır.
* Platformlar arası ve yerel geliştirme için yerleşik, Mobile Apps SDK'sı geliştirilen ve yerel olarak Windows, Linux ve OSX platformlarda çalıştırmak kullanabilirsiniz. Şimdi çalışıyor gibi kullanımı kolay ortak düğüm geliştirme teknikleri olan [Mocha](https://mochajs.org/) testleri dağıtımından önce.

## <a name="overview"></a>Temel yükseltmeye genel bakış
Node.js arka ucu yükseltme yardımcı olmak için Azure App Service uyumluluğu paketi sağlamıştır.  Yükseltmeden sonra yeni bir uygulama hizmeti siteye dağıtılabilir bir niew sitesi olur.

Mobile Services istemci SDK'ları olan **değil** yeni mobil uygulamalar sunucusu SDK ile uyumlu. Uygulamanız için hizmetin sürekliliği sağlamak için değişiklikleri şu anda yayımlanan istemciler hizmet veren bir site yayınlamalıdır değil. Bunun yerine, yinelenen hizmet veren yeni bir mobil uygulama oluşturmanız gerekir. Bu uygulamayı ek finansal ücret oluşmasını önlemek için aynı uygulama hizmeti plan üzerinde koyabilirsiniz.

Uygulamanın iki sürümlerini sonra gerekir: aynı kalır ve hizmet veren bir yayımlanan uygulamalar joker diğer, ardından yükseltin ve yeni bir istemci sürümü ile hedef. Taşıma ve kodunuzu test etmek, hızı, ancak yaptığınız tüm hata düzeltmeleri hem de uygulandığından emin olmanız gerekir. İstediğiniz istemci uygulamaları yazılımların en son sürüme güncelleştirilmemiş istenen sayısını, özgün geçirilen uygulama silebilirsiniz olduğunu düşündüğünüz sonra. Mobil uygulamanızın aynı uygulama hizmeti planında barındırılan kullanılıyorsa, tüm ek para ücrete neden değil.

Yükseltme işlemi için tam anahattı aşağıdaki gibidir:

1. Mevcut (geçirilen) Azure mobil hizmetiniz indirin.
2. Proje Uyumluluk Paketi kullanarak bir Azure mobil uygulaması dönüştürün.
3. Farkları (örneğin, kimlik doğrulama ayarları) düzeltin.
4. Dönüştürülen Azure mobil uygulaması projeniz için yeni bir uygulama hizmeti dağıtın.
5. Yeni mobil uygulamayı istemci uygulamanızı yeni bir sürümü kullanıma.
6. (İsteğe bağlı) Özgün geçirilen mobil hizmet uygulamanızı silin.

Özgün geçirilen mobil hizmette herhangi bir trafik görmüyorum silme ortaya çıkabilir.

## <a name="install-npm-package"></a>Önkoşulları yüklemek
Yerel makinenizde [Node] yüklemeniz gerekir.  Uyumluluk Paketi de yüklemeniz gerekir.  Düğüm yüklendikten sonra yeni cmd ya da PowerShell komut isteminde aşağıdaki komutu çalıştırabilirsiniz:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Azure mobil hizmetler komut dosyalarınız alın
* [Azure Portal]’da oturum açın.
* Kullanarak **tüm kaynakları** veya **uygulama hizmetleri**, Mobile Services sitenizi bulun.
* Site içinde tıklayın **Araçları** -> **Kudu** -> **Git** Kudu sitesini açın.
* Tıklayın **hata ayıklama Konsolu'nda** -> **PowerShell** hata ayıklama konsolunu açın.
* Gidin `site/wwwroot/App_Data/config` her bir dizinin sırayla tıklayarak
* İndirme simgesine tıklayın `scripts` dizin.

Bu komut dosyaları ZIP biçiminde indirir.  Yerel makinenizde yeni bir dizin oluşturun ve paket `scripts.ZIP` dizin içindeki dosya.  Bu oluşturacak bir `scripts` dizin.

## <a name="scaffold-app"></a>Yeni Azure Mobile Apps arka iskele
Komut dosyaları dizinini içeren dizininden aşağıdaki komutu çalıştırın:

```scaffold-mobile-app scripts out```

Bu kurulmuş bir Azure Mobile Apps arka ucu oluşturacak `out` dizin.  Gerekli değildir, ancak bunu denetlemek için iyi bir fikirdir `out` tercih ettiğiniz bir kaynak kod havuzunda dizin.

## <a name="deploy-ama-app"></a>Azure Mobile Apps arka dağıtma
Dağıtım sırasında aşağıdakileri yapmanız gerekir:

1. Yeni bir mobil uygulaması oluşturmak [Azure Portal].
2. Çalıştırma `createViews.sql` bağlı veritabanınızı komut.
3. Yeni uygulama hizmetiniz için mobil hizmetinize bağlı veritabanı bağlayın.
4. Yeni uygulama hizmeti (gibi bildirim hub'ları) başka kaynaklar bağlayın.
5. Oluşturulan kod yeni sitenize dağıtın.

### <a name="create-a-new-mobile-app"></a>Yeni bir mobil uygulaması oluşturma
1. [Azure Portal]’da oturum açın.
2. **+YENİ** > **Web + Mobil** > **Mobil Uygulama**’ya tıklayıp Mobile Uygulama arka ucu için bir ad verin.
3. **Kaynak Grubu** için yeni bir kaynak grubu seçin ya da yeni bir tane oluşturun (uygulamanızla aynı adı kullanarak).

    Başka bir App Service planı seçin veya yeni bir tane oluşturun. Uygulama hizmetleri hakkında daha fazla bilgi için planları ve farklı fiyatlandırma yeni bir plan oluşturmak nasıl katmanı ve tercih ettiğiniz konumda bkz [Azure App Service planlarına ayrıntılı genel bakış](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. **App Service planı** için, varsayılan plan ([Standart katman](https://azure.microsoft.com/pricing/details/app-service/)’da) seçilidir. Aynı zamanda başka bir plan da seçebilir veya [yeni bir tane oluşturabilirsiniz](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). App Service planının ayarları, uygulamanızla ilişkili [konumu, özellikleri, maliyeti ve işlem kaynaklarını](https://azure.microsoft.com/pricing/details/app-service/) saptar.

    Planla ilgili kararı verdikten sonra **Oluştur**’a tıklayın. Böylece Mobil Uygulama arka uç oluşturulur.

### <a name="run-createviewssql"></a>CreateViews.SQL Çalıştır
Adlı bir dosya kurulmuş uygulamasını içeren `createViews.sql`.  Bu komut dosyasını hedef veritabanında yürütülmelidir.  Hedef veritabanı için bağlantı dizesi geçirilen mobil hizmetinden gelen elde edilebilir **ayarları** altında dikey **bağlantı dizeleri**.  Bu adlı `MS_TableConnectionString`.

Bu komut dosyasından SQL Server Management Studio veya Visual Studio içinde çalıştırabilirsiniz.

### <a name="link-the-database-to-your-app-service"></a>Uygulama hizmetiniz için veritabanı bağlantı
Varolan bir veritabanını uygulama hizmetiniz için bağlantı:

* İçinde [Azure Portal], uygulama hizmetiniz açın.
* Seçin **tüm ayarları** -> **veri bağlantıları**.
* Tıklayın **+ Ekle**.
* Aşağı açılır menüsünde, seçin **SQL veritabanı**
* Altında **SQL veritabanı**mevcut veritabanını seçin, ardından tıklayın **seçin**.
* Altında **bağlantı dizesi**, veritabanı için kullanıcı adını ve parolasını girin, ardından tıklayın **Tamam**.
* İçinde **veri bağlantıları ekleme** dikey penceresinde, tıklatıldığında **Tamam**.

Kullanıcı adı ve parola geçirilen mobil hizmetinizi hedef veritabanı için bağlantı dizesini görüntüleyerek bulunabilir.

### <a name="set-up-authentication"></a>Kimlik doğrulamasını ayarlama
Azure Mobile Apps, Azure Active Directory, Facebook, Google, Microsoft ve Twitter kimlik doğrulama hizmeti içinde yapılandırmanıza olanak sağlar.  Özel kimlik doğrulama ayrı olarak geliştirilen gerekir.  Başvurmak [kimlik doğrulaması kavramlarını] belgelerine ve [kimlik doğrulaması Hızlı Başlangıç] daha fazla bilgi için.  

## <a name="updating-clients"></a>Mobil istemcilerin güncelleştir
İşletimsel bir mobil uygulama arka ucu olduktan sonra hangi içereceği tükettiği istemci uygulamanızı yeni bir sürümünü çalışabilirsiniz. Mobile Apps istemci SDK ' yeni bir sürümünü de içerir ve benzer sunucu yükseltme için Mobile Apps sürümleri yüklemeden önce Mobile Services SDK'ları tüm başvurularını kaldırmanız gerekir.

Sürümleri arasında ana değişikliklerden birini oluşturucular artık bir uygulama anahtarı gerekmesidir.
Artık yalnızca mobil uygulamanızın URL'de geçirdiğiniz. Örneğin, .NET istemcilerde `MobileServiceClient` Oluşturucusu olan şimdi:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net" // URL of the Mobile App
        );

Yeni SDK'ye yükleme ve yeni yapısı aşağıdaki bağlantıları üzerinden kullanma hakkında bilgi edinebilirsiniz:

* [Android 2.2 veya sonraki sürümü](app-service-mobile-android-how-to-use-client-library.md)
* [iOS sürüm 3.0.0 veya daha yenisi](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) sürüm 2.0.0 veya daha yenisi](app-service-mobile-dotnet-how-to-use-client-library.md)
* [Apache Cordova sürüm 2.0 veya üstü](app-service-mobile-cordova-how-to-use-client-library.md)

Olmuştur gibi bazı değişiklikler vardır de uygulamanız anında iletme bildirimleri kullanın, her platform için belirli kayıt yönergeleri not yapıyorsa.

Yeni istemci sürümü hazır olduğunda, yükseltilen sunucu projenizi karşı deneyin. Çalışır durumda olduğunu doğrulandıktan sonra müşterilere, uygulamanızın yeni bir sürüm serbest bırakabilirsiniz. Sonuç olarak, müşterilerinizin bu güncelleştirmeleri almak için bir fırsat beklendiğinden sonra uygulamanızın Mobile Services sürüm silebilirsiniz. Bu noktada, bir mobil en son mobil uygulamalar sunucusu SDK kullanarak uygulama hizmeti için tamamen yükselttiniz.

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Mobile Apps nedir?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: https://github.com/Azure/azure-webjobs-sdk/wiki
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[uygulama hizmeti fiyatlandırma]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[kimlik doğrulaması kavramlarını]: ../app-service/app-service-authentication-overview.md
[kimlik doğrulaması Hızlı Başlangıç]: app-service-mobile-auth.md

[Azure Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
