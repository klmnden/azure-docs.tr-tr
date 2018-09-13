---
title: Azure Application ınsights Java web uygulamaları için performans izleme | Microsoft Docs
description: Performans ve kullanım izleme Application Insights ile Java Web sitenizi genişletilmiş.
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 08/24/2016
ms.author: mbullwin
ms.openlocfilehash: 366e79e7a58f45f5a5eeb318d3dd08427fbec0b0
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647149"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Bağımlılıklar, yakalanan özel durumların ve yöntemi yürütme sürelerini Java web uygulamalarını izleme


Varsa [Java web uygulamanızı Application Insights ile izleme eklenmiş][java], hiçbir kod değişikliği yapmadan daha ayrıntılı Öngörüler almak için Java aracı kullanabilirsiniz:

* **Bağımlılıkları:** dahil olmak üzere diğer bileşenler için uygulamanızın yaptığı çağrılar hakkında veri:
  * **REST çağrılarını** HttpClient yapılan OkHttp ve RestTemplate (Spring) yakalanır.
  * **Redis** Jedis istemcisi aracılığıyla yapılan çağrılar yakalanır.
  * **[JDBC çağrıları](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server ve Oracle DB komutları otomatik olarak yakalanır. MySQL için sorgu planı aracının rapor göndereceği çağrı 10s uzun sürerse.
* **Özel durum yakalandı:** kodunuz tarafından işlenen özel durumlar hakkında bilgi.
* **Yöntem yürütme süresi:** süresi hakkında bilgi alması belirli bir yöntem yürütülemez için.

Java aracı kullanmak için bunu sunucunuza yüklemeniz gerekir. Web apps ile gerçekleştirilmeyecek [Application Insights Java SDK'sı][java]. 

## <a name="install-the-application-insights-agent-for-java"></a>Java için Application Insights aracıyı yükleme
1. Java sunucunuz makine üzerinde çalışan [aracıyı indirin](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest). Java Agent'ın aynı verson Application Insights Java SDK'sı core ve web paketleri olarak indirmek için lütfen emin olun.
2. Uygulama sunucu başlangıç komut dosyasını düzenleyin ve aşağıdaki JVM ekleyin:
   
    `javaagent:`*Aracı JAR dosyasının tam yolu*
   
    Örneğin, Tomcat'te Linux makinesinde:
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. Uygulama sunucunuzu yeniden başlatın.

## <a name="configure-the-agent"></a>Aracıyı yapılandırın
Adlı bir dosya oluşturun `AI-Agent.xml` ve aracı JAR dosyasını aynı klasöre yerleştirin.

Xml dosyasının içeriği ayarlayın. İstediğiniz dahil etmek veya özelliklerini atlamak için aşağıdaki örnek düzenleyin.

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

Raporlar özel durumu ve her bir yöntem yöntemi zamanlamasını etkinleştirmek zorunda.

Varsayılan olarak, `reportExecutionTime` true'dur ve `reportCaughtExceptions` false'tur.

## <a name="view-the-data"></a>Verileri görüntüleme
Application Insights kaynağını toplanan uzaktan bağımlılık ve Yöntem yürütme sürelerini görünür [performans bölmesi altında][metrics].

Bağımlılık ve özel durum yöntemi raporlar ayrı örneklerini aramak için açık [arama][diagnostic].

[Bağımlılık sorunlarını tanılama - daha fazla bilgi edinin](app-insights-asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Sorularınız mı var? Sorunlarınız mı var?
* Veri yok mu? [Küme güvenlik duvarı özel durumları](app-insights-ip-addresses.md)
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
