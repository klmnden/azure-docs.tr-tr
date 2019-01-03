---
title: 'Application Insights: programlama dilleri, platformlar ve tümleştirmeler| Microsoft Belgeleri'
description: Application Insights için programlama dilleri, platformlar ve tümleştirmeler bulunmaktadır
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 974db106-54ff-4318-9f8b-f7b3a869e536
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/01/2016
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: 3fe57fe0aac4c355b65b55ee37213221eb854a5c
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2019
ms.locfileid: "53976569"
---
# <a name="developer-analytics-languages-platforms-and-integrations"></a>Geliştirici analizleri: programlama dilleri, platformlar ve tümleştirmeler
Bu öğeler, bildiğimiz [Application Insights](app-insights-overview.md) uygulamalardır. Bunlardan bazıları üçüncü taraflar aracılığıyla sunulur.

## <a name="languages---officially-supported-by-application-insights-team"></a>Application Insights ekibi tarafından resmi olarak desteklenen programlama dilleri
* [C#|VB (.NET)](../azure-monitor/app/asp-net.md)
* [Java](../azure-monitor/app/java-get-started.md)
* [JavaScript web sayfaları](../azure-monitor/app/javascript.md)
* [Node.JS](app-insights-nodejs.md)

## <a name="languages---community-supported"></a>Topluluk tarafından desteklenen programlama dilleri
* [F#](https://safe-stack.github.io/docs/template-azure-ai/)
* [PHP](https://github.com/Microsoft/ApplicationInsights-PHP)
* [Python](https://pypi.python.org/pypi/applicationinsights/0.1.0)
* [Ruby](https://rubygems.org/gems/application_insights)
* [Diğer her şey](#projects)

## <a name="platforms-and-frameworks"></a>Platformlar ve altyapıları
* [ASP.NET](../azure-monitor/app/asp-net.md)
* [ASP.NET - zaten canlı olan uygulamalar için](../azure-monitor/app/monitor-performance-live-website-now.md)
* [ASP.NET Core](../azure-monitor/app/asp-net-core.md)
* [Android](app-insights-mobile-center-quickstart.md) (App Center)
* [Android](https://github.com/Microsoft/ApplicationInsights-Android) (App Center)
* [Angular](https://github.com/MarkPieszak/angular-application-insights)
* [Azure App Service](../azure-monitor/app/azure-web-apps.md)
* [Azure Bulut Hizmetleri](../azure-monitor/app/cloudservices.md)&#151;hem web hem de çalışan rolleri dahil
* [Azure İşlevleri](https://github.com/christopheranderson/azure-functions-app-insights-sample)
* [Docker](../azure-monitor/app/docker.md)
* [Glimpse](https://azure.microsoft.com/blog/glimpse-application-insights/)
* [iOS](app-insights-mobile-center-quickstart.md) (App Center)
* [Ionic](https://github.com/SoftwarePioniere/ionic-application-insights)
* [iOS](https://github.com/Microsoft/ApplicationInsights-iOS) (App Center)
* [J2EE](../azure-monitor/app/java-get-started.md)
* [J2EE - zaten canlı olan uygulamalar için](../azure-monitor/app/java-live.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [OSX](https://github.com/Microsoft/ApplicationInsights-OSX)
* [SAFE Yığını](https://safe-stack.github.io/docs/template-azure-ai/)
* [Spring](https://joe.blog.freemansoft.com/2015/12/enabling-microsoft-application-insight.html)
* [Evrensel Windows uygulaması](app-insights-mobile-center-quickstart.md) (App Center)
* [WCF](https://github.com/Microsoft/ApplicationInsights-SDK-Labs/blob/master/WCF/readme.md)
* [Windows masaüstü uygulamaları, hizmetleri ve çalışan rolleri](app-insights-windows-desktop.md)
* [Diğer her şey](#projects)

## <a name="logging-frameworks"></a>Günlük altyapıları
* [Log4Net, NLog veya System.Diagnostics.Trace](../azure-monitor/app/asp-net-trace-logs.md)
* [Java, Log4J veya Logback](../azure-monitor/app/java-trace-logs.md)
* [Semantik Günlük (SLAB)](https://github.com/fidmor89/SLAB_AppInsights) - [Semantik Günlük Uygulama Bloğu](https://msdn.microsoft.com/library/dn440729.aspx) ile tümleşir
* [Bulut tabanlı yük testi](https://blogs.msdn.com/b/visualstudioalm/archive/2015/07/30/getting-application-insights-counters-with-cloud-based-load-testing.aspx)
* [LogStash eklentisi](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Log Analytics](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)
* [Logary](https://www.nuget.org/packages/Logary.Targets.AppInsights/)
* [Logrus](https://github.com/jjcollinge/logrus-appinsights)

## <a name="content-management-systems"></a>İçerik Yönetim Sistemleri
* [Concrete](https://github.com/fidmor89/appInsights-Concrete)
* [Drupal](https://github.com/fidmor89/AppInsights-Drupal)
* [Joomla](https://github.com/fidmor89/AppInsights-Joomla)
* [Orchard](https://azure.microsoft.com/blog/integrating-application-insights-into-a-modular-cms-and-a-multi-tenant-public-saas/preview/)
* [SharePoint](app-insights-sharepoint.md)
* [WordPress](https://wordpress.org/plugins/application-insights/)

## <a name="export-and-data-analysis"></a>Dışarı Aktarma ve Veri Analizi
* [Alooma](https://www.alooma.com/blog/application-insights-amazon-redshift)
* [Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Akış Analizi](app-insights-export-power-bi.md)

## <a name="projects"></a>Kendi SDK'nizi oluşturun
Programlama diliniz veya platformunuz için henüz bir SDK yoksa belki de bir SDK oluşturmanızın zamanı gelmiştir. [GitHub’da Application Insights SDK projesi](https://github.com/Microsoft/AppInsights-Home) altında listelenen mevcut SDK kodlarını gözden geçirin.
