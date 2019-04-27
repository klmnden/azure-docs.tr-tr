---
title: Java web uygulamanızı Azure Application Insights telemetri filtreleme | Microsoft Docs
description: Olay İzleme gerekmez filtreleyerek telemetri trafiğini azaltır.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 11/23/2016
ms.author: mbullwin
ms.openlocfilehash: ee50a0e9c7fca8f01f12b3508c86d901b5315120
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60897123"
---
# <a name="filter-telemetry-in-your-java-web-app"></a>Java web uygulamanıza telemetri filtreleme

Filtreleri telemetri seçmek için bir yol sağlar, uygulamanızın [Java web uygulaması, Application Insights'a gönderir](java-get-started.md). Kullanabileceğiniz bazı Giden kutusu filtreler vardır ve kendi özel filtreler de yazabilirsiniz.

Kullanıma hazır filtreler aşağıdakileri içerir:

* Önem düzeyi izleme
* Belirli URL'lere, anahtar sözcükler veya yanıt kodları
* Hızlı yanıtlar - diğer bir deyişle, istekleri için uygulamanızı hızlı bir şekilde yanıt verdi.
* Belirli olay adları

> [!NOTE]
> Filtreler, uygulamanızın ölçümleri eğme. Örneğin, yavaş yanıtlar tanılamak için hızlı yanıt süresi atmak için bir filtre ayarlamanız, karar verebilirsiniz. Ancak, Application Insights tarafından bildirilen ortalama yanıt sürelerinin ardından true hızından daha yavaş olur ve isteklerinin sayısı gerçek sayısından daha küçük farkında olmanız gerekir.
> Bu bir konudur kullanırsanız [örnekleme](../../azure-monitor/app/sampling.md) yerine.

## <a name="setting-filters"></a>Filtre ayarlama

Applicationınsights.XML içinde ekleme bir `TelemetryProcessors` bölümü şu örnekteki gibi:


```XML

    <ApplicationInsights>
      <TelemetryProcessors>

        <BuiltInProcessors>
           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="100"/>
                  <Add name="NotNeededResponseCodes" value="200-400"/>
           </Processor>

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="100"/>
                  <Add name="NotNeededNames" value="home,index"/>
                  <Add name="NotNeededUrls" value=".jpg,.css"/>
           </Processor>

           <Processor type="TelemetryEventFilter">
                  <!-- Names of events we don't want to see -->
                  <Add name="NotNeededNames" value="Start,Stop,Pause"/>
           </Processor>

           <!-- Exclude telemetry from availability tests and bots -->
           <Processor type="SyntheticSourceFilter">
                <!-- Optional: specify which synthetic sources,
                     comma-separated
                     - default is all synthetics -->
                <Add name="NotNeededSources" value="Application Insights Availability Monitoring,BingPreview"
           </Processor>

        </BuiltInProcessors>

        <CustomProcessors>
          <Processor type="com.fabrikam.MyFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>

      </TelemetryProcessors>
    </ApplicationInsights>

```




[Tam yerleşik işlemci inceleyin](https://github.com/Microsoft/ApplicationInsights-Java/tree/master/core/src/main/java/com/microsoft/applicationinsights/internal/processor).

## <a name="built-in-filters"></a>Yerleşik filtreleri

### <a name="metric-telemetry-filter"></a>Ölçüm Telemetri filtreleme

```XML

           <Processor type="MetricTelemetryFilter">
                  <Add name="NotNeeded" value="metric1,metric2"/>
           </Processor>
```

* `NotNeeded` -Özel ölçüm adları virgülle ayrılmış listesi.


### <a name="page-view-telemetry-filter"></a>Sayfa görünümü Telemetrisini filtresi

```XML

           <Processor type="PageViewTelemetryFilter">
                  <Add name="DurationThresholdInMS" value="500"/>
                  <Add name="NotNeededNames" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```

* `DurationThresholdInMS` -Süresi sayfanın yüklenmesi için geçen süre için ifade eder. Bu ayarlanırsa, bu saatten daha hızlı yüklenen sayfaları raporlanmaz.
* `NotNeededNames` -Sayfa adları virgülle ayrılmış listesi.
* `NotNeededUrls` -Parçalarla ilgili URL virgülle ayrılmış listesi. Örneğin, `"home"` "home" URL'de bulunan tüm sayfaları filtreler.


### <a name="request-telemetry-filter"></a>İstek Telemetri filtresi


```XML

           <Processor type="RequestTelemetryFilter">
                  <Add name="MinimumDurationInMS" value="500"/>
                  <Add name="NotNeededResponseCodes" value="page1,page2"/>
                  <Add name="NotNeededUrls" value="url1,url2"/>
           </Processor>
```



### <a name="synthetic-source-filter"></a>Yapay Kaynak Filtresi

SyntheticSource özelliğinde değerlere sahip tüm telemetri filtreler. Bunlar, robotlar ve örümcekler kullanılabilirlik testleri gelen istekleri içerir.

Tüm yapay istekler için telemetri filtreleyin:


```XML

           <Processor type="SyntheticSourceFilter" />
```

Telemetri yapay belirli kaynaklar için filtreleyin:


```XML

           <Processor type="SyntheticSourceFilter" >
                  <Add name="NotNeeded" value="source1,source2"/>
           </Processor>
```

* `NotNeeded` -Yapay kaynak adları virgülle ayrılmış listesi.

### <a name="telemetry-event-filter"></a>Telemetri olay filtresi

Özel olayları filtreler (kullanarak oturum [TrackEvent()](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)).


```XML

           <Processor type="TelemetryEventFilter" >
                  <Add name="NotNeededNames" value="event1, event2"/>
           </Processor>
```


* `NotNeededNames` -Olay adları virgülle ayrılmış listesi.


### <a name="trace-telemetry-filter"></a>İzleme Telemetri filtreleme

Günlük izlemeleri filtreleri (kullanarak oturum [TrackTrace()](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace) veya [günlüğe kaydetme çerçevesi Toplayıcı](java-trace-logs.md)).

```XML

           <Processor type="TraceTelemetryFilter">
                  <Add name="FromSeverityLevel" value="ERROR"/>
           </Processor>
```

* `FromSeverityLevel` Geçerli değerler şunlardır:
  *  Kapalı - tüm izlemeleri Filtrele
  *  İzleme - filtre yok. İzleme düzeyi eşit
  *  BİLGİ - out izleme düzeyi filtresi
  *  Uyarı - filtre izleme ve bilgileri
  *  HATA - out UYAR, bilgi, izleme filtresi
  *  Kritik - tüm kritik filtresi


## <a name="custom-filters"></a>Özel Filtreler

### <a name="1-code-your-filter"></a>1. Filtreniz kod

Kodunuzda uygulayan bir sınıf oluşturma `TelemetryProcessor`:

```Java

    package com.fabrikam.MyFilter;
    import com.microsoft.applicationinsights.extensibility.TelemetryProcessor;
    import com.microsoft.applicationinsights.telemetry.Telemetry;

    public class SuccessFilter implements TelemetryProcessor {

       /* Any parameters that are required to support the filter.*/
       private final String successful;

       /* Initializers for the parameters, named "setParameterName" */
       public void setNotNeeded(String successful)
       {
          this.successful = successful;
       }

       /* This method is called for each item of telemetry to be sent.
          Return false to discard it.
          Return true to allow other processors to inspect it. */
       @Override
       public boolean process(Telemetry telemetry) {
        if (telemetry == null) { return true; }
        if (telemetry instanceof RequestTelemetry)
        {
            RequestTelemetry requestTelemetry = (RequestTelemetry)telemetry;
            return request.getSuccess() == successful;
        }
        return true;
       }
    }

```


### <a name="2-invoke-your-filter-in-the-configuration-file"></a>2. Yapılandırma dosyası, filtre çağırma

Applicationınsights.XML içinde:

```XML


    <ApplicationInsights>
      <TelemetryProcessors>
        <CustomProcessors>
          <Processor type="com.fabrikam.SuccessFilter">
            <Add name="Successful" value="false"/>
          </Processor>
        </CustomProcessors>
      </TelemetryProcessors>
    </ApplicationInsights>

```

### <a name="3-invoke-your-filter-java-spring"></a>3. Filtreniz (Java Spring) Çağır

Spring framework tabanlı uygulamalar için özel telemetri işlemci bir çekirdeklere ana uygulama sınıfınızda kaydedilmesi gerekir. Uygulama başlatıldığında bunlar autowired olacaktır.

```Java
@Bean
public TelemetryProcessor successFilter() {
      return new SuccessFilter();
}
```

Kendi filtre parametrelerinde oluşturmanız gerekecektir `application.properties` ve özel filtrenizle bu parametreleri geçirmek için Spring Boot'ın te dış yapılandırma framework yararlanın. 


## <a name="troubleshooting"></a>Sorun giderme

*My filtresi çalışmıyor.*

* Geçerli parametre değerleri sağladığınızdan denetleyin. Örneğin, süreleri tamsayılar olmalıdır. Geçersiz değerler yok sayılacak filtre neden olur. Filtrenizi özel bir oluşturucu veya set yöntemi bir özel durum oluşturursa, göz ardı edilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Örnekleme](../../azure-monitor/app/sampling.md) -örnekleme ölçümlerinizi eğme olmayan alternatif olarak göz önünde bulundurun.
