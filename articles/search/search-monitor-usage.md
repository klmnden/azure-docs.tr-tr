---
title: Bir arama hizmeti - Azure Search için kaynak kullanımı ve sorgu istatistikleri izleme
description: Sorgu etkinliği ölçümler, kaynak tüketimi ve diğer sistem verileri bir Azure Search hizmet alın.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: af2a9cd7f834f5c6f70a78d94e8826de2584127d
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55076392"
---
# <a name="monitor-an-azure-search-service-in-azure-portal"></a>Azure portalındaki Azure Search Hizmeti izleme

Azure Search hizmetinizin genel bakış sayfasında, kaynak kullanımının yanı sıra, sorgu başına ikinci (QPS), sorgunun gecikme süresi ve kısıtlanmış isteklerin yüzdesi gibi sorgu ölçümleri hakkında sistem verilerini görüntüleyebilirsiniz. Ayrıca, Azure platformunda daha ayrıntılı veri toplama özellikleri izleme yararlanmanıza için portalı kullanabilirsiniz. 

Bu makalede, tanımlar ve Azure arama işlemleri günlüğe kaydetme için kullanılabilir seçenekler karşılaştırılmaktadır. Bu, günlüğe kaydetme ve günlük depolama ve hizmet ve kullanıcı etkinliğini bilgileri nasıl etkinleştirmek için yönergeler içerir.

Günlüklerini ayarlama, kendi kendine tanılama ve hizmet işlemlerine geçmişini koruma için kullanışlıdır. Dahili olarak, günlükleri varsa araştırma ve analiz için yeterli zaman kısa bir süre için bir destek bileti çıkartın. Hizmetinize günlük bilgilerini depolama denetlemek istiyorsanız, bu makalede açıklanan çözümlerden birini ayarlamanız gerekir.

## <a name="metrics-at-a-glance"></a>Ölçümleri bir bakışta

**Kullanım** ve **izleme** genel bakış içinde yerleşik olarak bölümlerde depolama tüketimi Görselleştirme ve sorgu yürütme ölçümleri. Herhangi bir yapılandırma gerekmez hizmeti kullanmaya başladıktan hemen sonra bu bilgileri kullanılabilir. Bu sayfa birkaç dakikada bir yenilenir. Hakkında kararlar sonlandırılıyor varsa [hangi katmanı, üretim iş yükleri için kullanılacak](search-sku-tier.md), veya kullanılıp kullanılmayacağını [etkin çoğaltmalar ve bölümler sayısını](search-capacity-planning.md), bu ölçümlere göre bu kararları ile yardımcı olabilir size, kaynakları nasıl hızlı bir şekilde tüketilir ve geçerli yapılandırmasını mevcut yükü ne kadar iyi işler gösterir.

**Kullanım** sekmesi geçerli göre kaynak kullanılabilirliğini gösterir [sınırları](search-limits-quotas-capacity.md). Aşağıdaki çizimde, her türden 3 nesneleri ve 50 MB depolama büyük harflerle yazılmış ücretsiz hizmet içindir. Temel veya standart bir hizmet daha yüksek sınırlara sahiptir ve bölüm sayısını artırmak için en fazla depolama orantılı olarak artar.

![Kullanım durumu etkin sınırları göre](./media/search-monitor-usage/usage-tab.png
 "etkili sınırları göre kullanım durumu")

## <a name="queries-per-second-qps-and-other-metrics"></a>Sorgular / saniye (QPS) ve diğer ölçümleri

**İzleme** gösterir hareketli ortalamalar arama gibi ölçümler için sekmesinde *saniye başına sorgu* (QPS), dakika başına toplanır. 
*Arama gecikme süresi* arama hizmeti arama sorguları, dakika başına toplanan işlemek için gereken süreyi belirtir. *Kısıtlanmış arama sorguları yüzdesi* (gösterilmemiştir), ayrıca dakika başına toplanan kısıtlanan arama sorguları yüzdesidir.

Bu sayı, yaklaşık olarak düzenlenir ve ne kadar iyi sisteminizi isteklere hizmet veriyor genel bir fikir vermek için tasarlanmıştır. Gerçek QPS Portalı'nda rapor sayısı daha yüksek veya düşük olabilir.

![İkinci Etkinlik başına sorguları](./media/search-monitor-usage/monitoring-tab.png "sorguları ikinci Etkinlik başına")

## <a name="activity-logs"></a>Etkinlik günlükleri

**Etkinlik günlüğü** Azure Resource Manager'dan bilgi toplar. Etkinlik günlüğü'nde bulunan bilgilere örnek olarak, oluşturma veya silme isteği işlemek için ad kullanılabilirliğini denetleme veya hizmeti erişim anahtarını alma bir kaynak grubu, güncelleştirme, bir hizmet içerir. 

Erişebildiğiniz **etkinlik günlüğü** sol gezinti bölmesinden veya üst pencere komut çubuğunda veya gelen bildirimler **Tanıla ve problemleri çözmenize** sayfası.

Dizin oluşturma ya da bir veri kaynağını silme gibi hizmet içi görevleri için her istek, ancak değil belirli bir eylemi kendisi için "Yönetici anahtarını Al" gibi genel bildirim görürsünüz. Bu bilgi düzeyi için bir eklenti izleme çözümü etkinleştirmeniz gerekir.

## <a name="add-on-monitoring-solutions"></a>Eklenti izleme çözümleri

Azure arama, günlük veri harici olarak depolanması gereken diğer bir deyişle herhangi bir veri yönettiği, nesneler dışında depolamaz. Günlük verileri kalıcı hale getirmek istiyorsanız, kaynaklardan herhangi birini yapılandırabilirsiniz. 

Aşağıdaki tabloda, günlükleri depolamak ve geniş kapsamlı hizmet işlemlerini ve sorgu iş yükleri Application Insights ile izleme ekleme karşılaştırılmıştır.

| Kaynak | İçin kullanılan |
|----------|----------|
| [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) | Günlüğe kaydedilen olayları ve sorgu ölçümleri temel bir Aşağıda, şemaların uygulamanızda kullanıcı olayları ile ilişkili. Bu hesaba, uygulama kodu tarafından gönderilen istekleri filtrelemenize aksine, kullanıcı tarafından başlatılan arama eşleştirme olaylarını kullanıcı eylemleri veya sinyalleri alan tek bir çözümdür. Bu yaklaşımı kullanmak için kopyalama-izleme kodu, kaynak dosyaları için Application Insights için rota bilgi içine yapıştırın. Daha fazla bilgi için [arama trafiği analizi](search-traffic-analytics.md). |
| [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) | Günlüğe kaydedilen olayları sorgu ölçümleri temel bir aşağıdaki şemalar. Olayları, Log analytics'teki çalışma alanına kaydedilir. Günlük kaydından ayrıntılı bilgi almak için bir çalışma alanı karşı sorgular çalıştırabilirsiniz. Daha fazla bilgi için [Log Analytics ile çalışmaya başlama](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata) |
| [Blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) | Günlüğe kaydedilen olayları sorgu ölçümleri temel bir aşağıdaki şemalar. Olayları günlüğe bir Blob kapsayıcısına ve JSON dosyalarında depolanan. Dosya içeriğini görüntülemek için JSON düzenleyicisini kullanın.|
| [Olay Hub’ı](https://docs.microsoft.com/azure/event-hubs/) | Günlüğe kaydedilen olayları ve bu makalede anlatıldığı şemaları dayalı sorgu ölçümleri. Bu, çok büyük günlükleri için bir alternatif veri toplama hizmeti olarak seçin. |



Ürününü, yaşam süresi Azure aboneliğinize ücretsiz olarak deneyebilirsiniz böylece hem Log Analytics hem de Blob Depolama Paylaşılan ücretsiz hizmeti olarak kullanılabilir. Sonraki bölümde, etkinleştirme ve toplamak ve Azure arama işlemleri tarafından oluşturulan günlük verilerine erişmek için Azure Blob Depolama kullanma adımlarında size kılavuzluk eder.

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Dizin oluşturma ve sorgu iş yükleri için günlük varsayılan olarak kapalıdır ve günlüğe kaydetme altyapı hem de dış uzun vadeli depolama için eklenti çözümleri bağlıdır. Günlükleri başka bir yerde depolanması kendisi tarafından kalıcı veriler yalnızca Azure Search'te dizinler olduğundan.

Bu bölümde, günlüğe kaydedilen olayları ve ölçümleri verileri depolamak için Blob Depolama kullanan öğreneceksiniz.

1. [Depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) zaten yoksa. Bu alıştırmada kullanılan tüm kaynakları silmek isterseniz temizleme daha sonra basitleştirmek için Azure Search ile aynı kaynak grubundaki yerleştirebilirsiniz.

2. Arama hizmeti genel bakış sayfası açın. Sol gezinti bölmesinde aşağı kaydırarak **izleme** tıklatıp **izlemeyi etkinleştirme**.

   ![İzlemeyi Etkinleştir](./media/search-monitor-usage/enable-monitoring.png "izlemeyi etkinleştir")

3. Dışarı aktarmak istediğiniz verileri seçin: Günlükler, Ölçümler ve her ikisi de. Bir depolama hesabına kopyalayın, bir olay hub'ına gönderme veya Log Analytics'e giriş.

   İçin arşiv Blob Depolama, yalnızca depolama hesabının mevcut olması gerekir. Günlük verileri dışarı aktardığınızda, kapsayıcılar ve bloblar oluşturulur.

   ![Yapılandırma blob'u depolama arşiv](./media/search-monitor-usage/configure-blob-storage-archive.png "yapılandırma blob depolama Arşivi")

4. Profili kaydedin.

5. Test nesneleri silmesini veya yaratmasını günlüğe kaydetme (günlüğü olaylarını oluşturur) ve sorguları gönderme (ölçümleri oluşturur). 

Profili kaydedin, sonra günlüğe kaydetme etkinleştirildiğinde, kapsayıcıları yalnızca bir olay günlüğü veya ölçü olduğunda oluşturulur. Bu kapsayıcılar görünmesi birkaç dakika sürebilir. Verileri bir depolama hesabına kopyalanır, verileri JSON olarak biçimlendirilmiş ve iki kapsayıcılarda yer:

* ınsights günlükleri operationlogs: arama trafiği günlükleri
* ınsights ölçümleri pt1m: ölçümler için

Kullanabileceğiniz [Visual Studio Code](#Download-and-open-in-Visual-Studio-Code) veya dosyaları görüntülemek için başka bir JSON Düzenleyicisi. Kapsayıcı başına saatlik bir blob yok.

### <a name="example-path"></a>Örnek yol

```
resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2018/m=12/d=25/h=01/m=00/name=PT1H.json
```

## <a name="log-schema"></a>Günlüğü şeması
Arama hizmeti trafik günlüklerinizin içeren BLOB'ları, bu bölümde açıklanan şekilde yapılandırılmıştır. Her blob olarak adlandırılan bir kök nesnesinin sahip **kayıtları** günlük nesnelerinin bir dizisi içeren. Her blob aynı saat boyunca gerçekleşen tüm işlemler için kayıtlar içerir.

| Ad | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| time |datetime |"2018-12-07T00:00:43.6872559Z" |İşlemin zaman damgası |
| resourceId |dize |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>VARSAYILAN/RESOURCEGROUPS/SAĞLAYICILARI /<br/> MICROSOFT. ARAMA/SEARCHSERVICES/SEARCHSERVICE" |ResourceId |
| operationName |dize |"Query.Search" |İşlem adı |
| operationVersion |dize |"2017-11-11" |Kullanılan api-version |
| category |dize |"OperationLogs" |Sabit |
| resultType |dize |"Başarılı" |Olası değerler: Başarı veya başarısızlık |
| resultSignature |int |200 |HTTP Sonuç kodu |
| süre (MS) |int |50 |Milisaniye cinsinden işlem süresi |
| properties |object |aşağıdaki tabloya bakın |İşlem özgü verileri içeren nesne. |

**Özellik şeması**

| Ad | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| Açıklama |dize |"/İndexes('content')/docs Al" |İşlemin bitiş noktası |
| Sorgu |dize |"?search=AzureSearch&$count=true&api-version=2017-11-11" |Sorgu parametreleri |
| Belgeler |int |42 |İşlenen belge sayısı |
| indexName |dize |"testindex" |İşlemle ilişkili dizinin adı |

## <a name="metrics-schema"></a>Ölçüm şeması

Sorgu istekleri için ölçümleri yakalanır.

| Ad | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| resourceId |dize |"/ SUBSCRIPTIONS/11111111-1111-1111-1111-111111111111 /<br/>VARSAYILAN/RESOURCEGROUPS/SAĞLAYICILARI /<br/>MICROSOFT. ARAMA/SEARCHSERVICES/SEARCHSERVICE" |Kaynak Kimliği |
| MetricName |dize |"Gecikme süresi" |Ölçüm adı |
| time |datetime |"2018-12-07T00:00:43.6872559Z" |işlemin zaman damgası |
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

## <a name="download-and-open-in-visual-studio-code"></a>İndirme ve Visual Studio Code'da açın

Günlük dosyasını görüntülemek için herhangi bir JSON Düzenleyicisi'ni kullanabilirsiniz. Öneririz yoksa varsa, [Visual Studio Code](https://code.visualstudio.com/download).

1. Azure portalında, depolama hesabınızı açın. 

2. Sol gezinti bölmesinden **Blobları**. Görmelisiniz **ınsights günlükleri operationlogs** ve **ınsights ölçümleri pt1m**. Günlük verileri Blob depolama alanına aktarılır, bu kapsayıcılar, Azure Search tarafından oluşturulur.

3. Dosyanın .json ulaşana kadar klasör hiyerarşisi'e tıklayın.  Dosyayı indirmek için bağlam menüsünü kullanın.

Dosyayı indirdikten sonra içeriklerini görüntülemek için bir JSON düzenleyicisinde açın.

## <a name="get-sys-info-apis"></a>Sistem bilgisi API'ları Al
Azure Search REST API ve .NET SDK'sı hem hizmet ölçümleri, dizin ve dizin oluşturucu bilgileri programlı erişim sağlar ve belge sayılarını.

* [Hizmet istatistikleri alma](/rest/api/searchservice/get-service-statistics)
* [Dizin istatistiklerini alma](/rest/api/searchservice/get-index-statistics)
* [Belge sayısı](/rest/api/searchservice/count-documents)
* [Dizin Oluşturucu durumunu Al](/rest/api/searchservice/get-indexer-status)

PowerShell veya Azure CLI kullanarak etkinleştirmek için belgelere bakın [burada](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft azure'da arama hizmetinizi yönetin](search-manage.md) hizmet yönetimi hakkında daha fazla bilgi ve [performans ve iyileştirme](search-performance-optimization.md) ayarlama kılavuzu için.