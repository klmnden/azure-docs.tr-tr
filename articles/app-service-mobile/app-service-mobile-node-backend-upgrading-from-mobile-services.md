---
title: Azure App Service - Node.js için mobil hizmetler'den yükseltme
description: Bir App Service Mobile Apps, Mobile Services uygulamanıza kolayca yükseltmeyi öğrenin
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: yochayk
editor: ''
ms.assetid: c58f6df0-5aad-40a3-bddc-319c378218e3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: e54ed6c526182cea57e2d40f356ad9236510d82c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128075"
---
# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Kullanarak mevcut Node.js Azure Mobile Services'ı App Service'a yükseltme
App Service Mobile, Microsoft Azure kullanarak mobil uygulamalar derlemek için yepyeni bir yoludur. Daha fazla bilgi için bkz. [Mobile Apps nedir?].

Bu makalede, var olan bir Node.js arka uç uygulaması, yeni bir App Service Mobile Apps için Azure Mobile Services yükseltme açıklanır. Bu yükseltme gerçekleştirirken, var olan mobil hizmetler uygulamanız çalışmaya devam edebilir.  Node.js arka uç uygulaması yükseltmeniz gerekiyorsa başvurmak [.NET Mobil hizmetlerinizi yükseltme](app-service-mobile-net-upgrading-from-mobile-services.md).

Azure App Service'e bir mobil arka uç yükseltildiğinde, tüm App Service özellikleri erişebilir ve ayarına göre faturalandırılır [App Service fiyatlandırması], olmayan mobil hizmetler fiyatlandırma.

## <a name="migrate-vs-upgrade"></a>Yükseltme geçirme
[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

> [!TIP]
> Bu önerilir, [geçiş gerçekleştirmek](app-service-mobile-migrating-from-mobile-services.md) yükseltme geçmeden önce. Bu şekilde uygulamanızı her iki sürümü de aynı App Service planı üzerinde yerleştirin ve hiçbir ek ücret uygulanır.
>
>

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Mobile Apps Node.js sunucu SDK'sı yenilikleri
Yeni yükseltme [Mobile Apps SDK'sı](https://www.npmjs.com/package/azure-mobile-apps) dahil olmak üzere birçok geliştirmeler sağlar:

* Temel [Express framework](https://expressjs.com/en/index.html), yeni düğüm SDK'sı hafif ve yeni düğüm sürümlerle sunulur açık kalmasını sağlamak için tasarlanmıştır. Express ara yazılımını ile uygulama davranışını özelleştirebilirsiniz.
* Mobile Services SDK'sı için karşılaştırıldığında önemli performans geliştirmeleri.
* Artık mobil arka uç ile birlikte bir Web sitesi barındırabilirsiniz; benzer şekilde, var olan tüm express.v4 uygulamasına Azure Mobile SDK'sı eklemek kolaydır.
* Platformlar arası ve yerel geliştirme için yerleşik, Mobile Apps SDK'sı geliştirilebileceği ve Windows, Linux veya OSX platformunda yerel olarak çalıştırın. Çalıştırma gibi kullanımı kolay genel düğüm geliştirme tekniklerini sunulmuştur [Mocha](https://mochajs.org/) dağıtımından önce testler.

## <a name="overview"></a>Temel yükseltmeye genel bakış
Azure App Service Node.js arka ucu yükseltmeyi yardımcı olmak için bir uyumluluk paket sağlamıştır.  Yükseltmeden sonra yeni bir App Service sitesine dağıtılabilen yeni bir site gerekir.

Mobile Services istemci SDK'ları olan **değil** yeni Mobile Apps sunucu SDK'sı ile uyumludur. Uygulamanız için hizmet sürekliliği sağlamak için değişiklikleri şu anda yayımlanan istemcilere hizmet veren bir siteye yayımlamalısınız değil. Bunun yerine, bir yineleme hizmet veren yeni bir mobil uygulama oluşturmanız gerekir. Bu uygulama ek maliyet ücretlendirmeden kaçınmak için aynı App Service planı üzerinde koyabilirsiniz.

Ardından iki uygulama sürümü gerekir: aynı kalır ve hizmet veren bir yayımlanan uygulamaları joker diğer, ardından Yükseltme yapabileceğiniz ve yeni bir istemci sürümü ile hedef. Taşıyabilir ve kendi hızınızda kodunuzu test etmek, ancak her ikisini de yaptığınız herhangi bir hata düzeltmeleri uygulandığından emin olmanız gerekir. Bir kez istediğiniz uygulamaları yazılımların en son sürüme güncelleştirilmiş istemci istenen sayısını, özgün geçirilen uygulamayı silebilirsiniz olduğunu düşünüyor. Bu, mobil uygulama olarak aynı App Service planında barındırılıyorsa değil herhangi ek parasal ücret yansıtılmaz.

Yükseltme işlemi için tam anahat aşağıdaki gibidir:

1. Mevcut uygulamanızı (yükseltilen) Azure Mobile Services'ı indirin.
2. Proje Uyumluluk Paketi kullanarak bir Azure mobil uygulaması dönüştürün.
3. Farkları (örneğin, kimlik doğrulama ayarları) düzeltin.
4. Dönüştürülen Azure mobil uygulaması projenize yeni bir App Service'e dağıtın.
5. Yeni mobil uygulamanın kullandığı istemci uygulamanızın yeni bir sürümün.
6. (İsteğe bağlı) Özgün geçirilen mobil hizmet uygulamanızı silin.

Silme işlemi tüm trafiği özgün geçirilen mobil hizmetinizi göremiyor meydana gelebilir.

## <a name="install-npm-package"></a> Önkoşulları yükle
Yerel makinenizde [Node] yüklemeniz gerekir.  Ayrıca, uyumluluk paketini yüklemeniz gerekir.  Düğüm yüklendikten sonra yeni cmd ya da PowerShell isteminden aşağıdaki komutu çalıştırabilirsiniz:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a> Azure mobil hizmetler komut dosyalarınızı alın
* [Azure Portal]’da oturum açın.
* Kullanarak **tüm kaynakları** veya **uygulama hizmetleri**, mobil hizmetler sitenizi bulun.
* Site içinde tıklayın **Araçları** -> **Kudu** -> **Git** Kudu sitesini açın.
* Tıklayarak **hata ayıklama konsolunu** -> **PowerShell** hata ayıklama konsolunu açın.
* Gidin `site/wwwroot/App_Data/config` sırayla her bir dizinin tıklayarak
* İndirme simgeye tıklayın `scripts` dizin.

Bu ZIP biçiminde betikleri indirir.  Yerel makinenizde yeni bir dizin oluşturun ve paket `scripts.ZIP` dizininden dosya.  Bu oluşturacak bir `scripts` dizin.

## <a name="scaffold-app"></a> İskele yeni Azure Mobile Apps arka ucu
Komut dosyaları dizinini içeren dizinden aşağıdaki komutu çalıştırın:

```scaffold-mobile-app scripts out```

Bu iskele kurulmuş bir Azure Mobile Apps arka ucu oluşturur `out` dizin.  Gerekli olmamakla birlikte, bunu kontrol etmek için iyi bir fikirdir `out` seçtiğiniz kaynak kodu deposu içinde dizin.

## <a name="deploy-ama-app"></a> Azure Mobile Apps arka ucunuzu dağıtma
Dağıtım sırasında aşağıdakileri yapmanız gerekir:

1. Yeni bir mobil uygulama oluşturma [Azure portal].
2. Çalıştırma `createViews.sql` bağlı veritabanınızı komut.
3. Yeni uygulama hizmetiniz, mobil hizmetinize bağlı olduğu veritabanı bağlayın.
4. Diğer tüm kaynakları (örneğin, bildirim hub'ları) için yeni App Service bağlayın.
5. Oluşturulan kodun yeni sitenize dağıtma.

### <a name="create-a-new-mobile-app"></a>Yeni bir mobil uygulama oluşturun
1. [Azure Portal]’da oturum açın.
2. **+YENİ** > **Web + Mobil** > **Mobil Uygulama**’ya tıklayıp Mobile Uygulama arka ucu için bir ad verin.
3. **Kaynak Grubu** için yeni bir kaynak grubu seçin ya da yeni bir tane oluşturun (uygulamanızla aynı adı kullanarak).

    Başka bir App Service planı seçin veya yeni bir tane oluşturun. Uygulama Hizmetleri planları, farklı fiyatlandırma katmanında ve tercih ettiğiniz konumda yeni plan oluşturma hakkında daha fazla bilgi için bkz. [Azure App Service planlarına ayrıntılı genel bakış](../app-service/overview-hosting-plans.md).
4. **App Service planı** için, varsayılan plan ([Standart katman](https://azure.microsoft.com/pricing/details/app-service/)’da) seçilidir. Aynı zamanda başka bir plan da seçebilir veya [yeni bir tane oluşturabilirsiniz](../app-service/app-service-plan-manage.md#create-an-app-service-plan). App Service planının ayarları belirlemek [konumu, özellikleri, maliyeti ve işlem kaynaklarını](https://azure.microsoft.com/pricing/details/app-service/) uygulamanızla ilişkili.

    Planla ilgili kararı verdikten sonra **Oluştur**’a tıklayın. Böylece Mobil Uygulama arka uç oluşturulur.

### <a name="run-createviewssql"></a>CreateViews.SQL çalıştırın
Adlı bir dosya iskeleli uygulamayı içeren `createViews.sql`.  Bu betik, hedef veritabanında yürütülmelidir.  Hedef veritabanı için bağlantı dizesi geçirilen mobil hizmetinizden örneğinden alınabilen **ayarları** altındaki **bağlantı dizeleri**.  İsimlendirilmiştir `MS_TableConnectionString`.

Bu komut dosyasından SQL Server Management Studio veya Visual Studio içinde çalıştırabilirsiniz.

### <a name="link-the-database-to-your-app-service"></a>Uygulama hizmetinize veritabanını Bağla
Varolan bir veritabanını uygulama hizmetinize bağlayın:

* İçinde [Azure portal], uygulama hizmetinizi açın.
* Seçin **tüm ayarlar** -> **veri bağlantıları**.
* Tıklayarak **+ Ekle**.
* Açılan menü, seçin **SQL veritabanı**
* Altında **SQL veritabanı**, mevcut veritabanını seçin ve ardından tıklayarak **seçin**.
* Altında **bağlantı dizesi**, veritabanı için kullanıcı adı ve parola girin ve ardından tıklayarak **Tamam**.
* İçinde **veri bağlantıları Ekle** sayfasında, tıklayarak **Tamam**.

Kullanıcı adı ve parola, hedef veritabanı için bağlantı dizesi geçirilen mobil hizmetinizde görüntüleyerek bulunabilir.

### <a name="set-up-authentication"></a>Kimlik doğrulamasını ayarlama
Azure Mobile Apps, yapılandırma, Azure Active Directory, Facebook, Google, Microsoft ve Twitter kimlik doğrulaması hizmeti içindeki olanak tanır.  Özel kimlik doğrulama, ayrı olarak geliştirilen gerekecektir.  Başvurmak [kimlik doğrulaması kavramları] belgeleri ve [kimlik doğrulama-Hızlı Başlangıç] daha fazla bilgi için belgelere bakın.  

## <a name="updating-clients"></a>Mobil istemciler güncelleştir
İşletimsel bir mobil uygulama arka ucu oluşturduktan sonra kullandığı istemci uygulamanızın yeni bir sürümü üzerinde çalışabilir. Mobile Apps istemci SDK'ları yeni bir sürümünü de içerir ve benzer sunucu yükseltme için Mobile Apps sürümlerini yüklemeden önce mobil hizmetler SDK'ları tüm başvurularını kaldırmanız gerekir.

Sürümleri arasında ana değişiklikleri oluşturucular artık bir uygulama anahtarı gerektirir biridir.
Artık yalnızca mobil uygulamanızın URL'SİNDE geçirin. Örneğin, .NET istemcilerde `MobileServiceClient` Oluşturucusu sunuldu:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net" // URL of the Mobile App
        );

Yeni SDK yükleme ve yeni yapıyı aracılığıyla aşağıdaki bağlantıları kullanarak ilgili bilgi edinebilirsiniz:

* [Android 2.2 veya sonraki sürümü](app-service-mobile-android-how-to-use-client-library.md)
* [iOS sürüm 3.0.0 veya üzeri](app-service-mobile-ios-how-to-use-client-library.md)
* [.NET (Windows/Xamarin) 2.0.0 sürümü veya üzeri](app-service-mobile-dotnet-how-to-use-client-library.md)
* [Apache Cordova sürüm 2.0 veya üzeri](app-service-mobile-cordova-how-to-use-client-library.md)

Olmuştur gibi bazı değişiklikleri de uygulamanızı anında iletme bildirimleri kullanın, her platform için belirli kayıt yönergeleri not yaparsa.

Yeni istemci sürümü hazır olduğunda, yükseltilen sunucu projenizi karşı deneyin. Çalışır durumda olduğunu doğruladıktan sonra müşterilerin uygulamanızı yeni bir sürümü serbest bırakabilirsiniz. Sonuç olarak, müşterilerinizin bu güncelleştirmeleri almak için bir fırsat çalıştırılmış sonra uygulamanızın Mobile Services sürümü silebilirsiniz. Bu noktada, bir App Service mobil en son mobil uygulamalar sunucusu SDK'sı kullanarak uygulama için tamamen yükselttiniz.

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
[App Service fiyatlandırması]: https://azure.microsoft.com/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Kimlik doğrulaması kavramları]: ../app-service/overview-authentication-authorization.md
[Kimlik doğrulama-hızlı başlangıç]: app-service-mobile-auth.md

[Azure Portal]: https://portal.azure.com/
[OData]: https://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: https://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: https://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
