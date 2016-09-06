<properties
    pageTitle="Windows Phone ve Mağaza uygulamaları analizi | Microsoft Azure"
    description="Windows cihazı uygulamanızın kullanımını ve kilitlenmesini analiz edin."
    services="application-insights"
    documentationCenter="windows"
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

# Windows Phone ve Mağazası uygulamaları analizi

Microsoft, devOps cihazı için iki çözüm sağlar: [HockeyApp](http://hockeyapp.net/); istemci tarafı analiz için ve [Application Insights](app-insights-overview.md); sunucu tarafı için.

[HockeyApp](http://hockeyapp.net/) Mobil DevOps çözümümüzdür; iOS, OS X, Android veya Windows cihaz uygulamalarının yanı sıra Xamarin, Cordova ve Unity tabanlı platformlar arası uygulamalar için de kullanılır. Bununla, derlemeleri beta test edenlere dağıtabilir, kilitlenme verilerini toplayabilir ve kullanıcılara ölçüm ve geri bildirim sağlayabilirsiniz. Visual Studio Team Services ile tümleşerek kolay derleme dağıtımları ve çalışma öğesi tümleştirmeyi etkinleştirir. 

## HockeyApp kullanmaya başlama

Şuraya gidin:

* [HockeyApp](http://support.hockeyapp.net/kb)
* [Windows için HockeyApp](http://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone)
* [HockeyApp Blogu](http://hockeyapp.net/blog/)
* Sürümleri erken almak için [Hockeyapp Preseason](http://hockeyapp.net/preseason/)’a katılın.

Uygulamanız sunucu tarafındaysa, [ASP.NET](app-insights-asp-net.md) veya [J2EE](app-insights-java-get-started.md) üzerindeki uygulamanızın web sunucusu tarafını izlemek için [Application Insights](app-insights-overview.md)’ı kullanın. 


[Windows Masaüstü uygulamaları için Application Insights](app-insights-windows-desktop.md)’ı da kullanabilirsiniz.

## HockeyApp verileri ile analiz, dışarı aktarma ve API erişimi 

Application Insights’ta [HockeyApp köprüsü oluşturun](app-insights-hockeyapp-bridge-app.md). Şunları yapmanızı sağlar:

* Telemetriniz üzerinden güçlü [Analytics](app-insights-analytics.md) sorgu dilini kullanın. 
* Azure blob depolama alanına [telemetri aktarın](app-insights-export-telemetry.md).

## Sonraki adımlar

* [Windows için HockeyApp kullanmaya başlama](http://support.hockeyapp.net/kb/client-integration-windows-and-windows-phone)



<!--HONumber=ago16_HO5-->


