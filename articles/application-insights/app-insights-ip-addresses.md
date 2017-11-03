---
title: "Application Insights ve günlük analizi tarafından kullanılan IP adresleri | Microsoft Docs"
description: "Application Insights tarafından gerekli sunucu güvenlik duvarı özel durumlar"
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 44d989f8-bae9-40ff-bfd5-8343d3e59358
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: mbullwin
ms.openlocfilehash: 79ead157dc7509f035c491f9a4c4290eb4d70334
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="ip-addresses-used-by-application-insights-and-log-analytics"></a>Application Insights ve günlük analizi tarafından kullanılan IP adresleri
[Azure Application Insights](app-insights-overview.md) hizmeti, IP adreslerinin sayısı kullanır. İzlemekte olduğunuz uygulama güvenlik duvarının arkasında barındırılıyorsa bu adresleri bilmeniz gerekebilir.

> [!NOTE]
> Bu adresler statik olsa da, biz zaman zaman değiştirmek gerekir mümkündür.
> 
> 

## <a name="outgoing-ports"></a>Giden bağlantı noktaları
Application Insights SDK'sı ve/veya Durum İzleyicisi portalına veri göndermeye izin vermek için sunucunuzun güvenlik duvarında bazı giden bağlantı noktalarını açmanız gerekir:

| Amaç | URL | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| Telemetri |DC.Services.VisualStudio.com<br/>DC.applicationinsights.microsoft.com |40.114.241.141<br/>104.45.136.42<br/>40.84.189.107<br/>168.63.242.221<br/>52.167.221.184<br/>52.169.64.244 |443 |
| Canlı ölçümleri akış |RT.Services.VisualStudio.com<br/>RT.applicationinsights.microsoft.com |23.96.28.38<br/>13.92.40.198 |443 |
| İç Telemetri |BREEZE.aimon.applicationinsights.io |52.161.11.71 |443 |

## <a name="status-monitor"></a>Durum İzleyicisi
Durum İzleyicisi'ni yapılandırma - yalnızca değişiklik yaparken gerekir.

| Amaç | URL | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| Yapılandırma |`management.core.windows.net` | |`443` |
| Yapılandırma |`management.azure.com` | |`443` |
| Yapılandırma |`login.windows.net` | |`443` |
| Yapılandırma |`login.microsoftonline.com` | |`443` |
| Yapılandırma |`secure.aadcdn.microsoftonline-p.com` | |`443` |
| Yapılandırma |`auth.gfx.ms` | |`443` |
| Yapılandırma |`login.live.com` | |`443` |
| Yükleme |`packages.nuget.org` | |`443` |

## <a name="hockeyapp"></a>HockeyApp
| Amaç | URL | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| Çökme verileri |Gate.hockeyapp.NET |104.45.136.42 |80, 443 |

## <a name="availability-tests"></a>Kullanılabilirlik testleri
Hangi adreslerinden listesidir [kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md) çalıştırılır. Uygulamanıza web testleri çalıştırmak istediğinizi, ancak web sunucunuzu belirli istemciler hizmet için kısıtlı, test sunucuları bizim kullanılabilirlik gelen trafiğe izin gerekecektir.

(IP adresleri konuma göre gruplandırılır) Bu adreslerden gelen 80 (http) ve gelen trafiği için 443 (https) bağlantı noktalarını açın:

```
AU : Sydney
13.70.83.252
13.75.150.96
13.75.153.9
13.75.158.185
BR : Sao Paulo
191.232.32.122
191.232.172.45
191.232.176.218
191.232.191.225
CH : Zurich
94.245.66.43
94.245.66.44
94.245.66.45
94.245.66.48
FR : Paris
94.245.72.44
94.245.72.45
94.245.72.46
94.245.72.49
94.245.72.52
94.245.72.53
HK : Hong Kong
13.75.121.122
23.99.115.153
23.99.123.38
23.102.232.186
52.175.38.49
52.175.39.103
IE : Dublin
13.74.184.101
13.74.185.160
40.69.200.198
52.164.224.46
52.169.12.203
52.169.14.11
52.169.237.149
52.178.183.105
JP : Kawaguchi
52.243.33.33
52.243.33.141
52.243.35.253
52.243.41.117
NL : Amsterdam
52.174.166.113
52.174.178.96
52.174.31.140
52.174.35.14
52.178.104.23
52.178.109.190
52.178.111.139
52.233.166.221
RU : Moscow
94.245.82.32
94.245.82.33
94.245.82.37
94.245.82.38
51.140.79.229
51.140.84.172
51.140.87.211
51.140.105.74
SE : Stockholm
94.245.78.40
94.245.78.41
94.245.78.42
94.245.78.45
51.141.25.219
51.141.32.101
51.141.35.167
51.141.54.177
SG : Singapore
52.187.29.7
52.187.179.17
52.187.76.248
52.187.43.24
52.163.57.91
52.187.30.120
US : CA-San Jose
104.45.228.236
104.45.237.251
13.64.152.110
13.64.156.54
13.64.232.251
13.64.236.105
13.91.94.59
40.118.131.182
40.83.189.192
40.83.215.122
US : FL-Miami
65.54.78.49
65.54.78.50
65.54.78.51
65.54.78.54
65.54.78.57
65.54.78.58
65.54.78.59
65.54.78.60
52.165.130.58
52.173.142.229
52.173.147.190
52.173.17.41
52.173.204.247
52.173.244.190
52.173.36.222
52.176.1.226
US : IL-Chicago
23.96.247.139
23.96.249.113
52.162.124.242
52.162.126.139
52.162.241.147
52.162.246.222
52.162.247.136
52.237.153.231
52.237.154.216
52.237.156.14
52.237.157.218
52.237.157.37
US : TX-San Antonio
104.210.145.106
13.84.176.24
13.84.49.16
13.85.11.137
13.85.26.248
13.85.69.145
52.171.136.162
52.171.141.253
52.171.57.172
52.171.58.140
US : VA-Ashburn
13.82.218.95
13.90.96.71
13.90.98.52
13.92.137.70
40.85.187.235
40.87.61.61
52.168.8.247
52.170.38.79
52.170.80.61
52.179.9.26

```  

## <a name="application-insights-api"></a>Application Insights API'si
| Amaç | URI | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| API |api.applicationinsights.io<br/>api1.applicationinsights.io<br/>api2.applicationinsights.io<br/>api3.applicationinsights.io<br/>api4.applicationinsights.io<br/>api5.applicationinsights.io |13.82.26.252<br/>40.76.213.73 |80,443 |
| API belgeleri |dev.applicationinsights.io<br/>dev.applicationinsights.microsoft.com<br/>dev.aisvc.VisualStudio.com<br/>www.applicationinsights.io<br/>www.applicationinsights.microsoft.com<br/>www.aisvc.VisualStudio.com |13.82.24.149<br/>40.114.82.10 |80,443 |
| İç API |aigs.aisvc.VisualStudio.com<br/>aigs1.aisvc.VisualStudio.com<br/>aigs2.aisvc.VisualStudio.com<br/>aigs3.aisvc.VisualStudio.com<br/>aigs4.aisvc.VisualStudio.com<br/>aigs5.aisvc.VisualStudio.com<br/>aigs6.aisvc.VisualStudio.com |dinamik|443 |

## <a name="log-analytics-api"></a>Günlük analizi API
| Amaç | URI | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| API |api.loganalytics.io<br/>*. api.loganalytics.io |dinamik |80,443 |
| API belgeleri |dev.loganalytics.io<br/>docs.loganalytics.io<br/>www.loganalytics.io |dinamik |80,443 |

## <a name="application-insights-analytics"></a>Uygulama Öngörüler analizi

| Amaç | URI | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| Analytics portalı | Analytics.applicationinsights.io | dinamik | 80,443 |
| CDN | applicationanalytics.azureedge.NET | dinamik | 80,443 |
| Medya CDN | applicationanalyticsmedia.azureedge.NET | dinamik | 80,443 |

Not: *. Application Insights ekibi tarafından applicationinsights.io etki alanına ait.

## <a name="log-analytics-portal"></a>Günlük analizi portalında

| Amaç | URI | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| Portal | Portal.loganalytics.io | dinamik | 80,443 |
| CDN | applicationanalytics.azureedge.NET | dinamik | 80,443 |

Not: *. loganalytics.io etki alanı günlük analizi ekibi tarafından ait.

## <a name="application-insights-azure-portal-extension"></a>Application Insights Azure portalı uzantısı

| Amaç | URI | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| Uygulama Öngörüler uzantısı | stamp2.app.insightsportal.VisualStudio.com | dinamik | 80,443 |
| Uygulama Insights uzantısını CDN | insightsportal prod2 cdn.aisvc.visualstudio.com<br/>prod2 asiae cdn.aisvc.visualstudio.com insightsportal<br/>insightsportal cdn aimon.applicationinsights.io | dinamik | 80,443 |

## <a name="application-insights-sdks"></a>Uygulama Insights SDK'ları

| Amaç | URI | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| Application Insights JS SDK CDN | az416426.VO.msecnd.NET | dinamik | 80,443 |
| Uygulama Öngörüler Java SDK'sı | aijavasdk.BLOB.Core.Windows.NET | dinamik | 80,443 |

## <a name="profiler"></a>Profil Oluşturucu

| Amaç | URI | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| Aracı | Agent.azureserviceprofiler.NET<br/>*. agent.azureserviceprofiler.net | dinamik | 443
| Portal | Gateway.azureserviceprofiler.NET | dinamik | 443
| Depolama | *. core.windows.net | dinamik | 443

## <a name="snapshot-debugger"></a>Anlık Görüntü Hata Ayıklayıcı

| Amaç | URI | IP | Bağlantı Noktaları |
| --- | --- | --- | --- |
| Aracı | ppe.azureserviceprofiler.NET<br/>*. ppe.azureserviceprofiler.net | dinamik | 443
| Portal | ppe.Gateway.azureserviceprofiler.NET | dinamik | 443
| Depolama | *. core.windows.net | dinamik | 443
