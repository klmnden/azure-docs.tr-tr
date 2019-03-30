---
title: Azure Application Insights ile Java web uygulaması analizi | Microsoft Docs
description: 'Application Insights ile Java web uygulamaları için Uygulama Performansı İzleme. '
services: application-insights
documentationcenter: java
author: lgayhardt
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: lagayhar
ms.openlocfilehash: bbf9c162cd52dc94ee820c8597f36f7cbfeace5a
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630750"
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a>Java web projesinde Application Insights ile başlarken


[Application Insights](https://azure.microsoft.com/services/application-insights/), web geliştiricileri için canlı uygulamanızın performansını ve kullanımını anlamanıza yardımcı olan genişletilebilir bir analiz hizmetidir. Bunu kullanın [otomatik olarak gereç istek bağımlılıkları izleme ve toplama performans sayaçları](auto-collect-dependencies.md#java), [performans sorunlarını ve özel durumları tanılama](../../azure-monitor/app/detect-triage-diagnose.md), ve [kod yazma] [ api] uygulamanızla kullanıcıların ne yaptığını izlemek için. 

![Örnek veriler genel bakış görüntüsü](./media/java-get-started/overview-graphs.png)

Application Insights; Linux, Unix veya Windows üzerinde çalışan Java uygulamalarını destekler.

Gerekenler:

* JRE 1.7 veya 1.8 sürümü
* Bir [Microsoft Azure](https://azure.microsoft.com/) aboneliği.

Spring çerçevesini tercih ediyorsanız, [Application Insights kılavuzunu kullanmak için Spring Boot başlatıcı uygulamasını yapılandırmayı](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights) deneyin

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Application Insights izleme anahtarı edinme
1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.
2. Bir Application Insights kaynağı oluşturun. Uygulama türünü Java web uygulaması olarak ayarlayın.

3. Yeni kaynağın izleme anahtarını bulun. Bu anahtarı hemen kod projenize yapıştırmalısınız.

    ![Yeni kaynağa genel bakışta, Özellikler'e tıklayıp izleme anahtarını kopyalama](./media/java-get-started/instrumentation-key-001.png)

## <a name="2-add-the-application-insights-sdk-for-java-to-your-project"></a>2. Projenize Java için Application Insights SDK’sı ekleme
*Projeniz için uygun yolu seçin.*

#### <a name="if-youre-using-maven-a-namemaven-setup-"></a>Maven kullanıyorsanız... <a name="maven-setup" />
Projenizi derleme için zaten Maven kullanmak üzere ayarlanmışsa aşağıdaki kodu pom.xml dosyanızla birleştirin.

Daha sonra, proje bağımlılıklarını ikili dosyaları indirmek için yenileyin.

```XML

    <repositories>
       <repository>
          <id>central</id>
          <name>Central</name>
          <url>http://repo1.maven.org/maven2</url>
       </repository>
    </repositories>

    <dependencies>
      <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-web</artifactId>
        <!-- or applicationinsights-core for bare API -->
        <version>[2.0,)</version>
      </dependency>
    </dependencies>
```

* *Derleme veya sağlama toplamı doğrulama hataları mı var?* `<version>2.0.n</version>` gibi belirli bir sürümü kullanmayı deneyin. En son sürümü [SDK sürüm notlarında](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) veya [Maven yapıtları](https://search.maven.org/#search%7Cga%7C1%7Capplicationinsights) sitesinde bulacaksınız.
* *Yeni SDK’ye mi güncelleştirmeniz gerekiyor?* Proje bağımlılıklarınızı yenileyin.

#### <a name="if-youre-using-gradle-a-namegradle-setup-"></a>Gradle kullanıyorsanız... <a name="gradle-setup" />
Projenizi derleme için zaten Gradle kullanmak üzere ayarlanmışsa aşağıdaki kodu build.gradle dosyanızla birleştirin.

Daha sonra, proje bağımlılıklarını ikili dosyaları indirmek için yenileyin.

```gradle

    repositories {
      mavenCentral()
    }

    dependencies {
      compile group: 'com.microsoft.azure', name: 'applicationinsights-web', version: '2.+'
      // or applicationinsights-core for bare API
    }
```

#### <a name="if-youre-using-eclipse-to-create-a-dynamic-web-project-"></a>Dinamik Web projesi oluşturmak için Eclipse kullanıyorsanız...
[Java eklentisi için Application Insights SDK'sı][eclipse] kullanın. Not: Bu eklentiyi kullanarak Application Insights’ı daha hızlı kullanmaya başlayabilseniz de (Maven/Gradle kullanmadığınız varsayılarak), bu bir bağımlılık yönetim sistemi değildir. Bu nedenle, eklenti güncelleştirildiğinde, projenizdeki Application Insights kitaplıkları otomatik olarak güncelleştirilmez.

* *Derleme veya sağlama toplamı doğrulama hataları mı var?* `version:'2.0.n'` gibi belirli bir sürümü kullanmayı deneyin. En son sürümü [SDK sürüm notlarında](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) veya [Maven yapıtları](https://search.maven.org/#search%7Cga%7C1%7Capplicationinsights) sitesinde bulacaksınız.
* *Yeni bir SDK’ya güncelleştirmek için* Proje bağımlılıklarınızı yenileyin.

#### <a name="otherwise-if-you-are-manually-managing-dependencies-"></a>Aksi takdirde, bağımlılıkları el ile yönetiyorsanız...
[En son sürümü](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest) indirin ve önceki sürümleri değiştirerek gerekli dosyaları projenize kopyalayın.

### <a name="questions"></a>Sorular...
* *`-core` ve `-web` bileşenleri arasındaki ilişki nedir?*
  * `applicationinsights-core` size tam API sağlar. Bu bileşen her zaman gerekecektir.
  * `applicationinsights-web`, HTTP istek sayısını ve yanıt sürelerini izleyen ölçümleri sağlar. Bu telemetrinin otomatik olarak toplanmasını istemiyorsanız, bu bileşeni atlayabilirsiniz. Örneğin, kendiniz yazmak istiyorsanız.
  
* *SDK’yı en son sürüme nasıl güncelleştirmeliyim?*
  * Gradle veya Maven kullanıyorsanız...
    * En son sürümü belirtmek için derleme dosyanızı güncelleştirin veya en son sürümü otomatik olarak dahil etmek için Gradle/Maven’in joker karakter sözdizimini kullanın. Ardından projenizin bağımlılıklarını yenileyin. Joker karakter sözdizimi, [Gradle](#gradle-setup) veya [Maven](#maven-setup) için yukarıdaki örneklerde görülebilir.
  * Bağımlılıkları el ile yönetiyorsanız...
    * En son [Java için Application Insights SDK’si](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest)’ni indirin ve eskilerle değiştirin. Değişiklikler [SDK sürüm notlarında](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) açıklanmıştır.

## <a name="3-add-an-applicationinsightsxml-file"></a>3. ApplicationInsights.xml dosyası ekleme
Projenizin kaynaklar klasörüne ApplicationInsights.xml dosyasını ekleyin veya projenizin dağıtım sınıfı yoluna eklendiğinden emin olun. Aşağıdaki XML dosyasını buraya kopyalayın.

Azure portalından aldığınız izleme anahtarını bununla değiştirin.

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">


      <!-- The key from the portal: -->
      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>


      <!-- HTTP request component (not required for bare API) -->
      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>

      <!-- Events correlation (not required for bare API) -->
      <!-- These initializers add context data to each event -->

      <TelemetryInitializers>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

İsteğe bağlı olarak, yapılandırma dosyası, uygulamanızın erişebildiği herhangi bir konumda bulunabilir.  `-Dapplicationinsights.configurationDirectory` sistem özelliği, ApplicationInsights.xml dosyasını içeren dizini belirtir. Örneğin, `E:\myconfigs\appinsights\ApplicationInsights.xml` konumunda bulunan bir yapılandırma dosyası, `-Dapplicationinsights.configurationDirectory="E:\myconfigs\appinsights"` özelliği ile yapılandırılır.

* İzleme anahtarı telemetrinin her öğesiyle birlikte gönderilir ve Application Insights’ın bunu kaynağınızda görüntülemesini isteyin.
* HTTP isteği bileşeni isteğe bağlıdır. İstek ve yanıt süreleri hakkında telemetriyi otomatik olarak portala gönderir.
* Olay bağıntısı, HTTP isteği bileşenine bir ektir. Sunucu tarafından alınan her istek için bir tanımlayıcı atar ve bu tanımlayıcıyı bir özellik olarak, telemetrinin her öğesine 'Operation.Id' özelliği olarak ekler. [Tanı aramaya][diagnostic] bir filtre ayarlayarak her istekle ilişkili telemetrinin bağıntısını kurmanızı sağlar.

### <a name="alternative-ways-to-set-the-instrumentation-key"></a>İzleme anahtarını ayarlamak için alternatif yollar
Application Insights SDK’sı anahtarı şu sırayla arar:

1. Sistem özelliği: -DAPPLICATION_INSIGHTS_IKEY=your_ikey
2. Ortam değişkeni: APPLICATION_INSIGHTS_IKEY
3. Yapılandırma dosyası: ApplicationInsights.xml

Ayrıca [kod içinde ayarlayabilirsiniz](../../azure-monitor/app/api-custom-events-metrics.md#ikey):

```java
    String instrumentationKey = "00000000-0000-0000-0000-000000000000";

    if (instrumentationKey != null)
    {
        TelemetryConfiguration.getActive().setInstrumentationKey(instrumentationKey);
    }
```

## <a name="4-add-an-http-filter"></a>4. HTTP filtresi ekleme
Son yapılandırma adımı HTTP isteği bileşeninin her web isteğini kaydetmesini sağlar. (Yalnızca tam API istiyorsanız gerekmez.)

### <a name="spring-boot-applications"></a>Spring Boot Uygulamaları
Yapılandırma sınıfınıza Application Insights `WebRequestTrackingFilter` filtresini kaydetme:

```Java
package <yourpackagename>.configurations;

import javax.servlet.Filter;

import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.core.Ordered;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Configuration;
import com.microsoft.applicationinsights.TelemetryConfiguration;
import com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter;

@Configuration
public class AppInsightsConfig {

    @Bean
    public String telemetryConfig() {
        String telemetryKey = System.getenv("<instrumentation key>");
        if (telemetryKey != null) {
            TelemetryConfiguration.getActive().setInstrumentationKey(telemetryKey);
        }
        return telemetryKey;
    }

    /**
     * Programmatically registers a FilterRegistrationBean to register WebRequestTrackingFilter
     * @param webRequestTrackingFilter
     * @return Bean of type {@link FilterRegistrationBean}
     */
    @Bean
    public FilterRegistrationBean webRequestTrackingFilterRegistrationBean(WebRequestTrackingFilter webRequestTrackingFilter) {
        FilterRegistrationBean registration = new FilterRegistrationBean();
        registration.setFilter(webRequestTrackingFilter);
        registration.addUrlPatterns("/*");
        registration.setOrder(Ordered.HIGHEST_PRECEDENCE + 10);
        return registration;
    }


    /**
     * Creates bean of type WebRequestTrackingFilter for request tracking
     * @param applicationName Name of the application to bind filter to
     * @return {@link Bean} of type {@link WebRequestTrackingFilter}
     */
    @Bean
    @ConditionalOnMissingBean

    public WebRequestTrackingFilter webRequestTrackingFilter(@Value("${spring.application.name:application}") String applicationName) {
        return new WebRequestTrackingFilter(applicationName);
    }


}
```

> [!NOTE]
> Spring Boot 1.3.8 veya daha eski bir sürüm kullanıyorsanız, FilterRegistrationBean’i aşağıdaki satırla değiştirin

```Java
    import org.springframework.boot.context.embedded.FilterRegistrationBean;
```

Bu sınıf, `WebRequestTrackingFilter` filtresini, http filtresi zincirinde ilk filtre olacak şekilde yapılandırır. Varsa, işletim sistemi ortam değişkeninden izleme anahtarını çeker.

> Bu bir Spring Boot uygulaması olduğundan ve kendi Spring MVC yapılandırmasını içerdiğinden, Spring MVC yapılandırması yerine web http filtresi yapılandırmasını kullanıyoruz. Spring MVC’ye özgü yapılandırma için aşağıdaki bölüme bakın.

### <a name="applications-using-webxml"></a>Web.xml Kullanan Uygulamalar
Projenizde web.xml dosyasını bulup açın ve uygulama filtrelerinizin yapılandırıldığı web uygulaması düğümü altında aşağıdaki kodu birleştirin.

En doğru sonuçlar almak için önce filtrenin tüm diğer filtrelerle eşlenmesi gerekir.

```XML

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>

   <!-- This listener handles shutting down the TelemetryClient when an application/servlet is undeployed. -->
    <listener>
      <listener-class>com.microsoft.applicationinsights.web.internal.ApplicationInsightsServletContextListener</listener-class>
    </listener>
```

#### <a name="if-youre-using-spring-web-mvc-31-or-later"></a>Spring Web MVC 3.1 veya sonraki sürümleri kullanıyorsanız
Bu öğeleri *-servlet.xml içinde Application Insights paketini içerecek şekilde düzenleyin:

```XML

    <context:component-scan base-package=" com.springapp.mvc, com.microsoft.applicationinsights.web.spring"/>

    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.microsoft.applicationinsights.web.spring.RequestNameHandlerInterceptorAdapter" />
        </mvc:interceptor>
    </mvc:interceptors>
```

#### <a name="if-youre-using-struts-2"></a>Struts 2 kullanıyorsanız
Bu öğeyi Struts yapılandırma dosyasına ekleyin (genellikle struts.xml veya struts default.xml adıyla):

```XML

     <interceptors>
       <interceptor name="ApplicationInsightsRequestNameInterceptor" class="com.microsoft.applicationinsights.web.struts.RequestNameInterceptor" />
     </interceptors>
     <default-interceptor-ref name="ApplicationInsightsRequestNameInterceptor" />
```

Varsayılan yığında tanımlı dinleyiciler varsa, dinleyici o yığına eklenebilir.

## <a name="5-run-your-application"></a>5. Uygulamanızı çalıştırma
Geliştirme makinenizde hata ayıklama modunda çalıştırın ya da sunucunuza yayımlayın.

## <a name="6-view-your-telemetry-in-application-insights"></a>6. Application Insights'da telemetrinizi görüntüleme
[Microsoft Azure portalında](https://portal.azure.com), Application Insights kaynağınıza dönün.

HTTP isteklerine ilişkin veriler genel bakış dikey penceresinde görüntülenir. (Orada değilse, birkaç saniye bekleyip Yenile’ye tıklayın.)

![Örnek veriler genel bakış görüntüsü](./media/java-get-started/overview-graphs.png)

[Ölçümler hakkında daha fazla bilgi edinin.][metrics]

Daha ayrıntılı derlenmiş ölçümler görmek için herhangi bir grafiğe tıklayın.

![Application Insights hataları bölmesinde grafiklerle](./media/java-get-started/006-barcharts.png)

> Application Insights, MVC uygulamaları için HTTP isteklerinin biçiminin şu olduğunu varsayar: `VERB controller/action`. Örneğin, `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` ve `GET Home/Product/sdf96vws`; `GET Home/Product` içinde gruplandırılır. Bu gruplandırma, istek sayısı veya isteklerin yürütülme süresi gibi anlamlı istek toplamalarını etkinleştirir.
>
>

### <a name="instance-data"></a>Örnek veriler
Ayrı ayrı örnekleri görmek için belirli bir istek türüne tıklayın.

![Belirli bir örnek görünüme detaya gidin](./media/java-get-started/007-instance.png)

### <a name="analytics-powerful-query-language"></a>Analytics: Güçlü sorgu dili
Daha fazla veri birleştirdiğinizde hem veri toplama, hem de tek tek örneklerini bulmak için sorguları çalıştırabilirsiniz.  [Analiz](../../azure-monitor/app/analytics.md) hem performans, hem de kullanım için olmasının yanı sıra tanılama için de güçlü bir araçtır.

![Analizi örneği](./media/java-get-started/0025.png)

## <a name="7-install-your-app-on-the-server"></a>7. Uygulamanızı sunucuya yükleme
Artık uygulamanızı sunucuya yayımlayın, herkesin kullanmasını sağlayın ve portalda gösterilen telemetriye bakın.

* Güvenlik duvarınızın, uygulamanıza şu bağlantı noktalarına telemetri göndermesine izin verdiğinden emin olun:

  * dc.services.visualstudio.com:443
  * f5.services.visualstudio.com:443

* Giden trafiğin güvenlik duvarından geçirilmesi gerekiyorsa, `http.proxyHost` ve `http.proxyPort` sistem özelliklerini tanımlayın.

* Windows sunucularda yüklenecekler:

  * [Microsoft Visual C++ Yeniden Dağıtılabilir](https://www.microsoft.com/download/details.aspx?id=40784)

    (Bu bileşen, performans sayaçlarını etkinleştirir.)

## <a name="azure-app-service-config-spring-boot"></a>Azure App Service yapılandırma (Spring Boot)

Spring önyükleme uygulamalar Windows üzerinde çalışan Azure App Services'ta çalıştırmak için ek yapılandırma gerektirir. Değiştirme **web.config** ve aşağıdakileri ekleyin:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <handlers>
            <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified"/>
        </handlers>
        <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\AzureWebAppExample-0.0.1-SNAPSHOT.jar&quot;">
        </httpPlatform>
    </system.webServer>
</configuration>
```

## <a name="exceptions-and-request-failures"></a>Özel durumlar ve istek hataları
İşlenmeyen özel durumları otomatik olarak toplanır.

Diğer özel durumlar hakkında veri toplamak için iki seçeneğiniz vardır:

* [Kodunuzda trackException() çağrıları ekleme][apiexceptions].
* [Sunucunuza Java Agent yükleme](java-agent.md). İzlemek istediğiniz yöntemleri belirtin.

## <a name="monitor-method-calls-and-external-dependencies"></a>Yöntem çağrılarını ve dış bağımlılıkları izleme
Zamanlama verileriyle JDBC üzerinden yapılan belirli dahili yöntemleri ve çağrıları kaydetmek için [Java Agent yükleme](java-agent.md) işlemini gerçekleştirin.

## <a name="w3c-distributed-tracing"></a>W3C dağıtılmış izleme

Application Insights Java SDK'sı artık destekliyor [W3C dağıtılmış izleme](https://w3c.github.io/trace-context/).

Gelen SDK yapılandırması daha fazla makalemizde üzerinde açıklanmıştır [bağıntı](correlation.md#w3c-distributed-tracing).

Giden SDK yapılandırması tanımlanmış [AI Agent.xml](java-agent.md) dosya.

## <a name="performance-counters"></a>Performans sayaçları
Açık **Araştır**, **ölçümleri**çeşitli performans sayaçlarını görmek için.

![Seçili işlem özel baytlar ölçümleri bölmesinin ekran görüntüsü](./media/java-get-started/011-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Performans sayacı koleksiyonunu özelleştirme
Standart performans sayaçları dizisinin koleksiyonunu devre dışı bırakmak için aşağıdaki kodu ApplicationInsights.xml dosyasının kök düğümü altına ekleyin:

```XML
    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>
```

### <a name="collect-additional-performance-counters"></a>Ek performans sayaçlarını toplama
Toplanacak ek performans sayaçları belirtebilirsiniz.

#### <a name="jmx-counters-exposed-by-the-java-virtual-machine"></a>JMX sayaçları (Java Sanal Makinesi tarafından gösterilen)

```XML
    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>
```

* `displayName` – Application Insights portalında görüntülenen ad.
* `objectName` – JMX nesne adı.
* `attribute` – Getirilecek JMX nesne adının özniteliği
* `type` (isteğe bağlı) - JMX nesnenin öznitelik türü:
  * Varsayılan: int veya long gibi basit bir tür.
  * `composite`: performans sayacı verileri 'Attribute.Data' biçimindedir
  * `tabular`: performans sayacı verileri tablo satırı biçimindedir

#### <a name="windows-performance-counters"></a>Windows performans sayaçları
Her [Windows performans sayacı](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) bir kategorinin üyesidir (alanın bir sınıf üyesi olması gibi). Kategoriler genel olabileceği gibi numaralı veya adlı örneklere de sahip olabilir.

```XML
    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>
```

* displayName – Application Insights portalında görüntülenen ad.
* categoryName – Bu performans sayacıyla ilişkili performans sayacı kategorisi (performans nesnesi).
* counterName – Performans sayacının adı.
* instanceName – Performans sayacı kategorisi örneğinin adı veya kategoride tek örnek varsa boş bir dize (""). categoryName adı Process olursa ve uygulamanızın çalıştığı geçerli JVM işleminden performans sayacını toplamak istiyorsanız `"__SELF__"` öğesini belirtin.

### <a name="unix-performance-counters"></a>Unix Performans sayaçları
* Çok çeşitli sistem ve ağ verisi almak için [Application Insights eklentisiyle collectd yükleyin](java-collectd.md).

## <a name="local-forwarder"></a>Yerel iletici

[Yerel ileticisi](https://docs.microsoft.com/azure/application-insights/local-forwarder) Application Insights tarafından toplanan bir aracı veya [OpenCensus](https://opencensus.io/) çeşitli SDK'lar ve çerçeveleri alınan telemetri ve uygulama anlayışları'na yönlendirir. Bu, Windows ve Linux altında çalıştırma yeteneğine sahiptir.

```xml
<Channel type="com.microsoft.applicationinsights.channel.concrete.localforwarder.LocalForwarderTelemetryChannel">
<DeveloperMode>false</DeveloperMode>
<EndpointAddress><!-- put the hostname:port of your LocalForwarder instance here --></EndpointAddress>
<!-- The properties below are optional. The values shown are the defaults for each property -->
<FlushIntervalInSeconds>5</FlushIntervalInSeconds><!-- must be between [1, 500]. values outside the bound will be rounded to nearest bound -->
<MaxTelemetryBufferCapacity>500</MaxTelemetryBufferCapacity><!-- units=number of telemetry items; must be between [1, 1000] -->
</Channel>
```

SpringBoot başlangıç kullanıyorsanız, aşağıdaki yapılandırma dosyanız (application.properties) ekleyin:

```yml
azure.application-insights.channel.local-forwarder.endpoint-address=<!--put the hostname:port of your LocalForwarder instance here-->
azure.application-insights.channel.local-forwarder.flush-interval-in-seconds=<!--optional-->
azure.application-insights.channel.local-forwarder.max-telemetry-buffer-capacity=<!--optional-->
```

Varsayılan değerleri SpringBoot application.properties ve applicationınsights.XML yapılandırması için aynıdır.

## <a name="get-user-and-session-data"></a>Kullanıcı ve oturum verilerini alma
Tamam, web sunucunuzdan telemetri gönderiyorsunuz. Uygulamanızın 360 derecelik tam görünümünü almak için izlemeye katabileceğiniz birkaç şey daha vardır:

* Sayfa görünümlerini ve kullanıcı ölçümlerini izlemek için [web sayfalarınıza telemetri ekleyin][usage].
* Uygulamanızın canlı ve duyarlı kaldığından emin olmak için [web testleri oluşturun][availability].

## <a name="capture-log-traces"></a>Günlük izlemelerini yakalama
Log4J, Logback veya diğer günlük altyapılarına ait günlükleri ayrıntılı incelemek için Application Insights’ı kullanabilirsiniz. Günlükleri HTTP istekleri ve başka telemetriyle ilişkilendirebilirsiniz. [Nasıl olduğunu öğrenin][javalogs].

## <a name="send-your-own-telemetry"></a>Kendi telemetrinizi gönderme
Artık SDK'yı da yüklediğinize göre, kendi telemetrinizi göndermek için API'yi kullanabilirsiniz.

* Uygulamanızla kullanıcıların ne yaptıklarını öğrenmek için [Özel olayları ve ölçümleri izleyin][api].
* Sorunların tanımlanması için [Olayları ve günlükleri arayın][diagnostic].

## <a name="availability-web-tests"></a>Kullanılabilirlik web testleri
Kullanıma hazır ve düzgün yanıt verdiğini denetlemek için Application Insights belirli aralıklarla web sitenizi test edebilir.

[Kullanılabilirlik web testleri ayarlama hakkında daha fazla bilgi edinin.][availability]

## <a name="questions-problems"></a>Sorular? Sorunlarınız mı var?
[Java Sorun Giderme](java-troubleshoot.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Bağımlılık çağrılarını izleme](java-agent.md)
* [Unix Performans sayaçlarını izleme](java-collectd.md)
* Sayfa yükleme sürelerini, AJAX çağrılarını ve tarayıcı özel durumlarını izlemek için [web sayfalarınıza izleme ekleyin](javascript.md).
* Tarayıcıda veya sunucuda kullanımı izlemek için [özel telemetri](../../azure-monitor/app/api-custom-events-metrics.md) yazın.
* Sisteminizi izlemek üzere anahtar grafikleri bir araya getirmek için [panolar](../../azure-monitor/app/app-insights-dashboards.md) oluşturun.
* Uygulamanızdan telemetri üzerinde güçlü sorgular yapmak için [Analytics](../../azure-monitor/app/analytics.md)'i kullanın
* Daha fazla bilgi için bkz. [Java geliştiricileri için Azure](/java/azure).

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#trackexception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: ../../azure-monitor/learn/java-quick-start.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
[usage]: javascript.md
