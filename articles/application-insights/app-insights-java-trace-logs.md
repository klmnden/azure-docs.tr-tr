---
title: "İzleme günlüklerini Azure Application Insights'ta Java keşfetme | Microsoft Docs"
description: Application Insights arama Log4J veya Logback izlemeleri
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: mbullwin
ms.openlocfilehash: 6e441c9cbd15bb1528ea8e8a781f90900af90cf2
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Application Insights izleme günlüklerini Java keşfedin
Logback veya Log4J kullanıyorsanız (1.2 sürümü veya v2.0) için izlemeyi, izleme günlüklerinizi uygulama burada keşfedin ve bunlar üzerinde arama Öngörüler otomatik olarak gönderilen sahip olabilir.

## <a name="install-the-java-sdk"></a>Java'yı yükleme SDK

Yükleme [Java için Application Insights SDK][java], size, bu işlemi yapmadıysanız.

(HTTP isteklerini izlemek istemediğiniz, .xml yapılandırma dosyası çoğunu atlayabilirsiniz, ancak en az içermelidir `InstrumentationKey` öğesi. Ayrıca çağırmalıdır `new TelemetryClient()` SDK başlatılamadı.)


## <a name="add-logging-libraries-to-your-project"></a>Günlüğe kaydetme kitaplıklarını projenize ekleyin
*Projeniz için uygun yolu seçin.*

#### <a name="if-youre-using-maven"></a>Maven kullanıyorsanız...
Projeniz zaten Maven derleme için kullanmak üzere ayarlanmışsa, aşağıdaki kod parçacıkları birini pom.xml dosyanızla birleştirin.

Daha sonra proje bağımlılıklarını ikili dosyaları almak için yenileyin.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v2.0*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J 1.2 sürümü*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Gradle kullanıyorsanız...
Projeniz zaten Gradle için derleme kullanmak üzere ayarlanmışsa, aşağıdaki satırları ekleyin `dependencies` build.gradle dosyanızla grubu:

Daha sonra proje bağımlılıklarını ikili dosyaları almak için yenileyin.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

**Log4J v2.0**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

**Log4J 1.2 sürümü**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a>Aksi taktirde...
Karşıdan yükleyin ve uygun appender ayıklayın ve sonra uygun kitaplığını projenize ekleyin:

| Günlükçü | İndir | Kitaplık |
| --- | --- | --- |
| Logback |[SDK Logback appender ile](https://aka.ms/xt62a4) |applicationınsights günlük logback |
| Log4J v2.0 |[Log4J v2 appender SDK'sı](https://aka.ms/qypznq) |applicationınsights günlük log4j2 |
| Log4j 1.2 sürümü |[Log4J 1.2 sürümü appender SDK'sı](https://aka.ms/ky9cbo) |applicationınsights günlük log4j1_2 |

## <a name="add-the-appender-to-your-logging-framework"></a>Günlüğe kaydetme framework appender Ekle
İzlemeler alma başlatmak için ilgili Log4J veya Logback yapılandırma dosyası için kod parçacığını birleştirin: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J v2.0*

```XML

    <Configuration packages="com.microsoft.applicationinsights.Log4j">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J 1.2 sürümü*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

Application Insights appenders (Yukarıdaki kod örnekleri gösterildiği gibi) yapılandırılmış tüm Günlükçü ve mutlaka kök Günlükçü tarafından başvurulabilir.

## <a name="explore-your-traces-in-the-application-insights-portal"></a>Application Insights portalında, izlemeleri keşfedin
Projenizi izlemeleri Application Insights'a gönderme yapılandırdıktan, görüntüleyin ve bu izlemelerin Application Insights portalında arama [arama] [ diagnostic] dikey.

![Application Insights portalında arama açın](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>Sonraki adımlar
[Tanılama arama][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


