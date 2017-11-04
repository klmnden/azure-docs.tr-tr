---
title: "Azure Application Insights ile Java eclipse'te kullanmaya başlama | Microsoft docs"
description: "Eklenti Eclipse performansı ve Application Insights ile Java Web sitenize kullanımı izleme eklemek için kullanın"
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: e88c9f53-cd90-4abc-b097-1f170937908e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: mbullwin
ms.openlocfilehash: 616cbfed405454d2abbb6bb526166d2c72e4365d
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a>Application Insights ile Java eclipse'te kullanmaya başlama
Application Insights SDK'sı, telemetri Java web uygulamanızı gönderir kullanımını ve performansını analiz edin. Böylece kutusunu telemetri artı özel telemetri yazmak için kullanabileceğiniz bir API dışında almak için Application Insights eklenti Eclipse SDK projenize otomatik olarak yükler.   

## <a name="prerequisites"></a>Ön koşullar
Şu anda eklenti works Maven projelerini ve Eclipse dinamik Web projeleri için.
([Java projesi diğer türleri için Application Insights Ekle][java].)

Gerekenler:

* Oracle JRE 1.6 veya sonraki sürümleri
* Bir [Microsoft Azure](https://azure.microsoft.com/) aboneliği.
* [Java EE geliştiricileri için Eclipse IDE](http://www.eclipse.org/downloads/), Indigo olarak biliniyordu veya üzeri.
* Windows 7 veya üzeri ya da Windows Server 2008 veya üzeri

## <a name="install-the-sdk-on-eclipse-one-time"></a>Eclipse (bir kez) üzerinde SDK'sını yükleyin
Yalnızca bu makine başına bir kez yapmanız gerekir. Bu adım, daha sonra SDK her dinamik Web projesi ekleyebilirsiniz bir araç yükler.

1. Eclipse'te, Yardım, yeni yazılım Yükle'yi tıklatın.

    ![Yardım, yeni yazılım yükleme](./media/app-insights-java-eclipse/0-plugin.png)
2. SDK Azure Araç Seti altında http://dl.microsoft.com/eclipse kullanılıyor.
3. İşaretini **tüm güncelleştirme siteleri başvurun...**

    ![Application Insights SDK'sı, temizlemek için kişi tüm siteleri güncelleştir](./media/app-insights-java-eclipse/1-plugin.png)

Her bir Java projesi için kalan adımları izleyin.

## <a name="create-an-application-insights-resource-in-azure"></a>Application Insights kaynağı oluşturma
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Yeni Application Insights kaynağı oluşturma Uygulama türünü Java web uygulaması olarak ayarlayın.  

    ![Tıklama + ve Application Insights seçme](./media/app-insights-java-eclipse/01-create.png)  

4. Yeni kaynağın izleme anahtarını bulun. Bunu hemen kod projenize yapıştırmalısınız.  

    ![Yeni kaynağa genel bakışta, Özellikler'e tıklayıp izleme anahtarını kopyalama](./media/app-insights-java-eclipse/03-key.png)  

## <a name="add-application-insights-to-your-project"></a>Projenize Application Insights ekleyin
1. Application Insights Java web projeniz bağlam menüsünden ekleyin.

    ![Yeni kaynağa genel bakışta, Özellikler'e tıklayıp izleme anahtarını kopyalama](./media/app-insights-java-eclipse/02-context-menu.png)
2. Azure portalından aldığınız izleme anahtarını yapıştırın.

    ![Yeni kaynağa genel bakışta, Özellikler'e tıklayıp izleme anahtarını kopyalama](./media/app-insights-java-eclipse/03-ikey.png)

Anahtarı telemetrinin her öğesiyle birlikte gönderilir ve Application Insights kaynağınıza içinde görüntülemek için söyler.

## <a name="run-the-application-and-see-metrics"></a>Uygulamayı çalıştırın ve ölçümleri bakın
Uygulamanızı çalıştırın.

Microsoft Azure Application Insights kaynağınıza dönün.

HTTP verilerin genel bakış dikey penceresinde görüntülenmesini ister. (Orada değilse, birkaç saniye bekleyip Yenile’ye tıklayın.)

![Sunucu yanıtı, istek sayısını ve hataları ](./media/app-insights-java-eclipse/5-results.png)

Daha ayrıntılı ölçümler görmek için herhangi bir grafiğe tıklayın.

![Ada göre istek sayısı](./media/app-insights-java-eclipse/6-barchart.png)

[Ölçümler hakkında daha fazla bilgi edinin.][metrics]

Ve istek özellikleri görüntülendiğinde, istekler ve özel durumlar gibi ilişkili telemetri olayları görebilirsiniz.

![Bu istek için tüm izlemeleri](./media/app-insights-java-eclipse/7-instance.png)

## <a name="client-side-telemetry"></a>İstemci tarafı telemetri
Hızlı Başlangıç dikey penceresinden web sayfalarımı izlemeyi sağlayan kodu Al'ı tıklatın:

![Uygulamaya genel bakış dikey pencerenizde, Hızlı Başlat, Web sayfalarımı izlemeyi sağlayan kodu al'ı seçin. Betiği kopyalayın.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

Kod parçacığı, HTML dosyaları head içinde ekler.

#### <a name="view-client-side-data"></a>İstemci tarafı verileri görüntüleme
Güncelleştirilmiş web sayfalarınıza açın ve bunları kullanın. Veya iki dakika bekleyin, sonra Application Insights'a dönün ve kullanım dikey penceresini açın. (Genel bakış dikey penceresinden aşağı kaydırın ve kullanımı'nı tıklatın.)

Sayfa görünümü, kullanıcı ve oturum ölçümleri kullanım dikey penceresinde görünür:

![Oturumlar, kullanıcılar ve sayfa görünümleri](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[İstemci tarafı telemetri için ayarlama hakkında daha fazla bilgi edinin.][usage]

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

* [Kodunuzda TrackException çağrıları ekleme](app-insights-api-custom-events-metrics.md#trackexception).
* [Sunucunuza Java Agent yükleme](app-insights-java-agent.md). İzlemek istediğiniz yöntemleri belirtin.

## <a name="monitor-method-calls-and-external-dependencies"></a>Yöntem çağrılarını ve dış bağımlılıkları izleme
Zamanlama verileriyle JDBC üzerinden yapılan belirli dahili yöntemleri ve çağrıları kaydetmek için [Java Agent yükleme](app-insights-java-agent.md) işlemini gerçekleştirin.

## <a name="performance-counters"></a>Performans sayaçları
Genel Bakış dikey penceresini aşağı kaydırarak ve tıklayın **sunucuları** döşeme. Bir dizi performans sayacı göreceksiniz.

![Sunucular bölmesi tıklatın aşağı kaydırın](./media/app-insights-java-eclipse/11-perf-counters.png)

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
Kullanıma hazır ve düzgün yanıt verdiğini denetlemek için Application Insights belirli aralıklarla web sitenizi test edebilir. [Ayarlamak için][availability], kullanılabilirlik tıklattığınızdan aşağı kaydırın.

![Aşağı kaydırıp, Kullanılabilirlik’e ve Web testi ekle’ye tıklama](./media/app-insights-java-eclipse/31-config-web-test.png)

Yanıt süreleri grafiklerine ek olarak, siteniz devre dışı kalırsa e-posta bildirimleri de alacaksınız.

![Web testi örneği](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[Kullanılabilirlik web testleri hakkında daha fazla bilgi edinin.][availability]

## <a name="diagnostic-logs"></a>Tanılama günlükleri
Logback veya Log4J kullanıyorsanız (1.2 sürümü veya v2.0) için izlemeyi, izleme günlüklerinizi uygulama burada keşfedin ve bunlar üzerinde arama Öngörüler otomatik olarak gönderilen sahip olabilir.

[Tanılama günlükleri hakkında daha fazla bilgi edinin][javalogs]

## <a name="custom-telemetry"></a>Özel telemetri
Java web uygulamanızdaki kullanıcıların ne ile yaptıklarını bulmak için veya sorunlarını tanılamaya yardımcı olmak için birkaç satır kod ekleyin.

Kod, web sayfası JavaScript hem sunucu tarafı Java de ekleyebilirsiniz.

[Özel telemetri hakkında bilgi edinin][track]

## <a name="next-steps"></a>Sonraki adımlar
#### <a name="detect-and-diagnose-issues"></a>Sorunlarını tanılamak ve Algıla
* [Web istemcisi telemetrisini ekleyin] [ usage] performans telemetrisini web istemcisinde almanın.
* Uygulamanızın canlı ve duyarlı kaldığından emin olmak için [web testleri oluşturun][availability].
* Sorunların tanımlanması için [Olayları ve günlükleri arayın][diagnostic].
* [Log4J veya Logback izlemelerini yakalama][javalogs]

#### <a name="track-usage"></a>Kullanımı İzleme
* [Web istemcisi telemetrisini ekleyin] [ usage] İzleyici sayfa görünümleri ve temel kullanıcı ölçümleri.
* [Özel olayları ve ölçümleri izleme](app-insights-web-track-usage.md) nasıl uygulamanız, hem istemci ve sunucu kullanıldığı hakkında bilgi edinmek için.

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md
