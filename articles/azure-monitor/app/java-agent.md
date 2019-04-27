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
ms.openlocfilehash: ce5f7ab1e6751a9ce68aa2d9c466a112c9cac182
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60900617"
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
1. Java sunucunuz makine üzerinde çalışan [aracıyı indirin](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest). Application Insights Java SDK'sı core ve web paketleri aynı sürümde Java aracı, indirmek için lütfen emin olun.
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

## <a name="additional-config-spring-boot"></a>Ek yapılandırma (Spring Boot)

`java -javaagent:/path/to/agent.jar -jar path/to/TestApp.jar`

Azure App Services aşağıdakileri yapmak için:

* Ayarlar > Uygulama Ayarları'nı seçin.
* Uygulama Ayarları'nın altında yeni bir anahtar değer çifti ekleyin:

Anahtar: `JAVA_OPTS` Değer: `-javaagent:D:/home/site/wwwroot/applicationinsights-agent-2.3.1-SNAPSHOT.jar`

Yayınları Java agent en son sürümünü kontrol [burada](https://github.com/Microsoft/ApplicationInsights-Java/releases
). 

D:/home/site/wwwroot sonlanır, aracı, projenizdeki bir kaynak olarak paketlenmesi gerekir/dizin. Giderek aracınızın doğru bir App Service dizininde olduğunu onaylayabilirsiniz **geliştirme araçları** > **Gelişmiş Araçlar** > **hata ayıklama konsolunu**ve site dizinin içeriklerini inceleniyor.    

* Ayarları kaydetmek ve uygulamanızı yeniden başlatın. (Bu adımlar yalnızca uygulama Windows üzerinde çalışan hizmetler için geçerlidir.)

> [!NOTE]
> Yapay ZEKA Agent.xml ve aracı jar dosyasını aynı klasörde olmalıdır. Bunlar genellikle birlikte yerleştirilir `/resources` proje klasörü.  

### <a name="spring-rest-template"></a>Spring Rest şablonu

Application Insights'ın başarıyla Spring'in Rest şablonuyla yapılan HTTP çağrıları izleme için sırada Apache HTTP istemcisini kullanımı gereklidir. Varsayılan olarak, Spring'in Rest şablon Apache HTTP istemcisini kullanmak için yapılandırılmamış. Belirterek [HttpComponentsClientHttpRequestfactory](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/client/HttpComponentsClientHttpRequestFactory.html) Spring Rest şablon oluşturucusunun içinde Apache HTTP kullanır.

Spring Fasulye ile bunu ilişkin bir örnek aşağıda verilmiştir. Bu fabrika sınıfının varsayılan ayarları kullanan bir çok basit bir örnektir.

```java
@bean
public ClientHttpRequestFactory httpRequestFactory() {
return new HttpComponentsClientHttpRequestFactory()
}
@Bean(name = 'myRestTemplate')
public RestTemplate dcrAccessRestTemplate() {
    return new RestTemplate(httpRequestFactory())
}
```

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
