---
title: Azure Application Insights - bağımlılık otomatik toplama | Microsoft Docs
description: Application ınsights'ı otomatik olarak toplamak ve bağımlılıkları Görselleştirme
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 10/16/2018
ms.author: mbullwin
ms.openlocfilehash: 3ad2f4788a765366066023724772f5432d0d56eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60699214"
---
# <a name="application-insights-nuget-packages"></a>Application Insights NuGet paketleri

Application Insights için NuGet paketlerini kararlı sürüm geçerli listesi aşağıdadır.

## <a name="common-packages-for-aspnet"></a>ASP.NET için ortak paketleri

| Paket adı | Kararlı bir sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights | 2.8.0 | Diğer tüm Application Insights paketleri için bağımlı bir paket aktarım tüm Application Insights Telemetri türleri için çekirdek işlevselliği sunan ve | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
|Microsoft.ApplicationInsights.Agent.Intercept | 2.4.0 | Yöntem çağrılarının durdurma sağlar | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent.Intercept/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.8.0 | .NET uygulamaları için Application Insights bağımlılık Toplayıcı. Bu Application Insights platforma özel paketler için bağımlı bir paket ve bağımlılık telemetrisi otomatik olarak toplanmasını sağlar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.8.0 | Application Insights performans sayaçları Toplayıcı, Application Insights için toplanan performans sayaçlarını tarafından veri göndermesini sağlar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.Web | 2.8.0 | .NET için Application Insights web uygulamaları | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.8.0 | Application Insights Windows Server NuGet paketini otomatik olarak .NET uygulamaları için application ınsights telemetrisi toplama sağlar. Bu paket veya tek başına paketin platforma özel paketler (like .NET çalışan rolleri için) tarafından kapsanmayan .NET uygulamaları için Application Insights platforma özel paketler için bağımlı bir paket olarak kullanılabilir. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.8.0 | Çevrimdışı senaryolarda telemetri korur Application Insights Windows Server SDK telemetri kanal sağlar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="common-packages-for-aspnet-core"></a>ASP.NET Core için ortak paketleri

| Paket adı | Kararlı bir sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AspNetCore | 2.5.0 | ASP.NET Core için Application Insights web uygulamaları. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) |
| Microsoft.ApplicationInsights | 2.8.0 | Bu paket, tüm Application Insights Telemetri türlerini iletimini için temel işlevleri sağlar ve diğer tüm Application Insights paketleri için bağımlı bir paket | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.8.0 | .NET uygulamaları için Application Insights bağımlılık Toplayıcı. Bu Application Insights platforma özel paketler için bağımlı bir paket ve bağımlılık telemetrisi otomatik olarak toplanmasını sağlar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.8.0 | Application Insights performans sayaçları Toplayıcı, Application Insights için toplanan performans sayaçlarını tarafından veri göndermesini sağlar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.8.0 | Application Insights Windows Server NuGet paketini otomatik olarak .NET uygulamaları için application ınsights telemetrisi toplama sağlar. Bu paket veya tek başına paketin platforma özel paketler (like .NET çalışan rolleri için) tarafından kapsanmayan .NET uygulamaları için Application Insights platforma özel paketler için bağımlı bir paket olarak kullanılabilir. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  |
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.8.0 | Çevrimdışı senaryolarda telemetri korur Application Insights Windows Server SDK telemetri kanal sağlar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="listenerscollectorsappenders"></a>Dinleyicileri/toplayıcıları/appenders

| Paket adı | Kararlı bir sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.DiagnosticSourceListener | 2.7.2 |  Application ınsights'ı DiagnosticSource olayları iletme sağlar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/) |
| Microsoft.ApplicationInsights.EventSourceListener | 2.7.2 | Application Insights EventSourceListener veri EventSource olaylarını Application Insights'a gönderme sağlar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/) |
| Microsoft.ApplicationInsights.EtwCollector | 2.7.2 | Application Insights EtwCollector veri olay izleme için Windows (ETW gelen) Application Insights'a gönderme sağlar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/) |
| Microsoft.ApplicationInsights.TraceListener | 2.7.2 | Application Insights izleme günlüğü iletileri göndermenize olanak sağlayan özel TraceListener. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/) |
| Microsoft.ApplicationInsights.Log4NetAppender | 2.7.2 | Özel bir ekleyici Log4Net günlük iletilerini Application Insights'a gönderme etmenize imkan sağlar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
| Microsoft.ApplicationInsights.NLogTarget | 2.7.2 |  Application Insights NLog günlük iletileri göndermenize olanak sağlayan özel bir hedef. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
| Microsoft.applicationınsights.snapshotcollectya da | 1.3.1 | Uygulamanızdaki özel durumları izler ve çevrimdışı analiz için anlık görüntüleri otomatik olarak toplar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector/)

## <a name="service-fabric"></a>Service Fabric

| Paket adı | Kararlı bir sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.ServiceFabric | 2.2.0 | Bu paket, uygulamayı service fabric bağlamla telemetri otomatik düzenleme çalıştığı sağlar. Bu NuGet yerel Service Fabric uygulamaları için kullanmayın. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric/) |
| Microsoft.ApplicationInsights.ServiceFabric.Native | 2.2.0 | Service fabric uygulamaları için Application Insights modülü. Bu NuGet yalnızca yerel Service Fabric uygulamaları için kullanın. Kapsayıcılarda çalıştırılan uygulamalar için Microsoft.ApplicationInsights.ServiceFabric paketi kullanın. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric.Native/) |  

## <a name="status-monitor"></a>Durum İzleyicisi

| Paket adı | Kararlı bir sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.Agent_x64 | 2.2.1 |  Çalışma zamanı x64 için veri toplamayı etkinleştirir uygulamalar | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x64/) |
| Microsoft.ApplicationInsights.Agent_x86 | 2.2.1 |  Çalışma zamanı x86 için veri toplamayı etkinleştirir uygulamalar. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x86/) |

Bu paketler, çalışma zamanını izleme çekirdek işlevselliğini parçası oluşturan [Durum İzleyicisi](../../azure-monitor/app/monitor-performance-live-website-now.md). Bu paketleri doğrudan karşıdan yüklemek için yalnızca Durum İzleyicisi yükleyici kullanın gerekmez. Daha fazla nasıl bu paketler hakkında bileşenler bu işe anlamak istiyorsanız [blog gönderisi](https://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/) geliştiricilerimiz birinden iyi bir başlangıç noktasıdır.

## <a name="additional-packages"></a>Ek paketleri

| Paket adı | Kararlı bir sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AzureWebSites | 2.6.5 | Bu uzantı, bir Azure App Service Application Insights izleme sağlar. SDK'sı sürüm 2.6.1. Yönergeler: Uygulama ayarları, ikey 'Appınsıghts_ınstrumentatıonkey' ekleyin ve webapp etkili olması için yeniden başlatın.| [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AzureWebSites/) |
| Microsoft.ApplicationInsights.Injector | 2.6.7 | Bu paket Kodsuz Application Insights ekleme işlemi için gereken dosyaları içerir. | [Paketini indirme](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Injector/) |

## <a name="next-steps"></a>Sonraki adımlar

- İzleyici [ASP.NET Core](../../azure-monitor/app/asp-net-core.md).
- ASP.NET Core profil [Azure Linux web app'ler](../../azure-monitor/app/profiler-aspnetcore-linux.md).
- ASP.NET hata ayıklama [anlık görüntüleri](../../azure-monitor/app/snapshot-debugger.md).