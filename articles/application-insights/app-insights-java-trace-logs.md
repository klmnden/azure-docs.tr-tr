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
ms.date: 02/12/2018
ms.author: mbullwin
ms.openlocfilehash: fae3269e21d0f760ae77a70333047306c07c2961
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Application Insights izleme günlüklerini Java keşfedin
Logback veya Log4J kullanıyorsanız (1.2 sürümü veya v2.0) için izlemeyi, izleme günlüklerinizi uygulama burada keşfedin ve bunlar üzerinde arama Öngörüler otomatik olarak gönderilen sahip olabilir.

## <a name="install-the-java-sdk"></a>Java'yı yükleme SDK

Yüklemek için yönergeleri izleyin [Java için Application Insights SDK][java], size, bu işlemi yapmadıysanız.

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
Projeniz zaten Gradle için derleme kullanmak üzere ayarlanmışsa, aşağıdaki satırları ekleyin `dependencies` build.gradle dosyanızla grubu:

Daha sonra proje bağımlılıklarını ikili dosyaları almak için yenileyin.

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
El ile Application Insights Java SDK'sı (Maven Merkezi sayfasında ariving tıklattıktan sonra yükleme bölümünde 'jar' bağlantısında) jar için uygun appender karşıdan yükleyin ve indirilen appender jar projeye eklemek için yönergeleri izleyin.

| Günlükçü | İndirme | Kitaplık |
| --- | --- | --- |
| Logback |[Logback appender Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-logback%22) |applicationınsights günlük logback |
| Log4J v2.0 |[Log4J v2 appender Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j2%22) |applicationinsights-logging-log4j2 |
| Log4j v1.2 |[Log4J 1.2 sürümü appender Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j1_2%22) |applicationinsights-logging-log4j1_2 |


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

    <Configuration packages="com.microsoft.applicationinsights.log4j.v2">
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

*Log4J v1.2*

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

Özel durumlar submited günlükçüleri aracılığıyla özel durum Telemetrisi portalda görüntülenir.

![Application Insights portalında arama açın](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>Sonraki adımlar
[Tanılama arama][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


