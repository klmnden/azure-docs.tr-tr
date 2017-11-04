---
title: "Azure Application ınsights'ta Java web uygulamaları için performans izleme | Microsoft Docs"
description: "Genişletilmiş performans ve Application Insights ile Java Web sitenizin kullanım izleme."
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: ecfcf7a3b3698435f98b74474d0ca7223ab2b46c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Bağımlılıklar, özel durumlar ve yürütme sürelerini Java web uygulamalarını izleme


Varsa [Java web uygulamanıza Application Insights ile işaretlenir][java], kod değişiklikleri olmadan daha ayrıntılı Öngörüler almak için Java Agent kullanabilirsiniz:

* **Bağımlılıklar:** verileri de dahil olmak üzere diğer bileşenler için uygulamanızın yaptığı çağrıları hakkında:
  * **REST çağrılarını** HttpClient, OkHttp ve RestTemplate (yay) yapılır.
  * **Redis** Jedis istemcisi üzerinden yapılan çağrıları. Çağrı 10'luk uzun sürerse, aracı ayrıca çağrı bağımsız getirir.
  * **[JDBC çağrıları](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB veya Apache Derby DB. "executeBatch" çağrıları desteklenir. Çağrı 10'luk uzun sürerse, MySQL ve PostgreSQL için aracı sorgu planı bildirir.
* **Özel durum yakalandı:** kodunuz tarafından işlenen özel durumlar hakkında veri.
* **Yöntem yürütme süresi:** belirli yöntemleri yürütme süresini hakkındaki verileri.

Java Aracısı'nı kullanmak için sunucunuzda yüklemeniz gerekir. Web uygulamalarınızı ile işaretlenir gerekir [Application Insights Java SDK'sı][java]. 

## <a name="install-the-application-insights-agent-for-java"></a>Java için Application Insights aracısı yükleyin
1. Java sunucunuz makinede çalışan [Aracısı'nı indirme](https://aka.ms/aijavasdk).
2. Uygulama sunucusu başlangıç komut dosyasını düzenleyin ve aşağıdaki JVM ekleyin:
   
    `javaagent:`*Aracı JAR dosyasının tam yolu*
   
    Örneğin, Tomcat'te bir Linux makinesinde:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Uygulama sunucunuzu yeniden başlatın.

## <a name="configure-the-agent"></a>Aracısı'nı yapılandırma
Adlı bir dosya oluşturun `AI-Agent.xml` ve aracı JAR dosyasını aynı klasöre yerleştirin.

Xml dosyasının içeriğini ayarlarsınız. İstediğiniz dahil etmek veya özellikleri atlamak için aşağıdaki örnek düzenleyin.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

Raporları özel durumu ve yöntemi zamanlama tek tek yöntemleri için etkinleştirmeniz gerekir.

Varsayılan olarak, `reportExecutionTime` geçerlidir ve `reportCaughtExceptions` false olur.

## <a name="view-the-data"></a>Verileri görüntüleme
Application Insights kaynağını toplanmış uzak bağımlılık ve yöntemi yürütme sürelerinin görünür [performans bölmesi altında][metrics].

Bağımlılık, özel durum ve yöntemi raporları tek tek örneklerini aramak için açık [arama][diagnostic].

[Tanılama bağımlılık sorunları - daha fazla bilgi](app-insights-asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Sorularınız mı var? Sorunlarınız mı var?
* Veri yok mu? [Set güvenlik duvarı özel durumlar](app-insights-ip-addresses.md)
* [Java Sorun Giderme](app-insights-java-troubleshoot.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
