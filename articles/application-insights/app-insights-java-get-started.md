---
title: Azure Application Insights ile Java web uygulaması analizi | Microsoft Docs
description: 'Application Insights ile Java web uygulamaları için Uygulama Performansı İzleme. '
services: application-insights
documentationcenter: java
author: harelbr
manager: carmonm
ms.assetid: 051d4285-f38a-45d8-ad8a-45c3be828d91
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: f772485dd49a730e34ec856768fa91bc3fdd114b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-application-insights-in-a-java-web-project"></a>Java web projesinde Application Insights ile başlarken


[Application Insights](https://azure.microsoft.com/services/application-insights/), web geliştiricileri için canlı uygulamanızın performansını ve kullanımını anlamanıza yardımcı olan genişletilebilir bir analiz hizmetidir. [Performans sorunlarını ve özel durumlarını algılamak ve tanılamak](app-insights-detect-triage-diagnose.md) için bunu kullanın; uygulamanızla kullanıcıların ne yaptığını izlemek için de [kod yazın][api].

![örnek veri](./media/app-insights-java-get-started/5-results.png)

Application Insights; Linux, Unix veya Windows üzerinde çalışan Java uygulamalarını destekler.

Gerekenler:

* Oracle ya da Zulu JRE sürüm 1.7 veya 1.8
* Bir [Microsoft Azure](https://azure.microsoft.com/) aboneliği.

*Zaten canlı olan bir web uygulaması varsa, [web sunucusuna çalışma zamanında SDK eklemek](app-insights-java-live.md) için alternatif bir yordam izleyebilirsiniz. Bu alternatif yordam, kodun yeniden derlenmesini engellese de, kullanıcı etkinliğini izlemek için kod yazma seçeneğini elde etmezsiniz.*

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Application Insights izleme anahtarı edinme
1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.
2. Bir Application Insights kaynağı oluşturun. Uygulama türünü Java web uygulaması olarak ayarlayın.

    ![Ad girme, Java web uygulaması seçme ve Oluştur’a tıklama](./media/app-insights-java-get-started/02-create.png)
3. Yeni kaynağın izleme anahtarını bulun. Bu anahtarı hemen kod projenize yapıştırmalısınız.

    ![Yeni kaynağa genel bakışta, Özellikler'e tıklayıp izleme anahtarını kopyalama](./media/app-insights-java-get-started/03-key.png)

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

* *Derleme veya sağlama toplamı doğrulama hataları mı var?* `<version>2.0.n</version>` gibi belirli bir sürümü kullanmayı deneyin. En son sürümü [SDK sürüm notlarında](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) veya [Maven yapıtları](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights) sitesinde bulacaksınız.
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

* *Derleme veya sağlama toplamı doğrulama hataları mı var?* `version:'2.0.n'` gibi belirli bir sürümü kullanmayı deneyin. En son sürümü [SDK sürüm notlarında](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) veya [Maven yapıtları](http://search.maven.org/#search%7Cga%7C1%7Capplicationinsights) sitesinde bulacaksınız.
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

Ayrıca [kod içinde ayarlayabilirsiniz](app-insights-api-custom-events-metrics.md#ikey):

```Java
    TelemetryConfiguration.getActive().setInstrumentationKey(iKey);
```

## <a name="4-add-an-http-filter"></a>4. HTTP filtresi ekleme
Son yapılandırma adımı HTTP isteği bileşeninin her web isteğini kaydetmesini sağlar. (Yalnızca tam API istiyorsanız gerekmez.)

### <a name="spring-boot-applications"></a>Spring Boot Uygulamaları
Yapılandırma sınıfınıza Application Insights `WebRequestTrackingFilter` filtresini kaydetme:

```Java
package devCamp.WebApp.configurations;

    import javax.servlet.Filter;

    import org.springframework.boot.context.embedded.FilterRegistrationBean;
    import org.springframework.context.annotation.Bean;
    import org.springframework.core.Ordered;
    import org.springframework.beans.factory.annotation.Value;
    import org.springframework.context.annotation.Configuration;
    import com.microsoft.applicationinsights.TelemetryConfiguration;
    import com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter;


    @Configuration
    public class AppInsightsConfig {

    //Initialize AI TelemetryConfiguration via Spring Beans
        @Bean
        public String telemetryConfig() {
            String telemetryKey = System.getenv("APPLICATION_INSIGHTS_IKEY");
            if (telemetryKey != null) {
                TelemetryConfiguration.getActive().setInstrumentationKey(telemetryKey);
            }
            return telemetryKey;
        }
    
    //Set AI Web Request Tracking Filter
        @Bean
        public FilterRegistrationBean aiFilterRegistration(@Value("${spring.application.name:application}") String applicationName) {
           FilterRegistrationBean registration = new FilterRegistrationBean();
           registration.setFilter(new WebRequestTrackingFilter(applicationName));
           registration.setName("webRequestTrackingFilter");
           registration.addUrlPatterns("/*");
           registration.setOrder(Ordered.HIGHEST_PRECEDENCE + 10);
           return registration;
       } 

    //Set up AI Web Request Tracking Filter
        @Bean(name = "WebRequestTrackingFilter")
        public Filter webRequestTrackingFilter(@Value("${spring.application.name:application}") String applicationName) {
            return new WebRequestTrackingFilter(applicationName);
        }   
    }
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

![örnek veri](./media/app-insights-java-get-started/5-results.png)

[Ölçümler hakkında daha fazla bilgi edinin.][metrics]

Daha ayrıntılı derlenmiş ölçümler görmek için herhangi bir grafiğe tıklayın.

![](./media/app-insights-java-get-started/6-barchart.png)

> Application Insights, MVC uygulamaları için HTTP isteklerinin biçiminin şu olduğunu varsayar: `VERB controller/action`. Örneğin, `GET Home/Product/f9anuh81`, `GET Home/Product/2dffwrf5` ve `GET Home/Product/sdf96vws`; `GET Home/Product` içinde gruplandırılır. Bu gruplandırma, istek sayısı veya isteklerin yürütülme süresi gibi anlamlı istek toplamalarını etkinleştirir.
>
>

### <a name="instance-data"></a>Örnek veriler
Ayrı ayrı örnekleri görmek için belirli bir istek türüne tıklayın.

Application Insights’ta iki tür veri görüntülenir: ortalama, sayım ve toplam olarak depolanan birleşik veriler; HTTP isteklerinin tek tek raporu, özel durumlar, sayfa görünümleri veya özel olaylar olarak görüntülenen örnek veriler.

İstek özellikleri görüntülendiğinde, istekler ve özel durumlar gibi bununla ilişkili telemetri olayları görebilirsiniz.

![](./media/app-insights-java-get-started/7-instance.png)

### <a name="analytics-powerful-query-language"></a>Analiz: Güçlü sorgu dili
Daha fazla veri birleştirdiğinizde hem veri toplama, hem de tek tek örneklerini bulmak için sorguları çalıştırabilirsiniz.  [Analiz](app-insights-analytics.md) hem performans, hem de kullanım için olmasının yanı sıra tanılama için de güçlü bir araçtır.

![Analizi örneği](./media/app-insights-java-get-started/025.png)

## <a name="7-install-your-app-on-the-server"></a>7. Uygulamanızı sunucuya yükleme
Artık uygulamanızı sunucuya yayımlayın, herkesin kullanmasını sağlayın ve portalda gösterilen telemetriye bakın.

* Güvenlik duvarınızın, uygulamanıza şu bağlantı noktalarına telemetri göndermesine izin verdiğinden emin olun:

  * dc.services.visualstudio.com:443
  * f5.services.visualstudio.com:443

* Giden trafiğin güvenlik duvarından geçirilmesi gerekiyorsa, `http.proxyHost` ve `http.proxyPort` sistem özelliklerini tanımlayın.

* Windows sunucularda yüklenecekler:

  * [Microsoft Visual C++ Yeniden Dağıtılabilir](http://www.microsoft.com/download/details.aspx?id=40784)

    (Bu bileşen, performans sayaçlarını etkinleştirir.)


## <a name="exceptions-and-request-failures"></a>Özel durumlar ve istek hataları
İşlenmeyen özel durumlar otomatik olarak toplanır:

![Ayarlar, Hatalar’ı açın](./media/app-insights-java-get-started/21-exceptions.png)

Diğer özel durumlar hakkında veri toplamak için iki seçeneğiniz vardır:

* [Kodunuzda trackException() çağrıları ekleme][apiexceptions].
* [Sunucunuza Java Agent yükleme](app-insights-java-agent.md). İzlemek istediğiniz yöntemleri belirtin.

## <a name="monitor-method-calls-and-external-dependencies"></a>Yöntem çağrılarını ve dış bağımlılıkları izleme
Zamanlama verileriyle JDBC üzerinden yapılan belirli dahili yöntemleri ve çağrıları kaydetmek için [Java Agent yükleme](app-insights-java-agent.md) işlemini gerçekleştirin.

## <a name="performance-counters"></a>Performans sayaçları
Çeşitli performans sayaçlarını görmek için **Ayarlar**, **Sunucular**’ı açın.

![](./media/app-insights-java-get-started/11-perf-counters.png)

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

Özel ölçümleriniz [Ölçüm Gezgini][metrics]'nde olduğundan performans sayaçlarınız görünürdür.

![](./media/app-insights-java-get-started/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Unix Performans sayaçları
* Çok çeşitli sistem ve ağ verisi almak için [Application Insights eklentisiyle collectd yükleyin](app-insights-java-collectd.md).

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
Kullanıma hazır ve düzgün yanıt verdiğini denetlemek için Application Insights belirli aralıklarla web sitenizi test edebilir. [Ayarlamak için][availability], Web testleri'ne tıklayın.

![Web testleri’ne ve ardından Web testi ekle’ye tıklayın](./media/app-insights-java-get-started/31-config-web-test.png)

Yanıt süreleri grafiklerine ek olarak, siteniz devre dışı kalırsa e-posta bildirimleri de alacaksınız.

![Web testi örneği](./media/app-insights-java-get-started/appinsights-10webtestresult.png)

[Kullanılabilirlik web testleri hakkında daha fazla bilgi edinin.][availability]

## <a name="questions-problems"></a>Sorularınız mı var? Sorunlarınız mı var?
[Java Sorun Giderme](app-insights-java-troubleshoot.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar
* [Bağımlılık çağrılarını izleme](app-insights-java-agent.md)
* [Unix Performans sayaçlarını izleme](app-insights-java-collectd.md)
* Sayfa yükleme sürelerini, AJAX çağrılarını ve tarayıcı özel durumlarını izlemek için [web sayfalarınıza izleme ekleyin](app-insights-javascript.md).
* Tarayıcıda veya sunucuda kullanımı izlemek için [özel telemetri](app-insights-api-custom-events-metrics.md) yazın.
* Sisteminizi izlemek üzere anahtar grafikleri bir araya getirmek için [panolar](app-insights-dashboards.md) oluşturun.
* Uygulamanızdan telemetri üzerinde güçlü sorgular yapmak için [Analytics](app-insights-analytics.md)'i kullanın
* Daha fazla bilgi için bkz. [Java geliştiricileri için Azure](/java/azure).

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#trackexception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-javascript.md
