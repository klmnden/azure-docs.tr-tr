---
title: İzleme günlükleri Azure Application Insights Java keşfedin | Microsoft Docs
description: Application ınsights arama Log4J veya Logback izlemeleri
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: mbullwin
ms.openlocfilehash: 614f9a44f7c699be38906ac00e12f523490ce112
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58884304"
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Application Insights izleme günlükleri Java keşfedin
Logback veya Log4J kullanıyorsanız (v1.2 veya v2.0) için izleme, otomatik olarak burada keşfedin ve bunlar üzerinde arama Application ınsights'a gönderilen izleme günlüklerinizi sahip olabilir.

## <a name="install-the-java-sdk"></a>Java'yı yükleme SDK'sı

Yüklemek için yönergeleri izleyin [Java için Application Insights SDK'sı][java], zaten, yapmadınız.

## <a name="add-logging-libraries-to-your-project"></a>Günlüğe kaydetme kitaplıklarını projenize ekleyin.
*Projeniz için uygun yolu seçin.*

#### <a name="if-youre-using-maven"></a>Maven kullanıyorsanız...
Projeniz Maven derleme için kullanılacak ayarladıysanız, aşağıdaki kod parçacıkları birini pom.xml dosyanıza kopyalayıp birleştirin.

Ardından Proje bağımlılıklarını, ikili dosyaları indirmek için yenileyin.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v2.0*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v1.2*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Gradle kullanıyorsanız...
Projeniz zaten Gradle derleme için kullanılacak ayarlanıp ayarlanmadığını, aşağıdaki satırları ekleyin `dependencies` build.gradle dosyanıza grubu:

Ardından Proje bağımlılıklarını, ikili dosyaları indirmek için yenileyin.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '2.0.+'
```

**Log4J v2.0**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '2.0.+'
```

**Log4J v1.2**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '2.0.+'
```

#### <a name="otherwise-"></a>Aksi taktirde...
El ile Application Insights Java SDK'sı jar dosyasını (Maven Central Sayfası'nı tıklatın 'jar' bağlantısına yükleme bölümünde geldikten sonra) için uygun ekleyici karşıdan yükleyin ve indirilen ekleyici jar projeye eklemek için yönergeleri izleyin.

| Günlükçü | İndirme | Kitaplık |
| --- | --- | --- |
| Logback |[Jar Logback ekleyici](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-logback%22) |applicationınsights günlük logback |
| Log4J v2.0 |[Log4J v2 ekleyici Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j2%22) |applicationınsights günlük log4j2 |
| Log4j v1.2 |[Log4J v1.2 ekleyici Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j1_2%22) |applicationınsights günlük log4j1_2 |


## <a name="add-the-appender-to-your-logging-framework"></a>İçin günlüğe kaydetme çerçevesi ekleyici ekleyin
İzlemeleri başlamanız için ilgili Log4J veya Logback yapılandırma dosyası için kod parçacığı birleştirme: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
        <instrumentationKey>[APPLICATION_INSIGHTS_KEY]</instrumentationKey>
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J v2.0*

```XML

    <Configuration packages="com.microsoft.applicationinsights.log4j.v2">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" instrumentationKey="[APPLICATION_INSIGHTS_KEY]" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J v1.2*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
        <param name="instrumentationKey" value="[APPLICATION_INSIGHTS_KEY]" />
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

Application Insights appenders (Yukarıdaki kod örnekleri gösterildiği gibi) tüm yapılandırılmış Günlükçü ve mutlaka kök Günlükçü tarafından başvurulabilir.

## <a name="explore-your-traces-in-the-application-insights-portal"></a>Application Insights portalında, izlemeleri keşfet
Projenize Application Insights izlemelerini göndermek için yapılandırmış olduğunuz, görüntüleyebilir ve bu izlemelerin Application Insights portalında arama [arama] [ diagnostic] dikey penceresi.

Özel durum günlükçüleri gönderilen özel durum Telemetrisi portalda görüntülenir.

![Application Insights portalında arama açın](./media/java-trace-logs/01-diagnostics.png)

## <a name="next-steps"></a>Sonraki adımlar
[Tanılama araması][diagnostic]

<!--Link references-->

[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[java]: java-get-started.md


