---
title: Azure Application Insights Java SDK ile Micrometer kullanma | Microsoft Docs
description: 'Application Insights Spring Boot ve Spring Boot uygulamalarınızla Micrometer kullanma hakkında adım adım kılavuz. '
services: application-insights
documentationcenter: java
author: lgayhardt
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: lagayhar
ms.openlocfilehash: 778690fb2796cea3154b3acbb662341fdaea87da
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60699147"
---
# <a name="how-to-use-micrometer-with-azure-application-insights-java-sdk"></a>Azure Application Insights Java SDK ile Micrometer kullanma
Micrometer uygulama izleme ölçüler ölçümleri JVM tabanlı bir uygulama için kod ve izleme sistemlerinden sevdiğiniz veri dışa aktarmanızı sağlar. Bu makalede Spring Boot ve Spring Boot uygulamaları için Application Insights ile Micrometer kullanmayı öğretir.

## <a name="using-spring-boot-15x"></a>Spring kullanarak önyükleme 1,5 x
Pom.xml veya build.gradle dosyanıza aşağıdaki bağımlılıkları ekleyin: 
* [Application Insights spring-boot-Başlatıcı](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/azure-application-insights-spring-boot-starter)1.1.0-BETA veya üzeri
* Micrometer Azure kayıt defteri 1.1.0 veya üzeri
* [Micrometer Spring eski](https://micrometer.io/docs/ref/spring/1.5) 1.1.0 veya üzeri (Bu backports Spring framework autoconfig kodda).
* [Applicationınsights kaynak](../../azure-monitor/app/create-new-resource.md )

Adımlar

1. Spring Boot uygulamasının pom.xml dosyasını güncelleştirin ve aşağıdaki bağımlılıkları ekleyebilirsiniz:

    ```XML
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-spring-boot-starter</artifactId>
        <version>1.1.0-BETA</version>
    </dependency>

    <dependency>
        <groupId>io.micrometer</groupId>
        <artifactId>micrometer-spring-legacy</artifactId>
        <version>1.1.0</version>
    </dependency>

    <dependency>
        <groupId>io.micrometer</groupId>
        <artifactId>micrometer-registry-azure-monitor</artifactId>
        <version>1.1.0</version>
    </dependency>

    ```
2. Application.properties veya yml dosyasını aşağıdaki özelliği kullanarak Application Insights izleme anahtarı ile güncelleştirin:

     `azure.application-insights.instrumentation-key=<your-instrumentation-key-here>`
1. Uygulamanızı derleme ve çalıştırma
2. Yukarıdaki, almalısınız ve önceden toplanan ölçümler otomatik ile çalışan Azure İzleyici toplanır. Application Insights Spring Boot Başlatıcı ince ayar yapma hakkında ayrıntılı bilgi için bkz [github'daki Benioku](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).

## <a name="using-spring-2x"></a>Spring 2.x kullanma

Pom.xml veya build.gradle dosyanıza aşağıdaki bağımlılıkları ekleyin:

* Application Insights Spring boot Başlatıcı 2.1.2'yi veya üzeri
* Azure-spring-önyükleme-ölçümlerini-başlangıç 2.0.7 veya üzeri  
* [Application Insights kaynağı](../../azure-monitor/app/create-new-resource.md )

Adımlar:

1. Spring Boot uygulamasının pom.xml dosyasını güncelleştirin ve aşağıdaki bağımlılığı ekleyin:

    ```XML
    <dependency> 
          <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-spring-boot-metrics-starter</artifactId>
          <version>2.0.7</version>
    </dependency>
    ```
1. Application.properties veya yml dosyasını aşağıdaki özelliği kullanarak Application Insights izleme anahtarı ile güncelleştirin:

     `azure.application-insights.instrumentation-key=<your-instrumentation-key-here>`
3. Uygulamanızı derleme ve çalıştırma
4. Yukarıdaki, önceden toplanan ölçümler otomatik toplanan Azure İzleyici ile çalışan almanız gerekir. Application Insights Spring Boot Başlatıcı ince ayar yapma hakkında ayrıntılı bilgi için bkz [github'daki Benioku](https://github.com/Microsoft/azure-spring-boot/releases/latest).

Varsayılan ölçümleri:

*    Otomatik olarak ölçümleri Tomcat, JVM, Logback ölçümleri, çalışma süresi ölçümleri, işlemci ölçümleri FileDescriptorMetrics Log4J ölçümler için yapılandırılır.
*    Örneğin, netflix hystrix sınıfı yolunda mevcut olup olmadığını biz de bu ölçümleri alır. 
*    Aşağıdaki ölçümler, ilgili Fasulye ekleyerek kullanılabilir. 
        - CacheMetrics (CaffeineCache, EhCache2, GuavaCache, HazelcaseCache, Jcache)     
        - DataBaseTableMetrics 
        - HibernateMetrics 
        - JettyMetrics 
        - OkHttp3 ölçümleri 
        - Kafka ölçümleri 

 

Otomatik ölçüm koleksiyonunu devre dışı bırakma nasıl: 
 
- JVM ölçümleri: 
    - management.metrics.binders.jvm.enabled=false 
- Logback ölçümleri: 
    - management.metrics.binders.logback.enabled=false
- Çalışma süresi ölçümleri: 
    - management.metrics.binders.uptime.enabled=false 
- İşlemci ölçümleri:
    -  management.metrics.binders.processor.enabled=false 
- FileDescriptorMetrics:
    - management.metrics.binders.files.enabled=false 
- Hystrix ölçümleri, sınıf kitaplığı: 
    - management.metrics.binders.hystrix.enabled=false 
- AspectJ ölçümleri, sınıf kitaplığı: 
    - spring.aop.enabled=false 

> [!NOTE]
> Spring Boot uygulamasının application.properties veya application.yml dosyasında yukarıda özellikleri belirtin

## <a name="use-micrometer-with-non-spring-boot-web-applications"></a>Spring Boot web uygulamalarıyla Micrometer kullanın

Pom.xml veya build.gradle dosyanıza aşağıdaki bağımlılıkları ekleyin:
 
* [Application Insight çekirdek 2.2.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights/2.2.0) veya üzeri
* [Application Insights Web 2.2.0](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/2.2.0) veya üzeri
* [Kayıt Web filtresi](https://docs.microsoft.com/azure/application-insights/app-insights-java-get-started)
* Micrometer Azure kayıt defteri 1.1.0 veya üzeri
* [Application Insights kaynağı](../../azure-monitor/app/create-new-resource.md )

Adımlar:

1. Pom.xml veya build.gradle dosyanıza aşağıdaki bağımlılıkları ekleyin:

    ```XML
        <dependency>
            <groupId>io.micrometer</groupId>
            <artifactId>micrometer-registry-azure-monitor</artifactId>
            <version>1.1.0</version>
        </dependency>
        
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>applicationinsights-web</artifactId>
            <version>2.2.0</version>
        </dependency
     ```

2. Uygulama Insights.xml kaynaklar klasörüne yerleştirin.

    Örnek Servlet sınıfına (bir zamanlayıcı ölçüm gösterir):

    ```Java
        @WebServlet("/hello")
        public class TimedDemo extends HttpServlet {
    
          private static final long serialVersionUID = -4751096228274971485L;
    
          @Override
          @Timed(value = "hello.world")
          protected void doGet(HttpServletRequest request, HttpServletResponse response)
              throws ServletException, IOException {
    
            response.getWriter().println("Hello World!");
            MeterRegistry registry = (MeterRegistry) getServletContext().getAttribute("AzureMonitorMeterRegistry");
    
        //create new Timer metric
            Timer sampleTimer = registry.timer("timer");
            Stream<Integer> infiniteStream = Stream.iterate(0, i -> i+1);
            infiniteStream.limit(10).forEach(integer -> {
              try {
                Thread.sleep(1000);
                sampleTimer.record(integer, TimeUnit.MILLISECONDS);
              } catch (Exception e) {}
               });
          }
          @Override
          public void init() throws ServletException {
            System.out.println("Servlet " + this.getServletName() + " has started");
          }
          @Override
          public void destroy() {
            System.out.println("Servlet " + this.getServletName() + " has stopped");
          }
    
        }
    
    ```

      Örnek yapılandırma sınıfı:

    ```Java
         @WebListener
         public class MeterRegistryConfiguration implements ServletContextListener {
     
           @Override
           public void contextInitialized(ServletContextEvent servletContextEvent) {
     
         // Create AzureMonitorMeterRegistry
           private final AzureMonitorConfig config = new AzureMonitorConfig() {
             @Override
             public String get(String key) {
                 return null;
             }
            @Override
               public Duration step() {
                 return Duration.ofSeconds(60);}
     
             @Override
             public boolean enabled() {
                 return false;
             }
         };
     
      MeterRegistry azureMeterRegistry = AzureMonitorMeterRegistry.builder(config);
     
             //set the config to be used elsewhere
             servletContextEvent.getServletContext().setAttribute("AzureMonitorMeterRegistry", azureMeterRegistry);
     
           }
     
           @Override
           public void contextDestroyed(ServletContextEvent servletContextEvent) {
     
           }
         }
    ```

Ölçümler hakkında daha fazla bilgi için bkz [Micrometer belgeleri](https://micrometer.io/docs/).

Diğer örnek kod ölçümleri farklı türleri oluşturma konusunda bulunabilir[resmi Micrometer GitHub deposunu](https://github.com/micrometer-metrics/micrometer/tree/master/samples/micrometer-samples-core/src/main/java/io/micrometer/core/samples).

## <a name="how-to-bind-additional-metrics-collection"></a>Ek ölçümler toplama bağlama

### <a name="springbootspring"></a>SpringBoot/Spring

Bir çekirdeği ilgili ölçüm kategorisi oluşturun. Örneğin, Guava ihtiyacımız düşünelim önbellek ölçümlerini:

```Java
    @Bean
    GuavaCacheMetrics guavaCacheMetrics() {
        Return new GuavaCacheMetrics();
    }
```
Varsayılan olarak etkin değildir, ancak yukarıdaki biçimde bağlanabilir birkaç ölçüm vardır. Tam bir listesi için başvurmak [resmi Micrometer GitHub deposunu](https://github.com/micrometer-metrics/micrometer/tree/master/micrometer-core/src/main/java/io/micrometer/core/instrument/binder ).

### <a name="non-spring-apps"></a>Spring olmayan uygulamalar
Aşağıdaki bağlama kodunu yapılandırma dosyasına ekleyin:
```Java 
    New GuavaCacheMetrics().bind(registry);
```

## <a name="next-steps"></a>Sonraki adımlar

* Resmi başvuran Micrometer hakkında daha fazla bilgi edinmek için [Micrometer belgeleri](https://micrometer.io/docs).
* Spring hakkında bilgi edinmek için resmi Azure'da bakın [Azure Belgeleri'nde Spring](https://docs.microsoft.com/java/azure/spring-framework/?view=azure-java-stable).
