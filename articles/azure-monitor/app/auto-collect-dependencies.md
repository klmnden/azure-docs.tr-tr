---
title: Azure Application Insights - bağımlılık otomatik toplama | Microsoft Docs
description: Application ınsights'ı otomatik olarak toplamak ve bağımlılıkları Görselleştirme
services: application-insights
documentationcenter: .net
author: nikmd23
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 04/29/2019
ms.reviewer: mbullwin
ms.author: nimolnar
ms.openlocfilehash: 832f927f81b57fd16c202b855d8f1dbe0617ad56
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65149893"
---
# <a name="dependency-auto-collection"></a>Bağımlılık otomatik toplama

Uygulama kodlarınızdaki ek değişiklik gerektirmeden bağımlılıkları olarak otomatik olarak algılanır bağımlılık çağrıları şu anda desteklenen listesi aşağıdadır. Bu uygulama çerçeveleri ve sunucuları gelen çağrıların yanı sıra, iletişim kitaplıkları, depolama istemcileri, günlük ve ölçüm kitaplıkları giden çağrıları oluşur. Bu bağımlılıklar, Application Insights'ta görselleştirilmiştir [Uygulama Haritası](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) ve [işlem tanılamaları](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics) görünümleri. Bağımlılık listede değilse, yine de bunu el ile izleyebilirsiniz bir [izlemek bağımlılık çağrısının](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency).

## <a name="net"></a>.NET

| Uygulama çerçeveleri| Sürümler |
| ------------------------|----------|
| ASP.NET Webforms | 4.5+ |
| ASP.NET MVC | 4+ |
| ASP.NET Webapı | 4.5+ |
| ASP.NET Çekirdeği | 1.1+ |
| <b> İletişim kitaplıkları</b> |
| [HttpClient](https://www.microsoft.com/net/) | 4.5+, .NET Core 1.1+ |
| [SqlClient](https://www.nuget.org/packages/System.Data.SqlClient) | .NET Core 1.0+, NuGet 4.3.0 |
| [EventHubs istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | 1.1.0 |
| [Service Bus istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) | 3.0.0 |
| <b>Depolama istemcileri</b>|  |
| ADO.NET | 4.5+ |
| <b>Günlüğe kaydetme kitaplıkları</b> |  |
| ILogger | 1.1+ |
| System.Diagnostics.Trace | 4.5+ |
| [nLog](https://www.nuget.org/packages/NLog/) | 4.4.12+ |
| [log4net](https://www.nuget.org/packages/log4net/) | NetStandard 1.3, .NET 4.5 + üzerinde 2.0.6+ üzerinde 2.0.8+ |

## <a name="java"></a>Java
| Uygulama sunucuları | Sürümler |
|-------------|----------|
| [Tomcat](https://tomcat.apache.org/) | 7, 8 | 
| [JBoss EAP](https://developers.redhat.com/products/eap/download/) | 6, 7 |
| [Jetty](https://www.eclipse.org/jetty/) | 9 |
| <b>Uygulama çerçeveleri </b> |  |
| [Spring](https://spring.io/) | 3.0 |
| [Spring Boot](https://spring.io/projects/spring-boot) | 1.5.9+<sup>*</sup> |
| Java Servlet | 3.1+ |
| <b>İletişim kitaplıkları</b> |  |
| [Apache Http Client](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) | 4.3+<sup>†</sup> |
| <b>Depolama istemcileri</b> | |
| [SQL Server]( https://mvnrepository.com/artifact/com.microsoft.sqlserver/mssql-jdbc) | 1+<sup>†</sup> |
| [PostgreSQL (Beta desteği)](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/CHANGELOG.md#version-240-beta) | |
| [Oracle]( https://www.oracle.com/technetwork/database/application-development/jdbc/downloads/index.html) | 1+<sup>†</sup> |
| [MySql]( https://mvnrepository.com/artifact/mysql/mysql-connector-java) | 1+<sup>†</sup> |
| <b>Günlüğe kaydetme kitaplıkları</b> | |
| [Logback](https://logback.qos.ch/) | 1+ |
| [Log4j](https://logging.apache.org/log4j/) | 1.2+ |
| <b>Ölçümleri kitaplıkları</b> |  |
| JMX | 1.0+ |

> [!NOTE]
> * Dışında reaktif ölçeklenebilirliğinden desteği.
> <br>†Requires yüklenmesini [JVM aracı](https://docs.microsoft.com/azure/application-insights/app-insights-java-agent#install-the-application-insights-agent-for-java).

## <a name="nodejs"></a>Node.js

| İletişim kitaplıkları | Sürümler |
| ------------------------|----------|
| [HTTP](https://nodejs.org/api/http.html), [HTTPS](https://nodejs.org/api/https.html) | 0.10+ |
| <b>Depolama istemcileri</b> | |
| [Redis](https://www.npmjs.com/package/redis) | 2.x |
| [MongoDb](https://www.npmjs.com/package/mongodb); [MongoDb çekirdek](https://www.npmjs.com/package/mongodb-core) | 2.x - 3.x |
| [MySQL](https://www.npmjs.com/package/mysql) | 2.0.0 - 2.16.x |
| [PostgreSql](https://www.npmjs.com/package/pg); | 6.x - 7.x |
| [PG-pool](https://www.npmjs.com/package/pg-pool) | 1.x - 2.x |
| <b>Günlüğe kaydetme kitaplıkları</b> | |
| [Konsolu](https://nodejs.org/api/console.html) | 0.10+ |
| [Bunyan](https://www.npmjs.com/package/bunyan) | 1.x |
| [Winston](https://www.npmjs.com/package/winston) | 2.x - 3.x |

## <a name="javascript"></a>JavaScript

| İletişim kitaplıkları | Sürümler |
| ------------------------|----------|
| [XMLHttpRequest](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) | Tümü |

## <a name="next-steps"></a>Sonraki adımlar

- Özel bağımlılık izleme ayarlama [.NET](../../azure-monitor/app/asp-net-dependencies.md).
- Özel bağımlılık izleme ayarlama [Java](../../azure-monitor/app/java-agent.md).
- [Özel bağımlılık telemetri yazma](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- Bkz: [veri modeli](../../azure-monitor/app/data-model.md) için Application Insights türleri ve veri modeli.
- Kullanıma [platformları](../../azure-monitor/app/platforms.md) Application Insights tarafından desteklenir.
