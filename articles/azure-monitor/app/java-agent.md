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
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: mbullwin
ms.openlocfilehash: ceab5152d6dc6db573a7fea8c673157068009ebe
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2019
ms.locfileid: "54228818"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Bağımlılıklar, yakalanan özel durumların ve yöntemi yürütme sürelerini Java web uygulamalarını izleme


Varsa [Java web uygulamanızı Application Insights ile izleme eklenmiş][java], hiçbir kod değişikliği yapmadan daha ayrıntılı Öngörüler almak için Java aracı kullanabilirsiniz:

* **Bağımlılıkları:** Uygulamanız diğer bileşenler için de dahil olmak üzere, yaptığı çağrılar hakkında veri:
  * **REST çağrılarını** HttpClient yapılan OkHttp ve RestTemplate (Spring) yakalanır.
  * **Redis** Jedis istemcisi aracılığıyla yapılan çağrılar yakalanır.
  * **[JDBC çağrıları](https://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)**  -MySQL, SQL Server ve Oracle DB komutları otomatik olarak yakalanır. MySQL için sorgu planı aracının rapor göndereceği çağrı 10s uzun sürerse.
* **Yakalanan özel durumlar:** Kodunuz tarafından işlenen özel durumlar hakkında bilgiler.
* **Yöntem yürütme süresi:** Belirli bir yöntem yürütülemez kadar sürdüğünü süresi hakkında bilgi.

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

### <a name="spring-boot-agent-additional-config"></a>Yay önyükleme aracı ek yapılandırma

`java -javaagent:/path/to/agent.jar -jar path/to/TestApp.jar`

> [!NOTE]
> Yapay ZEKA Agent.xml ve aracı jar dosyasını aynı klasörde olmalıdır. Bunlar genellikle birlikte yerleştirilir `/resources` proje klasörü. 

#### <a name="enable-w3c-distributed-tracing"></a>W3C dağıtılmış izlemeyi etkinleştirme

AI için aşağıdakileri ekleyin-Agent.xml:

```xml
<Instrumentation>
        <BuiltIn enabled="true">
            <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
        </BuiltIn>
    </Instrumentation>
```

> [!NOTE]
> Geriye dönük uyumluluk modu varsayılan olarak etkindir ve enableW3CBackCompat parametresi isteğe bağlıdır ve yalnızca devre dışı bırakmak istediğinizde kullanılmalıdır. 

İdeal olarak tüm hizmetler SDK'ları W3C protokolü destekleyen daha yeni sürüme güncelleştirilip güncelleştirilmediğini olduğunda bu durum olabilir. SDK'ları sürüme W3C desteğiyle olabildiğince çabuk taşımak için önerilir.

Emin olun **hem [gelen](correlation.md#w3c-distributed-tracing) ve giden (aracı) yapılandırmalarını** tam olarak aynıdır.

## <a name="view-the-data"></a>Verileri görüntüleme
Application Insights kaynağını toplanan uzaktan bağımlılık ve Yöntem yürütme sürelerini görünür [performans bölmesi altında][metrics].

Bağımlılık ve özel durum yöntemi raporlar ayrı örneklerini aramak için açık [arama][diagnostic].

[Bağımlılık sorunlarını tanılama - daha fazla bilgi edinin](../../azure-monitor/app/asp-net-dependencies.md#diagnosis).

## <a name="questions-problems"></a>Sorularınız mı var? Sorunlarınız mı var?
* Veri yok mu? [Küme güvenlik duvarı özel durumları](../../azure-monitor/app/ip-addresses.md)
* [Java Sorun Giderme](java-troubleshoot.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
