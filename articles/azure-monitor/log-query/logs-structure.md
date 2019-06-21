---
title: Azure İzleyici günlüklerine yapısını | Microsoft Docs
description: Azure İzleyici'den günlük verilerini almak için günlük sorgusu gerektirir.  Bu makalede, Azure İzleyici'de kullanılan sorguları nasıl yeni bir günlük açıklar ve bir oluşturmadan önce anlamanız gereken kavramlar sağlar.
services: log-analytics
author: bwren
ms.service: log-analytics
ms.topic: conceptual
ms.date: 06/16/2019
ms.author: bwren
ms.openlocfilehash: e243ebbc31f9e941678ac76a83738276995b5ba1
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67297168"
---
# <a name="structure-of-azure-monitor-logs"></a>Azure İzleyici günlüklerine yapısı
İlişkin verileri kullanarak öngörüleri hızlıca elde olanağı bir [günlük sorgusu](log-query-overview.md) Azure İzleyici, güçlü bir özelliğidir. Verimli ve kullanışlı sorgular oluşturmak için istediğiniz verilerin bulunduğu ve nasıl yapılandırıldığını gibi bazı temel kavramları anlamanız gerekir. Bu makale, kullanmaya başlamak gereken temel kavramları sağlar.

## <a name="overview"></a>Genel Bakış
Azure İzleyici günlüklerine veriler, Log Analytics çalışma alanı veya bir Application Insights uygulama depolanır. Her ikisi tarafından desteklenen [Azure Veri Gezgini](/azure/data-explorer/) , güçlü veri altyapısı ve sorgu dili yararlanarak anlamına gelir.

Çalışma alanları ve uygulamalarda hem veri tablolarına, her biri farklı türlerde veri depolayan ve kendi benzersiz özellik kümesine sahiptir, düzenlenir. Çoğu [veri kaynakları](../platform/data-sources.md) sırasında Application Insights, Application Insights uygulamanın tablolarında önceden tanımlanmış bir dizi yazacak bir Log Analytics çalışma alanında, kendi tablolarında yazar. Günlük sorguları bile kaynaklar arası sorgu birden çok çalışma alanlarında tablolardaki verileri birleştiren ya da çalışma ve uygulama verilerini birleştiren sorgular yazmak için kullanın ve verileri kolayca birden fazla tablodan birleştirerek olanak tanıyan çok esnektir.

Aşağıdaki örnek sorgularda kullanılan olan farklı tabloların yazma veri kaynağı örnekleri gösterir.

![Tablolar](media/logs-structure/queries-tables.png)

## <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı
Azure İzleyici günlüklerine dışında Application Insights tarafından toplanan tüm verileri depolanan bir [Log Analytics çalışma alanı](../platform/manage-access.md). Belirli gereksinimlerinize bağlı olarak, bir veya daha fazla çalışma alanı oluşturabilirsiniz. [Veri kaynakları](../platform/data-sources.md) Azure kaynaklarından etkinlik günlükleri ve tanılama günlükleri gibi bunların onboarding işleminin bir parçası olarak yapılandırdığınız bir veya daha fazla çalışma alanları için veri sanal makineler ve veri öngörüleri ve izleme çözümlerinden aracıları yazılacaktır. Diğer hizmetler gibi [Azure Güvenlik Merkezi](/azure/security-center/) ve [Azure Gözcü](/azure/sentinel/) da izleme verilerini diğer birlikte günlük sorguları kullanılarak çözümlenebilecek kalmaması için verilerini saklamak için bir Log Analytics çalışma alanı kullanın kaynakları.

Farklı türlerde veri çalışma alanında farklı tablolarda depolanır ve her tablonun benzersiz bir özellikler kümesi vardır. Standart bir tablo, bir çalışma alanına eklenir, oluşturulur ve yeni tablolar eklenir farklı veri kaynakları, çözümleri ve Hizmetleri için oldukları gibi eklenmedi. Özel tablolardaki kullanarak da oluşturabilirsiniz [veri toplayıcı API'sini](../platform/data-collector-api.md).

Bir çalışma alanı ve bunların şemasında tablolarında göz atabilirsiniz **şema** Log Analytics çalışma alanı için sekmesinde.

![Çalışma alanı şema](media/scope/workspace-schema.png)

Önceki gün içinde her çalışma alanı ve kayıt sayısını tablolarında toplanan listesi için aşağıdaki sorguyu kullanın. 

```Kusto
union withsource = table * 
| where TimeGenerated > ago(1d)
| summarize count() by table
| sort by table asc
```
Her bir veri kaynağı oluşturdukları tablolar hakkında ayrıntılar için belgelere bakın. Örnekler için makale [aracı veri kaynakları](../platform/agent-data-sources.md), [tanılama günlükleri](../platform/diagnostic-logs-schema.md), ve [izleme çözümleri](../insights/solutions-inventory.md).

### <a name="workspace-permissions"></a>Çalışma alanı izinleri
Bkz: [çalışma alanı izinlerini ve kapsam](../platform/manage-access.md#workspace-permissions-and-scope) çalışma alanındaki verilere erişim sağlamaya ilişkin ayrıntılar için. Çalışma alanına erişim izni verme yanı sıra kullanarak tek tek tablolar için erişimi sınırlayabilirsiniz [tablo düzeyi RBAC](../platform/manage-access.md#table-level-rbac).

## <a name="application-insights-application"></a>Application Insights uygulaması
Uygulama anlayışları'nda bir uygulama oluşturduğunuzda, karşılık gelen bir uygulama, Azure İzleyici günlüklerine otomatik olarak oluşturulur. Yapılandırma verilerini toplamak için gereklidir ve uygulama otomatik olarak yazacak izleme verilerini sayfa görüntülemeleri, istekler ve özel durumlar gibi.

Log Analytics çalışma alanında farklı olarak, bir Application Insights uygulama tabloları sabit bir dizi sahiptir. Diğer veri kaynakları, hiçbir ek tablolar oluşturulabilmesi için uygulama yazmak için yapılandıramazsınız. 

| Tablo | Açıklama | 
|:---|:---|
| availabilityResults | Kullanılabilirlik testleri özet verileri. |
| browserTimings      | Gelen veri işlemek için geçen süre gibi istemci performansıyla ilgili verileri. |
| customEvents        | Uygulamanız tarafından oluşturulan özel olaylar. |
| customMetrics       | Uygulamanız tarafından oluşturulan özel ölçümleri. |
| Bağımlılıkları        | Dış bileşenler uygulamasından çağırır. |
| Özel durumlar          | Uygulama çalışma zamanı tarafından oluşturulan özel durumlar. |
| Sayfa görüntülemesi           | Her Web sitesi hakkında veri tarayıcı bilgileri görüntüleyin. |
| PerformanceCounters | Uygulama destekleyen bilgi işlem kaynaklarından performans ölçümleridir. |
| istekler            | Her uygulama isteği ayrıntıları.  |
| izlemeleri              | Dağıtılmış izleme sonuçları. |

Her tablo için şema görüntüleyebileceğiniz **şema** Log Analytics uygulama için sekmesinde.

![Uygulama şeması](media/scope/application-schema.png)

## <a name="standard-properties"></a>Standart özellikler
Azure İzleyici günlüklerine her tablo kendi şemasını olsa da tüm tabloları tarafından paylaşılan standart özellikler vardır. Bkz: [standart Azure İzleyici günlüklerine özelliklerinde](../platform/log-standard-properties.md) her hakkında ayrıntılar için.

| Log Analytics çalışma alanı | Application Insights uygulaması | Açıklama |
|:---|:---|:---|
| TimeGenerated | timestamp  | Tarih ve kaydın oluşturulduğu saat. |
| Tür          | Itemtype   | Kayıt alınan tablosunun adı. |
| _ResourceId   |            | Kaynak kaydı için benzersiz tanımlayıcı ile ilişkilidir. |
| _IsBillable   |            | Alınan veri Faturalanabilir olup olmadığını belirtir. |
| _BilledSize   |            | Faturalandırılır veri bayt cinsinden boyutunu belirtir. |

## <a name="next-steps"></a>Sonraki adımlar
- Kullanma hakkında bilgi edinin [oluşturmak ve düzenlemek için Log Analytics günlük aramaları](../log-query/portals.md).
- Kullanıma bir [sorgu yazmakla ilgili öğretici](../log-query/get-started-queries.md) yeni sorgu diline kullanarak.
