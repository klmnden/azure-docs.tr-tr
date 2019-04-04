---
title: Azure Application ınsights'ı uygulama verileri görüntüleme | Microsoft Docs
description: Performans sorunlarını tanılamak ve Application Insights ile izlenen uygulamanızla kullanıcıların ne yaptığını anlamak için Application Insights Bağlayıcısı çözümü kullanabilirsiniz.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 49280cad-3526-43e1-a365-c6a3bf66db52
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: magoedte
ms.openlocfilehash: aa1bb62e762925dcb5a0ee37b71602094e768137
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905707"
---
# <a name="application-insights-connector-management-solution-deprecated"></a>Application Insights Bağlayıcısı yönetim çözümü (kullanım dışı)

![Application Insights simgesi](./media/app-insights-connector/app-insights-connector-symbol.png)

>[!NOTE]
> Desteğiyle [kaynaklar arası sorgular](../../azure-monitor/log-query/cross-workspace-query.md), Application Insights Bağlayıcısı yönetim çözümü artık gerekli değildir. Bu olduğundan kullanım dışı ve 15 Ocak 2019 için Azure ticari bulutundaki resmi olarak kullanım dışı bırakılmış OMS portalında yanı sıra Azure Marketi'nde kaldırıldı. Azure ABD kamu bulutu için 30 Mart 2019 üzerinde bırakılacaktır.
>
>Varolan bağlantılar 30 Haziran 2019 kadar çalışmaya devam eder.  OMS portalı kullanımdan kaldırma ile yapılandırmak ve mevcut bağlantıları Portalı'ndan kaldırmak için hiçbir yolu yoktur. Bkz [PowerShell ile bağlayıcının çıkarılması](#removing-the-connector-with-powershell) aşağıda mevcut bağlantıları kaldırmak için PowerShell kullanma hakkında bir komut dosyası.
>
>Application Insights'ı sorgulama hakkında rehberlik için birden fazla uygulama için verileri günlüğe kaydetmek için bkz: [birden çok Azure İzleyici Application Insights kaynaklarını birleştirin](../log-query/unify-app-resource-data.md). OMS portalı kullanımdan kaldırma hakkında daha fazla bilgi için bkz. [Azure'a taşıyarak OMS portalında](../../azure-monitor/platform/oms-portal-transition.md).
>
> 

Uygulama öngörüleri Bağlayıcısı çözümü performans sorunlarını tanılayın ve ile izlenen kullanıcıların uygulamanızla ne yaptığını anlamanıza yardımcı olur. [Application Insights](../../azure-monitor/app/app-insights-overview.md). Log Analytics'te Application Insights'ta geliştiricilerin gördüğü aynı uygulama telemetrisini görünümlerini kullanılabilir. Ancak, Application Insights uygulamalarınızı Log Analytics ile tümleştirdiğinizde, uygulamalarınızın görünürlüğünü işlemi ve uygulama verilerini tek bir yerde sağlayarak artar. Aynı görünümleri olan, uygulama geliştiricilere işbirliği yapmasına yardımcı olur. Sık kullanılan görünümler, algılamak ve hem uygulama hem de platformu sorunları çözmek için gereken süreyi azaltmaya yardımcı olabilir.

Çözüm kullandığınızda şunları yapabilirsiniz:

- Farklı Azure aboneliklerinde olduklarında bile tüm Application Insights uygulamalarınızı tek bir yerde görüntüleyin.
- Uygulama verileri altyapı verilerle ilişkilendirmek
- Günlük araması'nda Perspektifler ile uygulama verileri Görselleştirme
- Application ınsights'ı uygulamanıza Azure portalında log Analytics verilerinden Özet


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="connected-sources"></a>Bağlı kaynaklar

Çoğu diğer Log Analytics çözümleri, aracıları tarafından için Application Insights Bağlayıcısı verileri toplanmaz. Çözüm tarafından kullanılan tüm verileri doğrudan Azure gelir.

| Bağlı Kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| [Windows aracıları](../../azure-monitor/platform/agent-windows.md) | Hayır | Çözüm, Windows aracılarından bilgi toplamaz. |
| [Linux aracıları](../../azure-monitor/learn/quick-collect-linux-computer.md) | Hayır | Çözüm, Linux aracılarından bilgi toplamaz. |
| [SCOM yönetim grubu](../../azure-monitor/platform/om-agents.md) | Hayır | Bir bağlı SCOM yönetim grubundaki aracılardan çözüm herhangi bir bilgi toplamaz. |
| [Azure depolama hesabı](collect-azure-metrics-logs.md) | Hayır | Çözüm, Azure depolama biriminden bilgilerin toplanması yapar. |

## <a name="prerequisites"></a>Önkoşullar

- Application Insights Bağlayıcısı bilgilere erişmek için bir Azure aboneliğinizin olması gerekir
- Yapılandırılan en az bir Application Insights kaynağı olmalıdır.
- Sahibi veya katkıda bulunan Application Insights kaynağına ait olmalıdır.

## <a name="configuration"></a>Yapılandırma

1. Azure Web Apps Analytics çözümü etkinleştirme [Azure Market](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AppInsights?tab=Overview) veya açıklanan işlemi kullanarak [Log Analytics çözümleri ekleme çözüm Galerisi'ndeki](../../azure-monitor/insights/solutions.md).
2. [Azure portala](https://portal.azure.com) gidin. Seçin **tüm hizmetleri** Application Insights'ı açın. Ardından Application ınsights'ı arayın. 
3. Altında **abonelikleri**, Application Insights kaynaklarını içeren bir aboneliği seçin ve ardından altındaki **adı**, bir veya daha fazla uygulama seçin.
4. **Kaydet**’e tıklayın.

Yaklaşık 30 dakika içinde veriler kullanılabilir duruma gelir ve Application Insights kutucuğunu aşağıdaki görüntü gibi verilerle güncelleştirilir:

![Application Insights kutucuğunu](./media/app-insights-connector/app-insights-tile.png)

Akılda tutulması gereken diğer noktalar:

- Bu gibi durumlarda, Application Insights uygulamaları yalnızca bir Log Analytics çalışma alanına bağlayabilirsiniz.
- Yalnızca bağlayabilirsiniz [temel veya Enterprise Application Insights kaynakları](https://azure.microsoft.com/pricing/details/application-insights) Log analytics'e. Bununla birlikte, ücretsiz katmanda Log analytics'in kullanabilirsiniz.

## <a name="management-packs"></a>Yönetim paketleri

Bu çözüm, bağlı yönetim gruplarında yönetim paketleri yüklemez.

## <a name="use-the-solution"></a>Çözüm kullanın

Aşağıdaki bölümlerde, görüntülemek ve uygulamalarınızdan verilerle etkileşimde bulunmak için Application Insights panosunda gösterilen dikey pencereleri nasıl kullanabileceğiniz açıklanmaktadır.

### <a name="view-application-insights-connector-information"></a>Application Insights Bağlayıcısı bilgilerini görüntüle

Tıklayın **Application Insights** açmak için kutucuğa **Application Insights** aşağıdaki dikey pencereleri görmek için Pano.

![Application Insights Panosu](./media/app-insights-connector/app-insights-dash01.png)

![Application Insights Panosu](./media/app-insights-connector/app-insights-dash02.png)

Pano tablosunda gösterilen dikey pencereleri içerir. Her dikey pencerede, dikey pencerenin belirtilen kapsam ve zaman aralığına yönelik ölçütleriyle eşleşen en fazla 10 öğe listelenir. ' A tıkladığınızda, tüm kayıtları döndüren bir günlük araması çalıştırabilirsiniz **tümünü gör** alt dikey penceresinin veya dikey pencere üst bilgisine tıklayın.


| **Sütun** | **Açıklama** |
| --- | --- |
| Uygulamalar - uygulama sayısı | Uygulama kaynaklarında uygulama sayısını gösterir. Ayrıca uygulama adlarını listeler ve her uygulama kayıt sayısını verir. Günlük araması çalıştırmak için numarasına tıklayın <code>ApplicationInsights &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName</code> <br><br>  Uygulama kayıtları başına konak, telemetri türüne göre kayıtlar ve tüm verileri (son gününe göre) türüne göre gösteren uygulama için bir günlük araması gerçekleştirmek için bir uygulama adına tıklayın. |
| Veri hacmi – veri gönderen konaklar | Veri gönderen barındıran bilgisayar sayısını gösterir. Ayrıca, ana bilgisayar ve her konak için kayıt sayısı listelenir. Günlük araması çalıştırmak için numarasına tıklayın <code>ApplicationInsights &#124; summarize AggregatedValue = sum(SampledCount) by Host</code> <br><br> Uygulama kayıtları başına konak, telemetri türüne göre kayıtlar ve tüm verileri (son gününe göre) türüne göre gösteren konak için bir günlük araması gerçekleştirmek için bir bilgisayar adına tıklayın. |
| Kullanılabilirlik – Web testi sonuçları | İçin web test sonuçları, Geçti veya başarısız gösteren bir halka grafik gösterir. Günlük araması çalıştırmak için bir grafiğe tıklayın <code>ApplicationInsights &#124; where TelemetryType == "Availability" &#124; summarize AggregatedValue = sum(SampledCount) by AvailabilityResult</code> <br><br> Sonuçları geçişleri ve tüm testler için hataları sayısını gösterir. Bu, son dakika için trafiği ile tüm Web uygulamaları gösterir. Başarısız web testi ayrıntılarını gösteren bir günlük araması'nı görüntülemek için bir uygulama adına tıklayın. |
| Sunucu istekleri – saat başına istek | Çeşitli uygulamalar için saat başına sunucu isteklerinin bir çizgi grafik gösterir. Zaman noktasına yönelik istekleri almaya ilk 3 uygulama görmek için grafikteki bir satır üzerine gelin. Ayrıca istek ve istek sayısı, seçilen süre için alma uygulamaların bir listesini gösterir. <br><br>Günlük araması çalıştırmak için grafiği tıklatın <code>ApplicationInsights &#124; where TelemetryType == "Request" &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> çeşitli uygulamalar için saat başına sunucu isteklerinin daha ayrıntılı bir çizgi grafik gösterir. <br><br> İçin bir günlük araması gerçekleştirmek için listede uygulamaya tıklayın <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true</code> yanıt kodları listesini istek, istek süresi ve istek süresini grafiklerde ve istek listesini gösterir.   |
| Hataları – saat başına başarısız istek | Saat başına başarısız uygulama isteklerinin bir çizgi grafik gösterir. Başarısız istekler için bir nokta ile ilk 3 uygulamalarını görmek için grafik üzerinde gezdirin. Ayrıca, her biri için başarısız istek sayısı ile uygulamaların bir listesini gösterir. Günlük araması çalıştırmak için grafiği tıklatın <code>ApplicationInsights &#124; where TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> başarısız uygulama isteklerinin daha ayrıntılı bir çizgi grafik gösterir. <br><br>Günlük araması çalıştırmak için listedeki bir öğeye tıklayın <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Request" and iff(isnotnull(toint(RequestSuccess)), RequestSuccess == false, RequestSuccess == "false") == true</code> gösterir başarısız istekler grafiklerinde başarısız istekler zaman ve istek süresini ve başarısız istek yanıt kodlarının bir listesi üzerinden. |
| Özel durumlar: özel durumlar / saat | Saat başına özel durum içeren bir çizgi grafik gösterir. Bir nokta için özel durumlar üst 3 uygulamaları görmek için grafik üzerinde gezdirin. Ayrıca, her biri için özel durumların sayısı ile uygulamaların bir listesini gösterir. Günlük araması çalıştırmak için grafiği tıklatın <code>ApplicationInsights &#124; where TelemetryType == "Exception" &#124; summarize AggregatedValue = sum(SampledCount) by ApplicationName, bin(TimeGenerated, 1h)</code> özel durumların daha ayrıntılı bir bağlantısını grafiği gösterir. <br><br>Günlük araması çalıştırmak için listedeki bir öğeye tıklayın <code>ApplicationInsights &#124; where ApplicationName == "yourapplicationname" and TelemetryType == "Exception"</code> , özel durumlar, özel durumlar üzerinden zaman ve başarısız istekler için grafikler ve özel durum türleri listesini listesini gösterir.  |

### <a name="view-the-application-insights-perspective-with-log-search"></a>Günlük araması ile Application Insights perspektif görüntüleyin

Pano herhangi bir öğeye tıkladığınızda, arama gösterilen bir Application Insights perspektif bakın. Perspektif seçilen telemetri türüne göre genişletilmiş bir görselleştirme sağlar. Bu nedenle, farklı telemetri türleri için görselleştirme içeriğini değiştirir.

Uygulamalar dikey penceresinde herhangi bir yere tıklayın, varsayılan gördüğünüz **uygulamaları** perspektif.

![Application Insights uygulamaları perspektifi](./media/app-insights-connector/applications-blade-drill-search.png)

Seçtiğiniz uygulamanın genel bir bakış açısından gösterir.

**Kullanılabilirlik** dikey penceresi, web test sonuçları ve başarısız olan ilgili istekleri görebileceğiniz farklı perspektif görünüm gösterir.

![Application Insights kullanılabilirlik perspektifi](./media/app-insights-connector/availability-blade-drill-search.png)

Tıkladığınızda herhangi bir yere **sunucu istekleri** veya **hataları** dikey pencereler, perspektif bileşenleri değiştirmek istekleriyle ilgili bir görselleştirme sağlar.

![Application Insights hataları dikey penceresi](./media/app-insights-connector/server-requests-failures-drill-search.png)

Tıkladığınızda herhangi bir yere **özel durumları** dikey penceresinde, özel durumlar için uyarlanmış bir görselleştirme görürsünüz.

![Application Insights özel durumları dikey penceresi](./media/app-insights-connector/exceptions-blade-drill-search.png)

Olup, bir şey tıklatın bakılmaksızın **Application Insights Bağlayıcısı** Pano dahilinde **arama** kendisi, Application Insights verilerini gösteren uygulama döndüren herhangi bir sorgu sayfası Insights perspektif. Application Insights verilerini görüntülüyorsunuz. Örneğin, bir **&#42;** sorgu ayrıca aşağıdaki görüntü gibi perspektif sekme gösterir:

![Application Insights](./media/app-insights-connector/app-insights-search.png)

Perspektif bileşenleri arama sorgusu bağlı olarak güncelleştirilir. Başka bir deyişle, verileri görme olanağı sunan herhangi bir arama alanını kullanarak sonuçları filtreleyebilirsiniz:

- Tüm uygulamalarınız
- Tek bir seçili uygulama
- Uygulama grubu

### <a name="pivot-to-an-app-in-the-azure-portal"></a>Azure Portalı'nda bir uygulama için Özet

Application Insights Bağlayıcısı dikey pencereleri seçili Application Insights uygulamayı Özet sağlamak için tasarlanmıştır *Azure portalı kullandığınızda*. Çözüm, sorunu giderme yardımcı olan üst düzey bir izleme platformu olarak kullanabilirsiniz. Olası bir soruna bağlı uygulamalarınızı hiçbirinde gördüğünüzde, ya da ayrıntıya, Log Analytics arama yapabilir veya doğrudan Application Insights uygulama Özet.

Özet için üç noktaya tıklayın (**...** ), her satırın sonunda görünür ve seçin **Application ınsights'ta Aç**.

>[!NOTE]
>**Application ınsights'ta Aç** Azure portalında kullanılabilir değil.

![Application Insights'ta aç](./media/app-insights-connector/open-in-app-insights.png)

### <a name="sample-corrected-data"></a>Veri örnek düzeltildi

Application ınsights *[düzeltme örnekleme](../../azure-monitor/app/sampling.md)* telemetri trafiğini azaltmak için. Application Insights uygulamanızı örnekleme etkinleştirdiğinizde, azaltılmış bir Application ınsights ve Log Analytics içinde depolanan girdi sayısı alın. Veri tutarlılığı de korunur ancak **Application Insights Bağlayıcısı** sayfası ve bakış açıları, örneklenen verileri özel sorgularınızı el ile düzeltmeniz gerekir.

Örnekleme düzeltme günlük arama sorgusu içindeki bir örnek aşağıda verilmiştir:

```
ApplicationInsights | summarize AggregatedValue = sum(SampledCount) by TelemetryType
```

**Örnek sayısı** alan tüm girişler mevcut olduğundan ve giriş temsil eden veri noktalarının sayısını gösterir. Application Insights uygulamanız için örnekleme kapatırsanız **örnek sayısı** 1'den büyüktür. Uygulamanızın oluşturduğu girişleri gerçek sayısını saymak için TOPLA **örnek sayısı** alanları.

Örnekleme, uygulamanızı oluşturan girişleri yalnızca toplam sayısını etkiler. Ölçüm alanları için örnekleme düzeltmek gerekmez **RequestDuration** veya **AvailabilityDuration** ortalama gösterilen girişler için bu alanları göstermek için.

## <a name="input-data"></a>Giriş verileri

Çözüme bağlı Application Insights uygulamalarınızdan aşağıdaki veri telemetri türlerini alır:

- Kullanılabilirlik
- Özel durumlar
- İstekler
- Sayfa görünümleri – sayfa görüntülemesi almak bir çalışma alanınız için bu bilgileri toplamak için uygulamalarınızı yapılandırmanız gerekir. Daha fazla bilgi için bkz. [PageViews](../../azure-monitor/app/api-custom-events-metrics.md#page-views).
- Özel olaylar – özel olaylarını almak bir çalışma alanınız için bu bilgileri toplamak için uygulamalarınızı yapılandırmanız gerekir. Daha fazla bilgi için bkz. [TrackEvent](../../azure-monitor/app/api-custom-events-metrics.md#trackevent).

Uygun olduğunda verilerini Application ınsights'tan Log Analytics tarafından alınır.

## <a name="output-data"></a>Çıktı verileri

Bir kayıt bir *türü* , *Applicationınsights* her giriş veri türü için oluşturulur. Applicationınsights kayıtları, aşağıdaki bölümlerde gösterilen özelliklere sahiptir:

### <a name="generic-fields"></a>Genel alanlar

| Özellik | Açıklama |
| --- | --- |
| Tür | ApplicationInsights |
| Clientıp |   |
| TimeGenerated | Kaydın zamanı |
| ApplicationId | Application Insights uygulama izleme anahtarı |
| ApplicationName | Application ınsights'ı adını uygulama |
| Roleınstance | Sunucu ana bilgisayar kimliği |
| deviceType | İstemci cihazı |
| ScreenResolution |   |
| Kıta | İsteğin geldiği kıta |
| Ülke | İsteğin geldiği ülke |
| İl | Bölge, eyalet veya yerel ayar isteği geldiği |
| Şehir | Şehir veya isteğin geldiği Şehir |
| isSynthetic | İstek bir kullanıcı veya otomatikleştirilmiş bir yöntem tarafından oluşturulup oluşturulmadığını belirtir. Oluşturulan kullanıcı = true veya false = otomatik yöntemi |
| SamplingRate | Portala gönderilen SDK'sı tarafından oluşturulan telemetri yüzdesi. 0,0-100.0 aralığı. |
| SampledCount | 100/(SamplingRate). Örneğin, 4 =&gt; % 25 |
| Isauthenticated durumunda olmasını gerektirir | True veya false |
| Operationıd | Aynı işlem kimliği ilişkili öğeleri portalda gösterilen olan öğeler. Genellikle istek kimliği |
| ParentOperationID | Üst işlem kimliği |
| OperationName |   |
| oturum kimliği | İsteğin oluşturulduğu oturum benzersiz olarak tanımlayan GUID |
| SourceSystem | ApplicationInsights |

### <a name="availability-specific-fields"></a>Kullanılabilirlik özgü alanlar

| Özellik | Açıklama |
| --- | --- |
| TelemetryType | Kullanılabilirlik |
| AvailabilityTestName | Web testinin adı |
| AvailabilityRunLocation | Http isteğinin coğrafi kaynak |
| AvailabilityResult | Web testi başarı sonucunu gösterir |
| AvailabilityMessage | Web testine bağlı ileti |
| AvailabilityCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| DataSizeMetricValue | 1.0 ya da 0.0 |
| DataSizeMetricCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| AvailabilityDuration | Web test süresinin milisaniye cinsinden süre |
| AvailabilityDurationCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| AvailabilityValue |   |
| AvailabilityMetricCount |   |
| AvailabilityTestId | Web testi için benzersiz GUID |
| AvailabilityTimestamp | Kullanılabilirlik testi, tam zaman damgası |
| AvailabilityDurationMin | Örneklenen kayıtlar için bu alan gösterilen veri noktaları için en düşük web test süresi (milisaniye cinsinden) gösterir. |
| AvailabilityDurationMax | Örneklenen kayıtlar için bu alan gösterilen veri noktaları için en fazla web test süresi (milisaniye cinsinden) gösterir. |
| AvailabilityDurationStdDev | Örneklenen kayıtlar için bu alan gösterilen veri noktaları için tüm web test süre (milisaniye) arasında standart sapma gösterir. |
| AvailabilityMin |   |
| AvailabilityMax |   |
| AvailabilityStdDev | &nbsp;  |

### <a name="exception-specific-fields"></a>Özel durum özgü alanlar

| Tür | ApplicationInsights |
| --- | --- |
| TelemetryType | Özel durum |
| ExceptionType | Özel durumun türü |
| ExceptionMethod | Özel durum oluşturan yöntemi |
| ExceptionAssembly | Ortak anahtar belirteci yanı sıra framework ve sürümünün derleme içerir |
| ExceptionGroup | Özel durumun türü |
| ExceptionHandledAt | Özel durumun işlenip düzeyini gösterir |
| ExceptionCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| ExceptionMessage | Özel durum iletisi |
| ExceptionStack | Özel durumun tam bir yığın |
| ExceptionHasStack | Özel durum yığın varsa TRUE |



### <a name="request-specific-fields"></a>İstek özgü alanlar

| Özellik | Açıklama |
| --- | --- |
| Tür | ApplicationInsights |
| TelemetryType | İstek |
| Yanıt kodu | İstemciye gönderilen HTTP yanıtı |
| RequestSuccess | Başarı veya başarısızlık gösterir. TRUE veya false. |
| RequestId | İstek benzersiz olarak tanımlanabilmesi için kimliği |
| RequestName | GET/POST + URL'si temeli |
| RequestDuration | İstek süresini saniye cinsinden süre |
| URL'si | Konak içermeden isteğin URL'si |
| Host | Web sunucusu ana bilgisayarı |
| URLBase | İstek tam URL'si |
| ApplicationProtocol | Uygulama tarafından kullanılan protokol türü |
| RequestCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| RequestDurationCount | 100 /(Sampling Rate). Örneğin, 4 =&gt; % 25 |
| RequestDurationMin | Örneklenen kayıt için bu alan için gösterilen veri noktalarının en az bir istek süresi (milisaniye cinsinden) gösterir. |
| RequestDurationMax | Örneklenen kayıtları için bu alan en fazla istek süresi (milisaniye) için gösterilen veri noktalarını gösterir. |
| RequestDurationStdDev | Örneklenen kayıtlar için tüm istek süre (milisaniye) arasında standart sapma Bu alan, gösterilen veri noktalarını gösterir. |

## <a name="sample-log-searches"></a>Örnek günlük aramaları

Bu çözüm panosunda gösterilen örnek günlük aramaları kümesi yok. Örnek günlük arama sorguları açıklamalarıyla birlikte ancak gösterilir [görünüm Application Insights Bağlayıcısı bilgi](#view-application-insights-connector-information) bölümü.

## <a name="removing-the-connector-with-powershell"></a>PowerShell ile bir bağlayıcı kaldırılıyor
OMS portalı kullanımdan kaldırma ile yapılandırmak ve mevcut bağlantıları Portalı'ndan kaldırmak için hiçbir yolu yoktur. Var olan bağlantıları aşağıdaki PowerShell Betiği ile kaldırabilirsiniz. Sahip veya bu işlemi gerçekleştirmek için çalışma alanının katkıda bulunan ve okuyucu Application Insights kaynağına ait olmalıdır.

```powershell
$Subscription_app = "App Subscription Name"
$ResourceGroup_app = "App ResourceGroup"
$Application = "Application Name"
$Subscription_workspace = "Workspace Subscription Name"
$ResourceGroup_workspace = "Workspace ResourceGroup"
$Workspace = "Workspace Name"

Connect-AzAccount
Set-AzContext -SubscriptionId $Subscription_app
$AIApp = Get-AzApplicationInsights -ResourceGroupName $ResourceGroup_app -Name $Application 
Set-AzContext -SubscriptionId $Subscription_workspace
Remove-AzOperationalInsightsDataSource -WorkspaceName $Workspace -ResourceGroupName $ResourceGroup_workspace -Name $AIApp.Id
```

REST API çağrısı çağıran aşağıdaki PowerShell betiğini kullanarak uygulamaların bir listesini alabilirsiniz. 

```powershell
Connect-AzAccount
$Tenant = "TenantId"
$Subscription_workspace = "Workspace Subscription Name"
$ResourceGroup_workspace = "Workspace ResourceGroup"
$Workspace = "Workspace Name"
$AccessToken = "AAD Authentication Token" 

Set-AzContext -SubscriptionId $Subscription_workspace
$LAWorkspace = Get-AzOperationalInsightsWorkspace -ResourceGroupName $ResourceGroup_workspace -Name $Workspace

$Headers = @{
    "Authorization" = "Bearer $($AccessToken)"
    "x-ms-client-tenant-id" = $Tenant
}

$Connections = Invoke-RestMethod -Method "GET" -Uri "https://management.azure.com$($LAWorkspace.ResourceId)/dataSources/?%24filter=kind%20eq%20'ApplicationInsights'&api-version=2015-11-01-preview" -Headers $Headers
$ConnectionsJson = $Connections | ConvertTo-Json
```
Bu betik, Azure Active Directory kimlik doğrulaması için bir taşıyıcı kimlik doğrulaması belirteci gerektirir. Bu belirteci almak için bir yol kullanarak bir makaledeki [REST API belgeleri site](https://docs.microsoft.com/rest/api/loganalytics/datasources/createorupdate). Tıklayın **deneyin** ve Azure aboneliğinizde oturum açın. Taşıyıcı belirtecinden kopyalayabilirsiniz **istek Önizleme** aşağıdaki görüntüde gösterildiği gibi.


![Taşıyıcı belirteç](media/app-insights-connector/bearer-token.png)


Ayrıca, günlük sorgusu uygulamaların kullanım listesini alabilirsiniz:

```Kusto
ApplicationInsights | summarize by ApplicationName
```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım [günlük araması](../../azure-monitor/log-query/log-query-overview.md) Application Insights uygulamalarınız için ayrıntılı bilgileri görüntülemek için.
