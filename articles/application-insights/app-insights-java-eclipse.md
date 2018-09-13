---
title: Eclipse'te Java ile Azure Application Insights ile çalışmaya başlama | Microsoft docs
description: Performans ve kullanım için Application Insights ile Java Web sitenizi izleme eklemek için Eclipse eklentisini kullanma
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: e88c9f53-cd90-4abc-b097-1f170937908e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 12/12/2016
ms.author: mbullwin
ms.openlocfilehash: ebcfe02eb8d969af26f5121bda85e4610302e838
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647591"
---
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a>Eclipse'te Java ile Application ınsights'ı kullanmaya başlama
Kullanımını ve performansını analiz etmek Application Insights SDK'sı, Java web uygulamanızdan telemetri gönderir. Kutusu telemetri yanı sıra özel telemetri yazma için kullanabileceğiniz bir API dışında elde etmeniz için Application Insights eklentisini Eclipse SDK projenize otomatik olarak yükler.   

## <a name="prerequisites"></a>Önkoşullar
Şu anda eklenti çalıştığı Maven projeleriyle ve eclipse'te dinamik Web projeleri için.
([Diğer türde bir Java projesi için Application Insights Ekle][java].)

Gerekenler:

* JRE 1.7 veya 1.8
* Bir [Microsoft Azure](https://azure.microsoft.com/) aboneliği.
* [Java EE geliştiricileri için Eclipse IDE](http://www.eclipse.org/downloads/), Indigo veya üzeri.
* Windows 7 veya üzeri ya da Windows Server 2008 veya üstü

Spring çerçevesini tercih ediyorsanız, [Application Insights kılavuzunu kullanmak için Spring Boot başlatıcı uygulamasını yapılandırmayı](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-java-applicationinsights) deneyin

## <a name="install-the-sdk-on-eclipse-one-time"></a>(Bir kez) üzerinde Eclipse SDK'sını yükleyin
Yalnızca bunu makine başına bir kez yapmanız gerekir. Bu adım, SDK her dinamik Web projesi için ardından ekleyebileceğiniz bir araç seti yükler.

1. Eclipse'te, Yardım, yeni yazılım Yükle'ye tıklayın.

    ![Yardım, yeni yazılım yükle](./media/app-insights-java-eclipse/0-plugin.png)
2. SDK'sı bulunduğu http://dl.microsoft.com/eclipse, Azure Araç Seti altında.
3. Onay kutusunu temizleyin **tüm güncelleştirme siteleriyle iletişim...**

    ![Application Insights SDK için clear tüm güncelleştirme siteleriyle iletişime geçin](./media/app-insights-java-eclipse/1-plugin.png)

Her bir Java projesi kalan adımları izleyin.

## <a name="create-an-application-insights-resource-in-azure"></a>Azure'da Application Insights kaynağı oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Yeni Application Insights kaynağı oluşturma Uygulama türünü Java web uygulaması olarak ayarlayın.  

    ![Tıklama + ve Application Insights seçme](./media/app-insights-java-eclipse/01-create.png)  

4. Yeni kaynağın izleme anahtarını bulun. Bunu hemen kod projenize yapıştırmalısınız.  

    ![Yeni kaynağa genel bakışta, Özellikler'e tıklayıp izleme anahtarını kopyalama](./media/app-insights-java-eclipse/03-key.png)  

## <a name="add-application-insights-to-your-project"></a>Projenize Application Insights ekleyin
1. Application Insights Java web projenizin bağlam menüsünden ekleyin.

    ![Yeni kaynağa genel bakışta, Özellikler'e tıklayıp izleme anahtarını kopyalama](./media/app-insights-java-eclipse/02-context-menu.png)
2. Azure portalından aldığınız izleme anahtarını yapıştırın.

    ![Yeni kaynağa genel bakışta, Özellikler'e tıklayıp izleme anahtarını kopyalama](./media/app-insights-java-eclipse/03-ikey.png)

Anahtarı telemetrinin her öğesine birlikte gönderilir ve Application Insights'ın bunu kaynağınızda görüntülenecek söyler.

## <a name="run-the-application-and-see-metrics"></a>Uygulamayı çalıştırmak ve ölçümleri görme
Uygulamanızı çalıştırın.

Microsoft azure'da Application Insights kaynağınıza dönün.

HTTP verilerin genel bakış dikey penceresinde görüntülenmesini ister. (Orada değilse, birkaç saniye bekleyip Yenile’ye tıklayın.)

![Sunucu yanıtı, istek sayısını ve hatalar ](./media/app-insights-java-eclipse/5-results.png)

Daha ayrıntılı ölçümler görmek için herhangi bir grafiğe tıklayın.

![İstek adına göre sayar](./media/app-insights-java-eclipse/6-barchart.png)

[Ölçümler hakkında daha fazla bilgi edinin.][metrics]

Ve istek özellikleri görüntülendiğinde, istekler ve özel durumlar gibi bununla ilişkili telemetri olayları görebilirsiniz.

![Bu istek için tüm izlemeler](./media/app-insights-java-eclipse/7-instance.png)

## <a name="client-side-telemetry"></a>İstemci tarafı telemetri
Hızlı Başlangıç dikey penceresinden, web sayfalarımı izlemeyi sağlayan kodu Al'ı tıklatın:

![Uygulamaya genel bakış dikey pencerenizde, Hızlı Başlat, Web sayfalarımı izlemeyi sağlayan kodu al'ı seçin. Betiği kopyalayın.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

HTML dosyaları Başkanı kod parçacığını ekleyin.

#### <a name="view-client-side-data"></a>İstemci tarafı verileri görüntüleme
Güncelleştirilmiş web sayfalarınıza açın ve bunları kullanın. Bir veya iki, dakika bekleyin, sonra Application Insights'a dönün ve kullanım dikey penceresini açın. (Genel bakış dikey penceresinde, aşağı kaydırın ve kullanım'a tıklayın.)

Sayfa görünümü ve kullanıcı oturumu ölçümleri kullanım dikey penceresinde görünür:

![Oturumlar, kullanıcılar ve sayfa görüntülemeleri](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[İstemci tarafı telemetri verileri ayarlama hakkında daha fazla bilgi edinin.][usage]

## <a name="publish-your-application"></a>Uygulamanızı yayımlama
Artık uygulamanızı sunucuya yayımlayın, herkesin kullanmasını sağlayın ve portalda gösterilen telemetriye bakın.

* Güvenlik duvarınızın, uygulamanıza şu bağlantı noktalarına telemetri göndermesine izin verdiğinden emin olun:

  * dc.services.visualstudio.com:443
  * dc.services.visualstudio.com:80
  * f5.services.visualstudio.com:443
  * f5.services.visualstudio.com:80
* Windows sunucularda yüklenecekler:

  * [Microsoft Visual C++ Yeniden Dağıtılabilir](http://www.microsoft.com/download/details.aspx?id=40784)

    (Performans sayaçlarını etkinleştirir.)

## <a name="exceptions-and-request-failures"></a>Özel durumlar ve istek hataları
İşlenmeyen özel durumlar otomatik olarak toplanır:

![](./media/app-insights-java-eclipse/21-exceptions.png)

Diğer özel durumlar hakkında veri toplamak için iki seçeneğiniz vardır:

* [Kodunuzda TrackException çağrıları Ekle](app-insights-api-custom-events-metrics.md#trackexception).
* [Sunucunuza Java Agent yükleme](app-insights-java-agent.md). İzlemek istediğiniz yöntemleri belirtin.

## <a name="monitor-method-calls-and-external-dependencies"></a>Yöntem çağrılarını ve dış bağımlılıkları izleme
Zamanlama verileriyle JDBC üzerinden yapılan belirli dahili yöntemleri ve çağrıları kaydetmek için [Java Agent yükleme](app-insights-java-agent.md) işlemini gerçekleştirin.

## <a name="performance-counters"></a>Performans sayaçları
Genel Bakış dikey penceresinde, aşağı kaydırın ve tıklayın **sunucuları** Döşe. Bir dizi performans sayacı göreceksiniz.

![Sunucuları kutucuğuna tıklayın aşağı kaydır](./media/app-insights-java-eclipse/11-perf-counters.png)

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

![](./media/app-insights-java-eclipse/12-custom-perfs.png)

### <a name="unix-performance-counters"></a>Unix Performans sayaçları
* Çok çeşitli sistem ve ağ verisi almak için [Application Insights eklentisiyle collectd yükleyin](app-insights-java-collectd.md).

## <a name="availability-web-tests"></a>Kullanılabilirlik web testleri
Kullanıma hazır ve düzgün yanıt verdiğini denetlemek için Application Insights belirli aralıklarla web sitenizi test edebilir. [Ayarlamak için][availability], kullanılabilirlik tıklayın için aşağı kaydırın.

![Aşağı kaydırıp, Kullanılabilirlik’e ve Web testi ekle’ye tıklama](./media/app-insights-java-eclipse/31-config-web-test.png)

Yanıt süreleri grafiklerine ek olarak, siteniz devre dışı kalırsa e-posta bildirimleri de alacaksınız.

![Web testi örneği](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[Kullanılabilirlik web testleri hakkında daha fazla bilgi edinin.][availability]

## <a name="diagnostic-logs"></a>Tanılama günlükleri
Logback veya Log4J kullanıyorsanız (v1.2 veya v2.0) için izleme, otomatik olarak burada keşfedin ve bunlar üzerinde arama Application ınsights'a gönderilen izleme günlüklerinizi sahip olabilir.

[Tanılama günlükleri hakkında daha fazla bilgi edinin][javalogs]

## <a name="custom-telemetry"></a>Özel telemetri
Java web uygulamanıza hangi kullanıcılar ile nasıl kullandığını görün veya sorunların tanılanmasına yardımcı olmak için birkaç satırlık bir kod ekleyin.

JavaScript web sayfası hem de sunucu tarafı Java kod ekleyebilirsiniz.

[Özel telemetri hakkında bilgi edinin][track]

## <a name="next-steps"></a>Sonraki adımlar
#### <a name="detect-and-diagnose-issues"></a>Sorunları algılayın ve tanılayın
* [Web istemcisi telemetrisini ekleyin] [ usage] performans telemetrisine web istemcisinden erişin.
* Uygulamanızın canlı ve duyarlı kaldığından emin olmak için [web testleri oluşturun][availability].
* Sorunların tanımlanması için [Olayları ve günlükleri arayın][diagnostic].
* [Log4J veya Logback izlerini yakalama][javalogs]

#### <a name="track-usage"></a>Kullanımı İzleme
* [Web istemcisi telemetrisini ekleyin] [ usage] sayfa görünümlerini ve temel kullanıcı ölçümlerini izlemek için.
* [Özel olayları ve ölçümleri izleyin](app-insights-web-track-usage.md) nasıl uygulamanız, hem istemci ve sunucu kullanıldığı hakkında bilgi edinmek için.

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md
