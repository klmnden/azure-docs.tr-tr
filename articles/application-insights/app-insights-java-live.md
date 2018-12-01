---
title: Zaten olan bir Java web uygulamaları için Application Insights Canlı
description: Sunucuda zaten çalışan bir web uygulamasını İzlemeyi Başlat
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 12f3dbb9-915f-4087-87c9-807286030b0b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/10/2016
ms.author: mbullwin
ms.openlocfilehash: 8e8a2e19e97bc07ed481adb3ecc3ae1d34ea8368
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721009"
---
# <a name="application-insights-for-java-web-apps-that-are-already-live"></a>Zaten olan bir Java web uygulamaları için Application Insights Canlı

J2EE sunucuda zaten çalışan bir web uygulamanız varsa, onunla izlemeye başlayabilirsiniz [Application Insights](app-insights-overview.md) gerekmeden kod değişiklikleri yapabilir veya projenizi yeniden derleyin. Bu seçenek ile sunucu, işlenmemiş özel durumlar ve performans sayaçları için gönderilen HTTP istekleriyle ilgili bilgileri alın.

Size bir [Microsoft Azure](https://azure.com) aboneliği gerekecektir.

> [!NOTE]
> Bu sayfadaki yordamı çalışma zamanında web uygulamanızı SDK'sını ekler. Bu çalışma zamanı izleme, güncelleştirme veya kaynak kodunuzu yeniden istemiyorsanız yararlıdır. Ancak, mümkünse öneririz [kaynak koduna SDK'sı ekleme](app-insights-java-get-started.md) yerine. Bu kullanıcı etkinliğini izlemek için diğer seçenekler kod yazma gibi sağlar.
> 
> 

## <a name="1-get-an-application-insights-instrumentation-key"></a>1. Application Insights izleme anahtarı edinme
1. Oturum [Microsoft Azure portal](https://portal.azure.com)
2. Yeni bir Application Insights kaynağı oluşturun ve uygulama türünü Java web uygulaması olarak ayarlayın.
   
    ![Ad girme, Java web uygulaması seçme ve Oluştur’a tıklama](./media/app-insights-java-live/02-create.png)

    Kaynak, birkaç saniye içinde oluşturulur.

4. Yeni kaynak açın ve kendi izleme anahtarını alın. Bu anahtarı hemen kod projenize yapıştırmalısınız.
   
    ![Yeni kaynağa genel bakışta, Özellikler'e tıklayıp izleme anahtarını kopyalama](./media/app-insights-java-live/03-key.png)

## <a name="2-download-the-sdk"></a>2. SDK'sını indirin
1. [Java için Application Insights SDK’sı](https://aka.ms/aijavasdk) indirin. 
2. Sunucunuzda SDK içeriği proje ikili yüklü dizine ayıklayın. Tomcat kullanıyorsanız, bu dizin altında genelde olacaktır `webapps/<your_app_name>/WEB-INF/lib`

Bu, her bir sunucu örneğindeki ve her uygulama için yineleyin gerektiğini unutmayın.

## <a name="3-add-an-application-insights-xml-file"></a>3. Bir Application Insights xml dosyası ekleyin
SDK'yı eklediğiniz klasörüne applicationınsights.xml dosyasını oluşturun. İçine aşağıdaki XML'i yerleştirin.

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
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>

      </TelemetryInitializers>
    </ApplicationInsights>
```

* İzleme anahtarı telemetrinin her öğesiyle birlikte gönderilir ve Application Insights’ın bunu kaynağınızda görüntülemesini isteyin.
* HTTP isteği bileşeni isteğe bağlıdır. İstek ve yanıt süreleri hakkında telemetriyi otomatik olarak portala gönderir.
* Olay bağıntısı HTTP isteği bileşenine bir ektir. Sunucu tarafından alınan her istek için bir tanımlayıcı atar ve bu tanımlayıcıyı bir özellik olarak, telemetrinin her öğesine 'Operation.Id' özelliği olarak ekler. Bir filtre ayarlayarak her istekle ilişkili telemetrinin bağıntısını kurmanızı sağlayan [tanılama araması](app-insights-diagnostic-search.md).

## <a name="4-add-an-http-filter"></a>4. HTTP filtresi ekleme
Bulun ve projenizde web.xml dosyasını açın ve aşağıdaki kod parçacığı, uygulama filtrelerinizin yapılandırıldığı web uygulaması düğümü altında kod birleştirme.

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

## <a name="5-check-firewall-exceptions"></a>5. Güvenlik Duvarı özel durumlarını kontrol edin
Gerekebilir [ayarla, giden veri göndermek için özel durumlar](app-insights-ip-addresses.md).

## <a name="6-restart-your-web-app"></a>6. Web uygulamanızı yeniden başlatın
## <a name="7-view-your-telemetry-in-application-insights"></a>7. Application Insights'da telemetrinizi görüntüleme
[Microsoft Azure portalında](https://portal.azure.com), Application Insights kaynağınıza dönün.

HTTP istekleriyle ilgili telemetri genel bakış dikey penceresinde görüntülenir. (Orada değilse, birkaç saniye bekleyip Yenile’ye tıklayın.)

![örnek veri](./media/app-insights-java-live/5-results.png)

Daha ayrıntılı ölçümler görmek için herhangi bir grafiğe tıklayın. 

![](./media/app-insights-java-live/6-barchart.png)

Ve istek özellikleri görüntülendiğinde, istekler ve özel durumlar gibi bununla ilişkili telemetri olayları görebilirsiniz.

![](./media/app-insights-java-live/7-instance.png)

[Ölçümler hakkında daha fazla bilgi edinin.](app-insights-metrics-explorer.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Web sayfalarınıza telemetri ekleyin](app-insights-javascript.md) sayfa görünümlerini ve kullanıcı ölçümlerini izlemek için.
* [Web testleri ayarlayın](app-insights-monitor-web-app-availability.md) canlı ve duyarlı uygulamanızı kaldığından emin olmak için.
* [Günlük izlemelerini yakalama](app-insights-java-trace-logs.md)
* [Olayları ve günlükleri arayın](app-insights-diagnostic-search.md) sorunların tanılanmasına yardımcı olmak için.
* [Bir Spring Boot Başlatıcı uygulamasını yapılandırma](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights)
