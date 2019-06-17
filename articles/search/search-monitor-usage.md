---
title: Bir arama hizmeti - Azure Search için kaynak kullanımı ve sorgu ölçümlerini izleme
description: Günlüğe kaydetmeyi etkinleştirmek, bir Azure Search hizmet sorgu etkinlik ölçümlerinin, kaynak kullanımı ve diğer sistem verileri alın.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: bac897178c8220abe72a92a5cf14fc4767cdd3bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66755065"
---
# <a name="monitor-resource-consumption-and-query-activity-in-azure-search"></a>Azure Search'te kaynak tüketimi ve sorgu etkinliğini İzle

Azure Search hizmetinizin genel bakış sayfasında, kaynak kullanımı, sorgu ölçümleri ve ne kadar kota daha çok dizin, dizin oluşturucular ve veri kaynakları oluşturmak kullanılabilir sistem verileri görüntüleyebilirsiniz. Portal, log analytics veya kalıcı veri toplama için kullanılan başka bir kaynak yapılandırmak için de kullanabilirsiniz. 

Kendi kendine tanılama ve koruma için işletimsel geçmişi günlüklerini ayarlama yararlıdır. Dahili olarak, bir destek bileti, araştırma ve analiz için yeterli zaman kısa bir süre için arka uçta günlükleri mevcut. Üzerinde denetim ve erişim bilgileri günlüğe kaydetmek istiyorsanız, bu makalede açıklanan çözümlerden birini ayarlamanız gerekir.

Bu makalede, Seçenekler, günlük kaydı etkinleştirme ve depolama oturum ve günlük içeriklerini görüntüleme izleme hakkında bilgi edinin.

## <a name="metrics-at-a-glance"></a>Ölçümleri bir bakışta

**Kullanım** ve **izleme** genel bakış'ta yerleşik olarak bölümlerde kaynak tüketimi üzerinde rapor sayfasında ve sorgu yürütme ölçümleri. Herhangi bir yapılandırma gerekmez hizmeti kullanmaya başladıktan hemen sonra bu bilgileri kullanılabilir. Bu sayfa birkaç dakikada bir yenilenir. Hakkında kararlar sonlandırılıyor varsa [hangi katmanı, üretim iş yükleri için kullanılacak](search-sku-tier.md), veya kullanılıp kullanılmayacağını [etkin çoğaltmalar ve bölümler sayısını](search-capacity-planning.md), bu ölçümlere göre bu kararları ile yardımcı olabilir size, kaynakları nasıl hızlı bir şekilde tüketilir ve geçerli yapılandırmasını mevcut yükü ne kadar iyi işler gösterir.

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

| Resource | Kullanıldığı yerler |
|----------|----------|
| [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) | Günlüğe kaydedilen olayları ve aşağıdaki şemalarını temel alan, sorgu ölçümleri uygulamanızda kullanıcı olayları ile ilişkili. Bu hesaba, uygulama kodu tarafından gönderilen istekleri filtrelemenize aksine, kullanıcı tarafından başlatılan arama eşleştirme olaylarını kullanıcı eylemleri veya sinyalleri alan tek bir çözümdür. Bu yaklaşımı kullanmak için kopyalama-izleme kodu, kaynak dosyaları için Application Insights için rota bilgi içine yapıştırın. Daha fazla bilgi için [arama trafiği analizi](search-traffic-analytics.md). |
| [Azure İzleyici günlükleri](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) | Günlüğe kaydedilen olayları ve şemalarına göre sorgu ölçümleri. Olaylar, Log Analytics çalışma alanına kaydedilir. Günlük kaydından ayrıntılı bilgi almak için bir çalışma alanı karşı sorgular çalıştırabilirsiniz. Daha fazla bilgi için [Azure İzleyici günlüklerine ile çalışmaya başlama](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata) |
| [Blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) | Günlüğe kaydedilen olayları ve şemalarına göre sorgu ölçümleri. Olayları günlüğe bir Blob kapsayıcısına ve JSON dosyalarında depolanan. Dosya içeriğini görüntülemek için JSON düzenleyicisini kullanın.|
| [Olay Hub’ı](https://docs.microsoft.com/azure/event-hubs/) | Günlüğe kaydedilen olayları ve bu makalede anlatıldığı şemaları dayalı sorgu ölçümleri. Bu, çok büyük günlükleri için bir alternatif veri toplama hizmeti olarak seçin. |

Ürününü, yaşam süresi Azure aboneliğinize ücretsiz olarak deneyebilirsiniz böylece hem Azure İzleyici günlüklerine hem de Blob Depolama ücretsiz paylaşılan hizmet olarak kullanılabilir. Application Insights, uygulama veri boyutu altında belirli sınırları olduğu sürece kaydolun ve ücretsiz (bkz [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/monitor/) Ayrıntılar için).

Sonraki bölümde, etkinleştirme ve toplamak ve Azure arama işlemleri tarafından oluşturulan günlük verilerine erişmek için Azure Blob Depolama kullanma adımlarında size kılavuzluk eder.

## <a name="enable-logging"></a>Günlü kaydını etkinleştir

Dizin oluşturma ve sorgu iş yükleri için günlük varsayılan olarak kapalıdır ve günlüğe kaydetme altyapı hem de dış uzun vadeli depolama için eklenti çözümleri bağlıdır. Günlükleri başka bir yerde depolanması için tek başına kalıcı veriler yalnızca Azure Search'te oluşturur ve yönetir, nesneleridir.

Bu bölümde, günlüğe kaydedilen olayları ve ölçümleri verileri depolamak için Blob Depolama kullanan öğreneceksiniz.

1. [Depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) zaten yoksa. Bu alıştırmada kullanılan tüm kaynakları silmek isterseniz temizleme daha sonra basitleştirmek için Azure Search ile aynı kaynak grubundaki yerleştirebilirsiniz.

   Depolama hesabınız, Azure Search ile aynı bölgede bulunmalıdır.

2. Arama hizmeti genel bakış sayfası açın. Sol gezinti bölmesinde aşağı kaydırarak **izleme** tıklatıp **izlemeyi etkinleştirme**.

   ![İzlemeyi Etkinleştir](./media/search-monitor-usage/enable-monitoring.png "izlemeyi etkinleştir")

3. Dışarı aktarmak istediğiniz verileri seçin: Günlükler, Ölçümler ve her ikisi de. Bir depolama hesabına kopyalayın, bir olay hub'ına gönderme veya Azure İzleyici günlüklerine verin.

   İçin arşiv Blob Depolama, yalnızca depolama hesabının mevcut olması gerekir. Günlük verileri dışarı aktardığınızda kapsayıcılar ve bloblar gerektiğinde oluşturulur.

   ![Yapılandırma blob'u depolama arşiv](./media/search-monitor-usage/configure-blob-storage-archive.png "yapılandırma blob depolama Arşivi")

4. Profili kaydedin.

5. Test nesneleri silmesini veya yaratmasını günlüğe kaydetme (günlüğü olaylarını oluşturur) ve sorguları gönderme (ölçümleri oluşturur). 

Profili kaydedin, sonra günlük kaydı etkindir. Kapsayıcılar, yalnızca bir etkinlik günlüğü veya ölçü olduğunda oluşturulur. Verileri bir depolama hesabına kopyalanır, verileri JSON olarak biçimlendirilmiş ve iki kapsayıcılarda yer:

* ınsights günlükleri operationlogs: arama trafiği günlükleri
* ınsights ölçümleri pt1m: ölçümler için

**Kapsayıcılar, Blob Depolama alanında görünmesi için önce bir saat sürer. Kapsayıcı başına saatlik bir blob yok.**

Kullanabileceğiniz [Visual Studio Code](#download-and-open-in-visual-studio-code) veya dosyaları görüntülemek için başka bir JSON Düzenleyicisi. 

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
| operationVersion |string |"2019-05-06" |Kullanılan api-version |
| category |dize |"OperationLogs" |Sabit |
| resultType |dize |"Başarılı" |Olası değerler: Başarı veya başarısızlık |
| resultSignature |int |200 |HTTP Sonuç kodu |
| süre (MS) |int |50 |Milisaniye cinsinden işlem süresi |
| properties |object |aşağıdaki tabloya bakın |İşlem özgü verileri içeren nesne. |

**Özellik şeması**

| Ad | Tür | Örnek | Notlar |
| --- | --- | --- | --- |
| Açıklama |dize |"/İndexes('content')/docs Al" |İşlemin bitiş noktası |
| Sorgu |string |"?search=AzureSearch&$count=true&api-version=2019-05-06" |Sorgu parametreleri |
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

## <a name="use-system-apis"></a>Sistem API'leri kullanın
Azure Search REST API ve .NET SDK'sı hem hizmet ölçümleri, dizin ve dizin oluşturucu bilgileri programlı erişim sağlar ve belge sayılarını.

* [Hizmet istatistikleri alma](/rest/api/searchservice/get-service-statistics)
* [Dizin istatistiklerini alma](/rest/api/searchservice/get-index-statistics)
* [Belge sayısı](/rest/api/searchservice/count-documents)
* [Dizin Oluşturucu durumunu Al](/rest/api/searchservice/get-indexer-status)

PowerShell veya Azure CLI kullanarak etkinleştirmek için belgelere bakın [burada](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview).

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft azure'da arama hizmetinizi yönetin](search-manage.md) hizmet yönetimi hakkında daha fazla bilgi ve [performans ve iyileştirme](search-performance-optimization.md) ayarlama kılavuzu için.