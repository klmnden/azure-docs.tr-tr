<properties
    pageTitle="iOS uygulamaları için analiz | Microsoft Azure"
    description="iOS uygulamanızın kullanımını ve performansını analiz edin."
    services="application-insights"
    documentationCenter="ios"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/26/2016"
    ms.author="awills"/>

# iOS uygulamaları analizi

Microsoft, devOps cihazı için iki çözüm sağlar: [HockeyApp](http://hockeyapp.net/); istemci cihazları için ve [Application Insights](app-insights-overview.md); sunucu tarafı ve istemci web sayfaları için.

[HockeyApp](http://hockeyapp.net/) Mobil DevOps çözümümüzdür; iOS, OS X, Android veya Windows cihaz uygulamaları derlemesinin yanı sıra Xamarin, Cordova ve Unity tabanlı platformlar arası uygulamalar için de kullanılır. Bununla, derlemeleri beta test edenlere dağıtabilir, kilitlenme verilerini toplayabilir ve kullanıcılara geri bildirim sağlayabilirsiniz. Visual Studio Team Services ile tümleşerek kolay derleme dağıtımları ve çalışma öğesi tümleştirmeyi etkinleştirir. 


Şuraya gidin:

* [HockeyApp](http://support.hockeyapp.net/kb)
* [iOS için HockeyApp](http://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)
* [HockeyApp Blogu](http://hockeyapp.net/blog/)
* Sürümleri erken almak için [Hockeyapp Preseason](http://hockeyapp.net/preseason/)’a katılın.

Uygulamanız sunucu tarafındaysa, [ASP.NET](app-insights-asp-net.md) veya [J2EE](app-insights-java-get-started.md) üzerindeki uygulamanızın web sunucusu tarafını izlemek için [Application Insights](app-insights-overview.md)’ı kullanın. 


## HockeyApp verileri ile analiz, dışarı aktarma ve API erişimi 

Application Insights’ta [HockeyApp köprüsü oluşturun](app-insights-hockeyapp-bridge-app.md). Şunları yapmanızı sağlar:

* Telemetriniz üzerinden güçlü [Analytics](app-insights-analytics.md) sorgu dilini kullanın. 
* Azure blob depolama alanına [telemetri aktarın](app-insights-export-telemetry.md).

## Sonraki adımlar

* [iOS için HockeyApp kullanmaya başlama](http://support.hockeyapp.net/kb/client-integration-ios-mac-os-x-tvos/hockeyapp-for-ios)




<!--HONumber=ago16_HO5-->


