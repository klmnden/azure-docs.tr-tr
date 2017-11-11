---
title: "Kullanım ve Azure Search Hizmeti istatistiklerine izleme | Microsoft Docs"
description: "Azure arama, Microsoft Azure üzerinde barındırılan bulut arama hizmeti için kaynak kullanım ve dizin boyutu izler."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 
ms.service: search
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 11/09/2017
ms.author: heidist
ms.openlocfilehash: fe852afedfc1cce99d81b8ab53c6c80df34ac6d6
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="monitoring-an-azure-search-service"></a>Azure Search Hizmeti izleme

Azure arama, kullanım ve arama hizmetleri performansını izleme için çeşitli kaynaklar sunar. Bu ölçümleri, günlükleri, dizin istatistikleri ve Power BI üzerinde genişletilmiş izleme yeteneklerini erişmenizi sağlar. Bu makalede, farklı izleme stratejileri etkinleştirme ve sonuç verileri yorumlama açıklanmaktadır.

## <a name="azure-search-metrics"></a>Azure arama ölçütleri
Ölçümleri arama hizmetinizi gerçek zamanlı görünürlük yakın verin ve hiçbir ek ayar ile her hizmet için kullanılabilir. Bunlar 30 güne kadar hizmetinizin performansını izlemenize olanak tanır.

Azure arama için üç farklı ölçüm verilerini toplar:

* Arama gecikme süresi: dakika başına toplanan arama sorguları işlemek için gereken arama hizmeti saat.
* Arama sorguları (QPS) saniyede: arama sayısını saniye başına alınan sorguları dakikada bir araya getirilir.
* Daraltılmış arama sorguları yüzdesi: kısıtlanan arama sorguları yüzdesi dakikada bir araya getirilir.

![Ekran görüntüsü, QPS etkinliği][1]

### <a name="set-up-alerts"></a>Uyarıları ayarlayın
Ölçüm ayrıntı sayfasından bir ölçüm tanımladığınız bir eşik kestiği zaman bir e-posta bildirimi veya otomatik bir eylemi tetikleyen üzere uyarılar yapılandırabilirsiniz.

Ölçümler hakkında daha fazla bilgi için Azure monitörde tam belgelerine bakın.  

## <a name="how-to-track-resource-usage"></a>Kaynak kullanımını izlemek nasıl
Dizinler ve belge boyutu büyümesini izleme proaktif olarak hizmetiniz için belirlediğinize göre üst sınırı basarsa önce kapasite ayarlamanıza yardımcı olabilir. Portalı veya programlı olarak REST API kullanarak bunu yapabilirsiniz.

### <a name="using-the-portal"></a>Portalı kullanma

Kaynak kullanımını izlemek için sayıları ve hizmetinizi istatistiklerini görüntülemek [portal](https://portal.azure.com).

1. Oturum [portal](https://portal.azure.com).
2. Azure Search hizmetinizin hizmet panosunu açın. Hizmet için kutucukları giriş sayfasında bulunabilir veya Gözat harf Çubuğu'ndan hizmete göz atabilirsiniz.

Hangi kullanılabilir kaynakları bölümündedir şu anda kullanımda söyleyen bir ölçer kullanım bölüm içerir. Dizinleri, belgeler ve depolama için başına hizmet sınırları hakkında daha fazla bilgi için bkz: [hizmet sınırları](search-limits-quotas-capacity.md).

  ![Kullanımı kutucuğu][2]

> [!NOTE]
> Yukarıdaki ekran görüntüsü, bir çoğaltma maksimum vardır ve her bölüm ve yalnızca konak 3 dizinleri, 10.000 belgeler veya veri 50 MB boş hizmeti için hangisi önce gelirse. Bir temel veya standart katmanını oluşturulan hizmetlerine çok daha büyük hizmet sınırları vardır. Bir katmanı seçme özelliği hakkında daha fazla bilgi için bkz: [bir katmanı SKU seçin veya](search-sku-tier.md).
>
>

### <a name="using-the-rest-api"></a>REST API kullanarak
Azure Search REST API'sini ve .NET SDK hizmeti ölçümleri programlı erişim sağlar.  Kullanıyorsanız [dizin oluşturucular](https://msdn.microsoft.com/library/azure/dn946891.aspx) Azure SQL veritabanı ya da Azure Cosmos DB dizin yüklemek için ek bir API ihtiyaç duyduğunuz numaraları almak kullanılabilir.

* [Dizin istatistikleri Al](/rest/api/searchservice/get-index-statistics)
* [Count belgeleri](/rest/api/searchservice/count-documents)
* [Dizin Oluşturucu durumunu Al](/rest/api/searchservice/get-indexer-status)

## <a name="how-to-export-logs-and-metrics"></a>Günlükleri ve ölçümleri dışarı aktarma

İşlem günlükleri hizmetiniz ve önceki bölümde açıklanan ölçümleri ham verileri dışarı aktarabilirsiniz. İşlemi, verileri bir depolama hesabına kopyalandığında hizmetinin nasıl kullanıldığını ve tüketilebilir Power BI'dan bildiğiniz let günlüğe kaydeder. Azure arama bu amaç için bir izleme Power BI içerik paketi sağlar.


### <a name="enabling-monitoring"></a>İzlemeyi etkinleştirme
Azure Search Hizmeti [Azure portal](http://portal.azure.com) izlemesini etkinleştir seçeneği altında.

Dışarı aktarmak istediğiniz verileri seçin: günlükleri, ölçümleri veya her ikisi de. Bir depolama hesabına kopyalamak, olay hub'ına göndermek veya günlük analizi verebilirsiniz.

![Portalda izlemeyi etkinleştirme][3]

PowerShell veya Azure CLI kullanarak etkinleştirmek için belgelere bakın [burada](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

### <a name="logs-and-metrics-schemas"></a>Günlükleri ve ölçümleri şemaları
Bir depolama hesabına veriler kopyalandığında, verileri JSON ve onun yerine iki kapsayıcı olarak biçimlendirilir:

* Öngörüler günlükleri operationlogs: arama trafiği günlükleri
* Öngörüler ölçümleri pt1m: ölçümler için

Kapsayıcı başına saat başına bir blob yok.

Örnek yolu:`resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2015/m=12/d=25/h=01/m=00/name=PT1H.json`

#### <a name="log-schema"></a>Günlüğü şeması
Günlükleri BLOB'ları arama hizmeti trafik günlüklerinizi içerir.
Her bir blob olarak adlandırılan bir kök nesnesi vardır **kayıtları** günlük nesnelerinin bir dizisi içerir.
Her bir blob kayıtları aynı saat boyunca gerçekleşen tüm işlemi üzerinde sahiptir.

| Ad | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| time |Tarih saat |"2015-12-07T00:00:43.6872559Z" |İşlemin zaman damgası |
| resourceId |Dize |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>VARSAYILAN/RESOURCEGROUPS/SAĞLAYICILARI /<br/> MICROSOFT. ARAMA/SEARCHSERVICES/SEARCHSERVICE" |ResourceId |
| operationName |Dize |"Query.Search" |İşlem adı |
| operationVersion |Dize |"2015-02-28" |Kullanılan API sürümü |
| category |Dize |"OperationLogs" |sabiti |
| resultType |Dize |"Başarılı" |Olası değerler: başarı veya başarısızlık |
| resultSignature |Int |200 |HTTP Sonuç kodu |
| durationMS |Int |50 |Milisaniye cinsinden işlem süresi |
| properties |Nesne |aşağıdaki tabloya bakın |İşlemi özgü verileri içeren nesnesi |

**Özellik şeması**
| Ad | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| Açıklama |Dize |"/İndexes('content')/docs Al" |İşlem uç noktası |
| Sorgu |Dize |"? arama = AzureSearch & $count = true & api-version = 2015-02-28" |Sorgu parametreleri |
| Belgeler |Int |42 |İşlenen belge sayısı |
| indexName |Dize |"testindex" |İşlemle ilişkili dizinin adı |

#### <a name="metrics-schema"></a>Ölçümleri şeması
| Ad | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| resourceId |Dize |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>VARSAYILAN/RESOURCEGROUPS/SAĞLAYICILARI /<br/>MICROSOFT. ARAMA/SEARCHSERVICES/SEARCHSERVICE" |Kaynak Kimliği |
| metricName |Dize |"Gecikme" |Ölçüm adı |
| time |Tarih saat |"2015-12-07T00:00:43.6872559Z" |işlem zaman damgası |
| Ortalama |Int |64 |Ortalama değer ölçüm zaman aralığında ham örnekleri |
| en az |Int |37 |En düşük değer ölçüm zaman aralığında ham örnekleri |
| Maksimum |Int |78 |En büyük değer ölçüm zaman aralığında ham örnekleri |
| Toplam |Int |258 |Toplam değer ölçüm zaman aralığında ham örnekleri |
| Sayısı |Int |4 |Ölçüm oluşturmak için kullanılan ham örnek sayısı |
| timegrain |Dize |"PT1M" |ISO 8601 ölçümün zaman birimi |

Tüm ölçümlerini bir dakikalık aralıklarla raporlanır. Her ölçüm dakikada en az, en fazla ve ortalama değerleri gösterir.

SearchQueriesPerSecond için en az bu dakikada kaydedildiği saniye başına arama sorguları için en düşük değer ölçümüdür. Aynı en büyük değeri için de geçerlidir. Toplama tüm üzerinde ortalama, dakikadır.
Bir dakika sırasında bu senaryoyu düşünün: yüksek bir saniye yüklenemedi diğer bir deyişle maksimum 58 saniyelik ortalama yük tarafından izlenen SearchQueriesPerSecond için ve son olarak yalnızca bir sorgu ile bir saniye olan en düşük gereksinimdir.

ThrottledSearchQueriesPercentage için minimum, maksimum, ortalama ve toplam, tümü aynı değere sahip:, arama sorguları bir dakikada toplam sayısı kısıtlanan arama sorguları yüzdesi.

## <a name="analyzing-your-data-with-power-bi"></a>Power BI ile verilerinizi analiz etme

Kullanmanızı öneririz [Power BI](https://powerbi.microsoft.com) keşfetmek ve verilerinizi görselleştirmek için. Kolayca Azure depolama hesabınıza bağlanın ve hızlı bir şekilde verilerinizi çözümlemeye başlayın.

Azure arama sağlayan bir [Power BI içerik paketi](https://app.powerbi.com/getdata/services/azure-search) izlemek ve önceden tanımlanmış grafikler ve tablolar ile arama trafiğinizi anlamanıza olanak sağlar. Otomatik olarak verilerinize bağlanın ve arama hizmetinizi hakkında visual bilgiler Power BI raporları kümesi içerir. Daha fazla bilgi için bkz: [içerik paketi Yardım sayfası](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-search/).

![Azure arama için Power BI Panosu][4]

## <a name="next-steps"></a>Sonraki adımlar
Gözden geçirme [ölçeklendirme çoğaltmaları ve bölümleri](search-limits-quotas-capacity.md) bölümler ve çoğaltmalar var olan bir hizmet için dengelemeye yönelik yönergeler için.

Ziyaret [Microsoft azure'da Search hizmetinizi yönetme](search-manage.md) Hizmeti Yönetimi hakkında daha fazla bilgi için veya [performans ve en iyi duruma getirme](search-performance-optimization.md) Kılavuzu ayarlama.

Harika raporları oluşturma hakkında daha fazla bilgi edinin. Bkz: [Power BI Desktop ile çalışmaya başlama](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) Ayrıntılar için

<!--Image references-->
[1]: ./media/search-monitor-usage/AzSearch-Monitor-BarChart.PNG
[2]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG
[3]: ./media/search-monitor-usage/AzureSearch-Enable-Monitoring.PNG
[4]: ./media/search-monitor-usage/AzureSearch-PowerBI-Dashboard.png
