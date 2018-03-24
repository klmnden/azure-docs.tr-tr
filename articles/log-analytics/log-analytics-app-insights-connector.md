---
title: Azure Application Insights uygulama verileri görüntüleme | Microsoft Docs
description: Performans sorunlarını tanılamak ve Application Insights ile izlenen uygulamanızla kullanıcıların ne anlamak için uygulama Öngörüler Bağlayıcısı çözüm kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: 49280cad-3526-43e1-a365-c6a3bf66db52
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: magoedte
ms.openlocfilehash: 854ec70c897b6a561fdec056228f82ccec3ae16c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="application-insights-connector-management-solution-preview"></a>Uygulama Öngörüler Bağlayıcısı yönetim çözümü (Önizleme)

![Uygulama Öngörüler simgesi](./media/log-analytics-app-insights-connector/app-insights-connector-symbol.png)

Uygulamaları Öngörüler Bağlayıcısı çözüm, performans sorunlarını tanılamak ve ile izlenen kullanıcılar ile uygulamanızı ne anlamanıza yardımcı olur [Application Insights](../application-insights/app-insights-overview.md). Application Insights'ta geliştiriciler bkz aynı uygulama telemetri görünümlerini günlük analizi içinde kullanılabilir. Ancak, Application Insights uygulamalarınızı günlük analizi ile tümleştirdiğinizde, uygulamalarınızı görünürlüğünü işlemi ve uygulama verilerini tek bir yerde sağlayarak artar. Aynı görünümleri olan, uygulama geliştiricilerine işbirliği yardımcı olur. Sık kullanılan görünümleri algılamak ve uygulama ve platform sorunları gidermek için zaman azaltmaya yardımcı olabilir.

Çözüm kullandığınızda aşağıdakileri yapabilirsiniz:

- Farklı Azure aboneliklerinde olsalar bile tüm Application Insights uygulamaları tek bir yerde görüntüleyin
- Uygulama verileri ile altyapı verilerin bağıntısını
- Günlük arama açılardan ile uygulama verileri görselleştirin
- Azure portalında Application Insights uygulamanız için günlük analizi verilerden Özet

## <a name="connected-sources"></a>Bağlı kaynaklar

Çoğu diğer günlük analizi çözümlerden farklı olarak uygulama Öngörüler Bağlayıcısı için aracıları tarafından toplanan veriler değil. Çözüm tarafından kullanılan tüm verileri doğrudan Azure'dan gelir.

| Bağlı Kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| [Windows aracıları](log-analytics-windows-agent.md) | Hayır | Çözüm Windows aracılardan bilgi toplamaz. |
| [Linux aracıları](log-analytics-linux-agents.md) | Hayır | Çözüm, Linux aracılarını bilgi toplamaz. |
| [SCOM yönetim grubu](log-analytics-om-agents.md) | Hayır | Çözüm bağlı SCOM yönetim grubunda aracılardan gelen bilgiler toplamaz. |
| [Azure depolama hesabı](log-analytics-azure-storage.md) | Hayır | Çözüm koleksiyonu bilgileri Azure depolama biriminden değil yapar. |

## <a name="prerequisites"></a>Önkoşullar

- Uygulama Öngörüler Bağlayıcısı bilgilere erişmek için bir Azure aboneliğinizin olması gerekir
- En az bir yapılandırılmış Application Insights kaynağı olması gerekir.
- Sahibi veya katkıda Application Insights kaynağı olmalıdır.

## <a name="configuration"></a>Yapılandırma

1. Azure Web Apps Analytics çözümden etkinleştirmek [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ApplicationInsights?tab=Overview) veya açıklanan işlemi kullanarak [Çözümleri Galerisi eklemek günlük analizi çözümleri](log-analytics-add-solutions.md).
2. OMS Portalı'nda tıklatın **ayarları** &gt; **veri** &gt; **Application Insights**.
3. Altında **bir abonelik seçin**, Application Insights kaynaklara sahip bir abonelik seçin ve ardından **uygulama adı**, bir veya daha fazla uygulamaları seçin.
4. **Kaydet**’e tıklayın.

Yaklaşık 30 dakika içinde veri kullanılabilir hale gelir ve Application Insights döşeme aşağıdaki görüntü gibi verilerle güncelleştirilir:

![Application Insights döşeme](./media/log-analytics-app-insights-connector/app-insights-tile.png)

Göz önünde bulundurmanız alınacak diğer noktalar:

- Bu gibi durumlarda, Application Insights uygulamalar yalnızca bir günlük analizi çalışma alanına bağlayabilirsiniz.
- Yalnızca bağlayabilirsiniz [Basic veya Enterprise Application Insights kaynağı](https://azure.microsoft.com/pricing/details/application-insights) günlük analizi için. Ancak, günlük analizi ücretsiz katmanı kullanabilirsiniz.

## <a name="management-packs"></a>Yönetim paketleri

Bu çözüm, bağlı yönetim gruplarında herhangi bir Yönetim Paketi yüklemez.

## <a name="use-the-solution"></a>Çözüm kullanın

Aşağıdaki bölümlerde görüntüleyip, uygulamalardan veri ile etkileşim için Application Insights Panoda gösterilen Kanatlar nasıl kullanabileceğinizi açıklar.

### <a name="view-application-insights-connector-information"></a>Uygulama Öngörüler Bağlayıcısı bilgilerini görüntüleme

Tıklatın **Application Insights** açmak için kutucuğa **Application Insights** aşağıdaki Kanatlar görmek için Pano.

![Uygulama öngörüleri Panosu](./media/log-analytics-app-insights-connector/app-insights-dash01.png)

![Uygulama öngörüleri Panosu](./media/log-analytics-app-insights-connector/app-insights-dash02.png)

Pano tabloda gösterilen Kanatlar içerir. Her dikey penceresinde belirtilen kapsam ve zaman aralığı için o dikey 's ölçütlerle eşleşen en fazla 10 öğeleri listeler. Tüm kayıtları tıkladığınızda döndüren bir günlük arama çalıştırabilirsiniz **tümünü görmek** alt dikey veya dikey başlığını tıklatın.

[!INCLUDE [log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

| **Sütun** | **Açıklama** |
| --- | --- |
| Uygulamalar - uygulama sayısı | Uygulama kaynaklarında uygulama sayısını gösterir. Ayrıca uygulama adlarını listeler ve her uygulama kayıt sayısı. Günlük aramasını çalıştırmak üzere numarasını tıklatın <code>ApplicationInsights &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName</code> <br><br>  Konak, telemetri türe göre kayıt ve tüm verileri (son gününde dayanarak) türüne göre uygulama kayıtları gösteren uygulama için bir günlük arama çalıştırmak için bir uygulama adına tıklayın. |
| Veri birimi – ana verileri gönderme | Verileri gönderme barındıran bilgisayar sayısını gösterir. Ayrıca barındıran bilgisayar ve her konak için kayıt sayısı listelenir. Günlük aramasını çalıştırmak üzere numarasını tıklatın <code>ApplicationInsights &#124; summarize AggregatedValue = sum(SampledCount) by Host</code> <br><br> Konak, telemetri türe göre kayıt ve tüm verileri (son gününde dayanarak) türüne göre uygulama kayıtları gösterir ana bilgisayar için bir günlük arama çalıştırmak için bir bilgisayar adına tıklayın. |
| Kullanılabilirlik – Web testini sonuçları | Web test sonuçları, geçişi veya başarısız olduğunu gösteren bir halka grafik gösterir. Günlük aramasını çalıştırmak için grafiği tıklatın <code>ApplicationInsights &#124; where TelemetryType == "Availability" &#124; summarize AggregatedValue = sum(SampledCount) by AvailabilityResult</code> <br><br> Sonuçları geçişleri ve tüm testleri için hata sayısını gösterir. Bu trafiği ile tüm Web uygulamaları için son dakika gösterir. Başarısız web testleri ayrıntılarını gösteren bir günlük arama görüntülemek için bir uygulama adına tıklayın. |
| Sunucu istekleri – saat başına istek sayısı | Sunucu istekleri çeşitli uygulamalar için saatte bir çizgi grafiğini gösterir. Zamanında bir noktası yönelik isteklerini almayı en üst 3 uygulamaları görmek için grafik bir satırda üzerine gelerek. Ayrıca seçili dönem için istekleri ve istek sayısı alma uygulamaların bir listesini gösterir. <br><br>Günlük aramasını çalıştırmak için grafiği tıklatın <code>ApplicationInsights &#124; where TelemetryType == "Request" &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> çeşitli uygulamalar için saat başına sunucu isteklerinin daha ayrıntılı bir çizgi grafiği gösterir. <br><br> Günlük aramasını çalıştırmak için listedeki bir uygulamaya tıklayın <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true</code> yanıt kodları istekleri, saat ve istek süresi üzerinden isteklerine ilişkin grafikleri ve istek listesini listesini gösterir.   |
| Hataları – saat başına başarısız istekleri | Başarısız uygulama isteklerini saatte bir çizgi grafiğini gösterir. Başarısız istekler için bir noktayı üst 3 uygulamalarla görmek için grafik üzerine gelerek. Ayrıca her biri için başarısız istek sayısı ile uygulamaların bir listesini gösterir. Günlük aramasını çalıştırmak için grafiği tıklatın <code>ApplicationInsights &#124; where TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> başarısız uygulama isteklerinin daha ayrıntılı bir çizgi grafiği gösterir. <br><br>Günlük aramasını çalıştırmak için listedeki bir öğeye tıklayın <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true</code> başarısız istekleri, grafiklerde gösterilmiştir başarısız istekler zaman ve istek süresi ve başarısız istek yanıt kodları listesini üzerinden. |
| Özel durumlar – saat başına özel durumlar | Saat başına özel durumların bir çizgi grafiği gösterir. Özel durumlar bir nokta için en üst 3 uygulamalarla görmek için grafik üzerine gelerek. Ayrıca her özel durum sayısı ile uygulamaların bir listesini gösterir. Günlük aramasını çalıştırmak için grafiği tıklatın <code>ApplicationInsights &#124; where TelemetryType == "Exception" &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> daha ayrıntılı bir bağlantı grafik özel durumların gösterir. <br><br>Günlük aramasını çalıştırmak için listedeki bir öğeye tıklayın <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Exception"</code> özel durumlar, zaman ve başarısız istekleri üzerinden özel durumlara grafiklerde ve özel durum türleri listesini listesini gösterir.  |

### <a name="view-the-application-insights-perspective-with-log-search"></a>Application Insights perspektif günlük arama ile görüntüleme

Panodaki herhangi bir öğeyi tıklattığınızda aramada gösterilen Application Insights perspektif bakın. Perspektif seçili telemetri türüne göre bir genişletilmiş görselleştirme sağlar. Bu nedenle, görselleştirme içerik için farklı telemetri türlerini değiştirir.

Uygulamalar dikey penceresinde herhangi bir yere tıkladığınızda, varsayılan bkz **uygulamaları** perspektif.

![Uygulama Öngörüler uygulamaları perspektifi](./media/log-analytics-app-insights-connector/applications-blade-drill-search.png)

Seçtiğiniz uygulamanın genel bir bakış açısından gösterir.

**Kullanılabilirlik** dikey web test sonuçlarını ve başarısız olan ilgili istekleri görebileceğiniz farklı perspektif görünümünü gösterir.

![Uygulama Öngörüler kullanılabilirlik perspektifi](./media/log-analytics-app-insights-connector/availability-blade-drill-search.png)

Tıkladığınızda herhangi bir yere **sunucu istekleri** veya **hataları** perspektif bileşenleri Kanatlar, değişiklik istekleriyle ilgili bir görsel öğe vermek için.

![Uygulama Öngörüler hataları dikey penceresi](./media/log-analytics-app-insights-connector/server-requests-failures-drill-search.png)

Tıkladığınızda herhangi bir yere **özel durumları** dikey penceresinde, özel durumlar için özel bir görsel öğe görürsünüz.

![Uygulama Öngörüler özel durumlar dikey penceresi](./media/log-analytics-app-insights-connector/exceptions-blade-drill-search.png)

Olup, bir şey birini tıklatın bağımsız olarak **uygulama Öngörüler Bağlayıcısı** Pano, içinde **arama** kendisi, Application Insights verileri gösteren uygulama döndüren herhangi bir sorgu sayfası Öngörüler perspektif. Örneğin, Application Insights veri görüntülüyorsanız bir **&#42;** sorgu ayrıca aşağıdaki görüntü gibi perspektif sekmesini gösterir:

![Application Insights ](./media/log-analytics-app-insights-connector/app-insights-search.png)

Perspektif bileşenleri arama sorgusu bağlı olarak güncelleştirilir. Başka bir deyişle, verileri görme olanağı verir herhangi bir arama alanı kullanarak sonuçları filtreleyebilirsiniz:

- Tüm uygulamalar
- Tek bir seçilen uygulama
- Bir uygulama grubu

### <a name="pivot-to-an-app-in-the-azure-portal"></a>Azure portalında bir uygulama için Özet

Uygulama Öngörüler Bağlayıcısı Kanatlar, seçili Application Insights uygulama Özet sağlamak için tasarlanmıştır *OMS Portalı'nı kullandığınızda,*. Bir uygulama gidermenize yardımcı olacak üst düzey bir izleme platform çözüm kullanabilirsiniz. Olası bir soruna bağlı uygulamalarınızı hiçbirinde gördüğünüzde, her iki ayrıntıya, günlük analizi arama yapabilir veya doğrudan Application Insights uygulama Özet.

Özet için üç noktaya tıklayın (**...** ), her satırın sonunda görünür ve seçin **Application Insights Aç**.

>[!NOTE]
>**Application Insights Aç** Azure portalında kullanılamaz.

![Application Insights Aç](./media/log-analytics-app-insights-connector/open-in-app-insights.png)

### <a name="sample-corrected-data"></a>Veri örnek düzeltildi

Application Insights sağlar *[düzeltme örnekleme](../application-insights/app-insights-sampling.md)* telemetri trafiğini azaltmak için. Application Insights uygulamanıza örnekleme etkinleştirdiğinizde, Application Insights hem de günlük analizi depolanan girişleri azaltılmış sayısını alır. Veri tutarlılığı içinde korunur sırada **uygulama Öngörüler Bağlayıcısı** sayfası ve Perspektifler, örneklenen verileri özel sorgularınızı el ile düzeltmeniz gerekir.

Örnekleme düzeltme günlük arama sorguda bir örneği burada verilmiştir:

```
ApplicationInsights | summarize AggregatedValue = sum(SampledCount) by TelemetryType
```

**Örneklenen sayısı** alan tüm girişler varsa ve girişini temsil eden veri noktası sayısını gösterir. Application Insights uygulamanız için örnekleme üzerinde kapatırsanız **örneklenen sayısı** 1'den büyük. Gerçek uygulamanız oluşturur girişleri sayısı için toplam **örneklenen sayısı** alanları.

Örnekleme uygulamanızı oluşturur girişleri yalnızca toplam sayısını etkiler. Ölçüm alanları örnekleme gidermek gerekmeyen **RequestDuration** veya **AvailabilityDuration** ortalama gösterilen girişleri için bu alanları göstermek için.

## <a name="input-data"></a>Giriş verisi

Çözüm, aşağıdaki veri telemetri türlerini bağlı Application Insights uygulamalardan alır:

- Kullanılabilirlik
- Özel durumlar
- İstekler
- Sayfa görünümleri – sayfa görünümleri almak çalışma alanınız için bu bilgileri toplamak için uygulamalarınızı yapılandırmanız gerekir. Daha fazla bilgi için bkz: [PageViews](../application-insights/app-insights-api-custom-events-metrics.md#page-views).
- Özel olaylar – özel olaylarını almak çalışma alanınız için bu bilgileri toplamak için uygulamalarınızı yapılandırmanız gerekir. Daha fazla bilgi için bkz: [TrackEvent](../application-insights/app-insights-api-custom-events-metrics.md#trackevent).

Kullanılabilir olduğunda veri Application Insights gelen günlük analizi tarafından alınır.

## <a name="output-data"></a>Çıktı verileri

Bir kayıtla bir *türü* , *Applicationınsights* her giriş veri türü için oluşturulur. Applicationınsights kayıtları, aşağıdaki bölümlerde gösterilen özelliklere sahiptir:

### <a name="generic-fields"></a>Genel alanlar

| Özellik | Açıklama |
| --- | --- |
| Tür | ApplicationInsights |
| ClientIP |   |
| TimeGenerated | Kayıt zamanı |
| ApplicationId | Application Insights uygulamasının izleme anahtarı |
| ApplicationName | Application Insights adını uygulama |
| RoleInstance | Sunucu ana bilgisayar kimliği |
| DeviceType | İstemci aygıtı |
| ScreenResolution |   |
| Kıta | İsteğin geldiği Kıtada |
| Ülke | İsteğin geldiği ülke |
| İl | Bölge, durum veya isteğin geldiği yerel ayar |
| Şehir | Şehir veya isteğin geldiği Şehir |
| isSynthetic | İstek bir kullanıcı veya otomatik olarak yöntemi tarafından oluşturulup oluşturulmadığını belirtir. Doğru oluşturulan kullanıcı = ya da yanlış = otomatik yöntemi |
| SamplingRate | Portala gönderilen SDK'sı tarafından oluşturulan telemetri yüzdesi. 0,0 100.0 aralığı. |
| SampledCount | 100/(SamplingRate). Örneğin, 4 =&gt; % 25 |
| IsAuthenticated | True veya false |
| OperationID | Kimliği ilgili öğeler Portalı'nda gösterilen aynı işlemi sahip öğeler. Genellikle istek kimliği |
| ParentOperationID | Üst işlem kimliği |
| OperationName |   |
| SessionID | İstek oluşturulduğu oturumunu benzersiz şekilde tanımlamak için GUID |
| SourceSystem | ApplicationInsights |

### <a name="availability-specific-fields"></a>Kullanılabilirlik özel alanlar

| Özellik | Açıklama |
| --- | --- |
| TelemetryType | Kullanılabilirlik |
| AvailabilityTestName | Web test adı |
| AvailabilityRunLocation | Http isteğinin coğrafi kaynak |
| AvailabilityResult | Web testi başarı sonucunu gösterir |
| AvailabilityMessage | Web testi bağlı ileti |
| AvailabilityCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| DataSizeMetricValue | 1.0 veya 0,0 |
| DataSizeMetricCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| AvailabilityDuration | Web testi sürenin milisaniye cinsinden süre |
| AvailabilityDurationCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| AvailabilityValue |   |
| AvailabilityMetricCount |   |
| AvailabilityTestId | Web testi için benzersiz bir GUID |
| AvailabilityTimestamp | Kullanılabilirlik testinin kesin zaman damgası |
| AvailabilityDurationMin | Örneklenen kayıtlar için bu alan gösterilen veri noktaları için minimum web test süresi (milisaniye cinsinden) gösterir. |
| AvailabilityDurationMax | Örneklenen kayıtlar için bu alan gösterilen veri noktaları için en büyük web test süresi (milisaniye cinsinden) gösterir. |
| AvailabilityDurationStdDev | Örneklenen kayıtlar için bu alan gösterilen veri noktaları için tüm web testi süre (milisaniye) arasında standart sapma gösterir. |
| AvailabilityMin |   |
| AvailabilityMax |   |
| AvailabilityStdDev | &nbsp;  |

### <a name="exception-specific-fields"></a>Özel durum özel alanlar

| Tür | ApplicationInsights |
| --- | --- |
| TelemetryType | Özel durum |
| ExceptionType | Özel durum türü |
| ExceptionMethod | Özel durumu oluşturan yöntemi |
| ExceptionAssembly | Ortak anahtar belirteci yanı sıra framework ve sürüm derleme içerir |
| ExceptionGroup | Özel durum türü |
| ExceptionHandledAt | Özel durumun işlenip düzeyini gösterir |
| ExceptionCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| ExceptionMessage | Özel durum iletisi |
| ExceptionStack | Tam özel durum yığını |
| ExceptionHasStack | Özel durum yığın varsa true |



### <a name="request-specific-fields"></a>İsteği özel alanlar

| Özellik | Açıklama |
| --- | --- |
| Tür | ApplicationInsights |
| TelemetryType | İstek |
| Yanıt kodu | İstemciye gönderilen HTTP yanıtı |
| RequestSuccess | Başarı veya başarısızlık gösterir. TRUE veya false. |
| RequestId | İstek benzersiz şekilde tanımlamak için kimliği |
| RequestName | GET/POST + URL tabanı |
| RequestDuration | İstek süresini saniye cinsinden süre |
| URL'si | Ana bilgisayar hariç istek URL'si |
| Host | Web sunucusu ana bilgisayar |
| URLBase | Tam istek URL'si |
| ApplicationProtocol | Uygulama tarafından kullanılan protokol türü |
| RequestCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| RequestDurationCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| RequestDurationMin | Örneklenen kayıtlar için bu alan için gösterilen veri noktalarının en düşük istek süresi (milisaniye) gösterir. |
| RequestDurationMax | Örneklenen kayıtlar için bu alan en fazla istek süresi (milisaniye) için gösterilen veri noktalarını gösterir |
| RequestDurationStdDev | Örneklenen kayıtlar için bu alan tüm istek süre (milisaniye) arasında standart sapma için gösterilen veri noktalarını gösterir |

## <a name="sample-log-searches"></a>Örnek günlük aramaları

Bu çözüm panosunda gösterilen örnek günlük aramaları kümesi yok. Ancak, örnek günlük arama sorguları açıklamalarını gösterilen [görünüm uygulama Öngörüler Bağlayıcısı bilgi](#view-application-insights-connector-information) bölümü.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım [günlük arama](log-analytics-log-searches.md) Application Insights uygulamalarınız için ayrıntılı bilgileri görüntülemek için.
