<properties
    pageTitle="Azure App Service Mobile Apps’de Cordova uygulaması oluşturma | Microsoft Azure"
    description="Apache Cordova geliştirme için Azure mobil uygulaması arka uçlarını kullanmaya başlamak için bu öğreticiden yararlanın."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova,javascript,mobil,istemci" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/11/2016"
    ms.author="glenga"/>

#Apache Cordova uygulaması oluşturma

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## Genel Bakış

Bu öğreticide, bir Apache Cordova mobil uygulamasına Azure mobil uygulaması arka ucunu kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir.  Yeni bir mobil uygulama arka ucu ve uygulama verilerini Azure’da depolayan basit bir _Yapılacaklar listesi_ Apache Cordova uygulaması oluşturacaksınız.

Bu öğreticiyi tamamlamak, Azure App Service’de Mobile Apps özelliğini kullanmayla ilgili diğer tüm Apache Cordova öğreticileri için ön koşuldur.

## Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Visual Studio Community 2015] ya da daha yeni sürümünü içeren bir bilgisayar.
* [Apache Cordova için Visual Studio Araçları].
* [Etkin bir Azure hesabı](https://azure.microsoft.com/pricing/free-trial/).

Ayrıca Visual Studio’yu atlayabilir ve doğrudan Apache Cordova komut satırını kullanabilirsiniz.  Bu, öğreticiyi bir Mac bilgisayarda tamamladığınızda kullanışlıdır.  Komut satırını kullanarak Apache Cordova istemci uygulamalarını derleme bu öğretici kapsamında değildir.

## Yeni bir Azure mobil uygulama arka ucu oluşturma

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Benzer adımları gösteren bir video izleyin](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## Sunucu projesi yapılandırma

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## Apache Cordova uygulamasını indirme ve çalıştırma

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## Sonraki Adımlar

Bu hızlı başlangıç öğreticisini tamamladığınıza göre, şu eğitimlerden birine geçin:

* Apache Cordova uygulamanıza [Kimlik Doğrulaması Ekleme].
* Apache Cordova uygulamanıza [Anında İletme Bildirimleri Ekleme].

Azure App Service temel kavramları hakkında daha fazla bilgi edinin.

* [Kimlik Doğrulaması]
* [Anında İletme Bildirimleri ]

SDK'ları kullanmayı öğrenin.

* [Apache Cordova SDK]
* [ASP.NET Sunucusu SDK]
* [Node.js Sunucusu SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portalına]: https://portal.azure.com/
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Apache Cordova için Visual Studio Araçları]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Kimlik Doğrulaması Ekleme]: app-service-mobile-cordova-get-started-users.md
[Anında İletme Bildirimleri Ekleme]: app-service-mobile-cordova-get-started-push.md
[Kimlik Doğrulaması]: app-service-mobile-auth.md
[Anında İletme Bildirimleri ]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Sunucusu SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Sunucusu SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md



<!--HONumber=sep16_HO1-->


