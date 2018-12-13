---
title: Kullanımı ve istatistikleri - Azure Search arama hizmeti için izleme
description: Microsoft Azure üzerinde barındırılan bulut arama hizmeti olan Azure arama için kaynak tüketimi ve dizin boyutu izleyin.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 11/09/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 584d1d8ce3285f9f5fb986c9779d3c403ce13d1b
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53314168"
---
# <a name="monitor-an-azure-search-service-in-azure-portal"></a>Azure portalındaki Azure Search Hizmeti izleme

Azure arama, kullanım ve arama hizmetleri performansını izleme için çeşitli kaynaklar sunar. Ölçümler, günlükler, dizin istatistikleri ve Power bı'da genişletilmiş izleme özellikleri erişmenizi sağlar. Bu makalede, farklı izleme stratejileri etkinleştirme ve sonuçta elde edilen verileri nasıl yorumlayacağınız açıklanmaktadır.

## <a name="azure-search-metrics"></a>Azure arama ölçümleri
Ölçümler, neredeyse gerçek zamanlı görünürlük, arama hizmetinize vermek ve ek kurulum ile her hizmet için kullanılabilir. Bunlar en çok 30 gün için hizmetinizin performansını izlemenize olanak tanır.

Azure Search'ü üç farklı ölçüm için veri toplar:

* Arama gecikme süresi: Arama Hizmeti arama sorguları, dakika başına toplanan işlemek için gereken süre.
* Arama sorguları / saniye (QPS): Arama sayısı, saniyede alınan sorguları dakika başına toplanır.
* Kısıtlanmış arama sorguları yüzdesi: Dakika başına toplanan kısıtlanan arama sorguları yüzdesi.

![Ekran görüntüsü, QPS etkinliği][1]

### <a name="set-up-alerts"></a>Uyarılar ayarlayın
Ölçüm ayrıntıları sayfasından, bir ölçüm tanımladığınız eşiği aştığında da bir e-posta bildirimi veya otomatik bir eylemi tetiklemesine uyarıları yapılandırabilirsiniz.

Ölçümler hakkında daha fazla bilgi için Azure İzleyici üzerinde tam belgelere bakın.  

## <a name="how-to-track-resource-usage"></a>Kaynak kullanımını izleme
Dizin ve belge boyut büyümesi izleme proaktif olarak kapasite üst sınırı hizmetiniz için belirlediğiniz ulaşmaktan önce ayarlamanıza yardımcı olabilir. Portalı veya REST API'sini kullanarak program aracılığıyla bunu yapabilirsiniz.

### <a name="using-the-portal"></a>Portalı kullanma

Kaynak kullanımını izlemek için hizmetinizde için istatistikleri ve sayıları görüntülemek [portalı](https://portal.azure.com).

1. [Portalda](https://portal.azure.com) oturum açın.
2. Azure Search hizmetinizin hizmet panosunu açın. Hizmet için kutucuklar giriş sayfasında bulunabilir veya atlama Gözat gelen hizmete göz atabilirsiniz.

Kullanım bölümüne kullanılabilir kaynakları hangi kısmını şu anda kullanımda söyleyen bir ölçüm içerir. Dizin, belgeler ve depolama için hizmet başına sınırları hakkında daha fazla bilgi için bkz: [hizmet sınırları](search-limits-quotas-capacity.md).

  ![Kullanım kutucuğu][2]

> [!NOTE]
> Yalnızca konak 3 dizin, 10.000 belge veya veri 50 MB en fazla bir çoğaltma sahiptir ve her bölüm ücretsiz hizmet için yukarıdaki ekran görüntüsü olduğundan, hangisinin önce geldiğine bağlı. Bir temel veya standart katmanda oluşturulan Hizmetleri çok büyük hizmet sınırlara sahiptir. Bir katmanı seçme hakkında daha fazla bilgi için bkz. [katmanı veya SKU seçme](search-sku-tier.md).
>
>

### <a name="using-the-rest-api"></a>REST API’sini kullanma
Azure Search REST API ve .NET SDK'sı hem hizmet ölçümlerini programlı erişim sağlar.  Kullanıyorsanız [dizin oluşturucular](https://msdn.microsoft.com/library/azure/dn946891.aspx) Azure SQL veritabanı veya Azure Cosmos DB dizin yüklemek için ek bir API gerektiren sayıları almak kullanılabilir.

* [Dizin istatistiklerini alma](/rest/api/searchservice/get-index-statistics)
* [Belge sayısı](/rest/api/searchservice/count-documents)
* [Dizin Oluşturucu durumunu Al](/rest/api/searchservice/get-indexer-status)

## <a name="how-to-export-logs-and-metrics"></a>Günlükleri ve ölçümleri dışarı aktarma

Hizmetiniz ve ham veri önceki bölümde açıklanan ölçümler için işlem günlükleri dışarı aktarabilirsiniz. İşlemi, verileri bir depolama hesabına kopyalanır hizmetinin nasıl kullanıldığını ve tüketilebilir Power BI'dan haberdar let günlüğe kaydeder. Azure arama, bu amaç için izleme ve Power BI içerik paketi sağlar.


### <a name="enabling-monitoring"></a>İzlemeyi etkinleştirme
Azure Search Hizmeti [Azure portalında](http://portal.azure.com) izlemeyi etkinleştir seçeneğinin altında.

Dışarı aktarmak istediğiniz verileri seçin: Günlükler, Ölçümler ve her ikisi de. Bir depolama hesabına kopyalayın, bir olay hub'ına gönderme veya Log Analytics'e giriş.

![Portalda izleme olanağı tanıma][3]

PowerShell veya Azure CLI kullanarak etkinleştirmek için belgelere bakın [burada](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

### <a name="logs-and-metrics-schemas"></a>Günlükleri ve ölçümleri şemaları
Verileri bir depolama hesabına kopyalanır, verileri JSON ya da onun yerinde iki kapsayıcı olarak biçimlendirilir:

* ınsights günlükleri operationlogs: arama trafiği günlükleri
* ınsights ölçümleri pt1m: ölçümler için

Kapsayıcı başına saatlik bir blob yok.

Örnek yol: `resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2015/m=12/d=25/h=01/m=00/name=PT1H.json`

#### <a name="log-schema"></a>Günlüğü şeması
Günlükleri BLOB'ları, arama hizmeti trafik günlüklerinizin içerir.
Her blob olarak adlandırılan bir kök nesnesinin sahip **kayıtları** günlük nesnelerinin bir dizisi içeren.
Her blob kayıtlar üzerinde aynı saat boyunca gerçekleşen tüm işlem vardır.

| Ad | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| time |datetime |"2015-12-07T00:00:43.6872559Z" |İşlemin zaman damgası |
| resourceId |dize |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>VARSAYILAN/RESOURCEGROUPS/SAĞLAYICILARI /<br/> MICROSOFT. ARAMA/SEARCHSERVICES/SEARCHSERVICE" |ResourceId |
| operationName |dize |"Query.Search" |İşlem adı |
| operationVersion |dize |"2015-02-28" |Kullanılan api-version |
| category |dize |"OperationLogs" |Sabit |
| resultType |dize |"Başarılı" |Olası değerler: Başarı veya başarısızlık |
| resultSignature |int |200 |HTTP Sonuç kodu |
| süre (MS) |int |50 |Milisaniye cinsinden işlem süresi |
| properties |object |aşağıdaki tabloya bakın |İşlem özgü verileri içeren nesne. |

**Özellik şeması**

| Ad | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| Açıklama |dize |"/İndexes('content')/docs Al" |İşlemin bitiş noktası |
| Sorgu |dize |"? arama = Azure Search & $count = true & API-version = 2015-02-28" |Sorgu parametreleri |
| Belgeler |int |42 |İşlenen belge sayısı |
| indexName |dize |"testindex" |İşlemle ilişkili dizinin adı |

#### <a name="metrics-schema"></a>Ölçüm şeması

| Ad | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| resourceId |dize |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>VARSAYILAN/RESOURCEGROUPS/SAĞLAYICILARI /<br/>MICROSOFT. ARAMA/SEARCHSERVICES/SEARCHSERVICE" |Kaynak Kimliği |
| MetricName |dize |"Gecikme süresi" |Ölçüm adı |
| time |datetime |"2015-12-07T00:00:43.6872559Z" |işlemin zaman damgası |
| ortalama |int |64 |Ham örnekleri ölçüm zaman aralığındaki ortalama değeri |
| en az |int |37 |En düşük değer ölçüm zaman aralığında ham örnekleri |
| en fazla |int |78 |Ölçüm zaman aralığında ham örnekleri en yüksek değeri |
| toplam |int |258 |Toplam değer ölçüm zaman aralığında ham örnekleri |
| count |int |4 |Ölçü oluşturmak için kullanılan ham örnek sayısı |
| timegrain |dize |"PT1M" |ISO 8601 ölçümü, zaman dilimi |

Tüm ölçümler, bir dakikalık aralıklar olarak raporlanır. Her ölçüm, dakika başına en düşük, en yüksek ve ortalama değerleri gösterir.

SearchQueriesPerSecond ölçüm için en az bu dakikasındaki kaydedildiği saniye başına arama sorguları için en düşük değeridir. Aynı en yüksek değeri için geçerlidir. Toplama tamamı ortalama, dakikadır.
Bir dakika boyunca bu senaryoyu düşünün: yüksek bir saniye yüklemek diğer bir deyişle en fazla SearchQueriesPerSecond 58 saniyelik ortalama yük tarafından izlenen için ve son olarak yalnızca bir sorgu ile bir saniye olan en düşük gereksinimdir.

ThrottledSearchQueriesPercentage için minimum, maksimum, ortalama ve toplam, tümü aynı değere sahip:, arama sorguları bir dakika boyunca toplam sayısı azaltıldı arama sorguları yüzdesi.

## <a name="analyzing-your-data-with-power-bi"></a>Power BI ile verilerinizi analiz etme

Kullanmanızı öneririz [Power BI](https://powerbi.microsoft.com) verilerinizi görselleştirin ve keşfedin. Bu kolayca Azure depolama hesabınıza bağlayın ve hızlı bir şekilde verilerinizi analiz etmeye başlayabilirsiniz.

Azure arama sağlayan bir [Power BI içerik paketi](https://app.powerbi.com/getdata/services/azure-search) izlemek ve önceden tanımlanmış grafikleri ve tabloları ile arama trafiğiniz anlamanıza olanak tanır. Bu, otomatik olarak verilerinize bağlanmak ve arama hizmetiniz ilgili görsel Öngörüler sağlayın Power BI rapor kümesini içerir. Daha fazla bilgi için [içerik paketi yardım sayfasına](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/).

![Azure Search için Power BI Panosu][4]

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [ölçeklendirme çoğaltmalar ve bölümler](search-limits-quotas-capacity.md) bölümleri ve çoğaltmalarını mevcut bir hizmet ayrılması dengelemeye yönelik yönergeler için.

Ziyaret [Microsoft azure'da arama hizmetinizi yönetin](search-manage.md) hizmet yönetimi hakkında daha fazla bilgi veya [performans ve iyileştirme](search-performance-optimization.md) ayarlama kılavuzu için.

Muhteşem raporlar oluşturma hakkında daha fazla bilgi edinin. Bkz: [Power BI Desktop ile çalışmaya başlama](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) için Ayrıntılar

<!--Image references-->
[1]: ./media/search-monitor-usage/AzSearch-Monitor-BarChart.PNG
[2]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG
[3]: ./media/search-monitor-usage/AzureSearch-Enable-Monitoring.PNG
[4]: ./media/search-monitor-usage/AzureSearch-PowerBI-Dashboard.png
