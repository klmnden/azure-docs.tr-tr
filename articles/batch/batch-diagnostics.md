---
title: Ölçümleri, uyarılar ve Azure Batch için tanılama günlüklerini | Microsoft Docs
description: Kayıt ve havuzları ve görevler gibi Azure Batch hesabı kaynaklarına için tanılama günlük olayları analiz edin.
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 04/05/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: e64d272695c4e47c972df040d1c1c2a63bf3dddd
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="batch-metrics-alerts-and-logs-for-diagnostic-evaluation-and-monitoring"></a>Toplu işlem ölçümleri, uyarılar ve tanılama değerlendirme ve izleme günlükleri

Bu makalede özelliklerini kullanarak Batch hesabı izlemek açıklanmaktadır [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Azure İzleyici toplar [ölçümleri](../monitoring-and-diagnostics/monitoring-overview-metrics.md) ve [tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) Batch hesabınızdaki kaynaklara için. Toplayın ve bu verileri bir çeşitli şekillerde Batch hesabınıza izlemek ve sorunlarını tanılamak için kullanmak. Ayrıca yapılandırabilirsiniz [ölçüm uyarıları](../monitoring-and-diagnostics/monitoring-overview-alerts.md#alerts-on-azure-monitor-data) ölçüm bir belirtilen değere ulaştığında bildirimleri almak için. 

## <a name="batch-metrics"></a>Toplu işlem ölçümleri

(Performans sayaçlarını olarak da bilinir) Azure telemetri verilerini Azure izleme hizmeti tarafından tüketilen, Azure kaynaklarınızı tarafından gösterilen ölçümleridir. Bir toplu işlem hesabı örnek ölçümlerini içerir: havuzu oluşturma olayları, düşük öncelikli düğüm sayısı ve görev Complete olayları. 

Bkz: [desteklenen toplu ölçüm listesine](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftbatchbatchaccounts).

Ölçümler şunlardır:

* Varsayılan olarak her bir Batch hesabınıza ek yapılandırma olmadan etkin
* 1 dakikada oluşturulan
* Otomatik olarak kalıcı değildir, ancak 30 günlük çalışırken geçmişi bulunur. Etkinlik ölçümlerinin parçası olarak kalıcı [tanılama günlük](#work-with-diagnostic-logs).

### <a name="view-metrics"></a>Metrikleri görüntüleyin

Batch hesabınıza Azure portalında metrikleri görüntüleyin. **Genel bakış** sayfası anahtar düğümü, çekirdek ve görev ölçümleri hesap varsayılan olarak gösterir. 

Tüm Batch hesabı ölçümlerini görüntülemek için: 

1. Portalı'nda tıklatın **tüm hizmetleri** > **Batch hesapları**ve ardından, Batch hesabınızın adını tıklatın.
2. Altında **izleme**, tıklatın **ölçümleri**.
3. Bir veya daha fazla ölçümleri seçin. İsterseniz, ek kaynak ölçümleri kullanarak seçme **abonelikleri**, **kaynak grubu**, **kaynak türü**, ve **kaynak** bırakmalar.

    ![Toplu işlem ölçümleri](media/batch-diagnostics/metrics-portal.png)

Ölçümleri programlı olarak almak için Azure İzleyici API'lerini kullanın. Örneğin, [.NET ile Azure İzleyici almak ölçümleri](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/).

## <a name="batch-metric-alerts"></a>Toplu ölçüm uyarıları

İsteğe bağlı olarak, yakın gerçek zamanlı yapılandırın *ölçüm uyarıları* atadığınız bir eşik değeri, belirtilen bir ölçüm kestiği zaman tetikler. Bir uyarı oluşturur bir [bildirim](../monitoring-and-diagnostics/insights-alerts-portal.md) uyarı "(eşiği aşıldığında ve uyarı koşulu karşılanır olduğunda) etkinleştirildiğinde" yanı sıra, seçtiğiniz "Çözülene" (zaman eşiği yeniden çapraz ve koşul yok artık karşılanmış). 

Örneğin, düşük öncelikli işler için çekirdek sayısı belirli bir düzeyine düştüğünde havuzlarınızı oluşumunu ayarlayabilmeniz için ölçüm bir uyarı yapılandırmak isteyebilirsiniz.

Portalda ölçüm bir uyarı yapılandırmak için:

1. **Tüm hizmetler** > **Batch hesapları** seçeneğine ve sonra da Batch hesabınızın adına tıklayın.
2. Altında **izleme**, tıklatın **uyarı kuralları** > **ölçüm uyarı Ekle**.
3. Ölçüm, bir uyarı durumu (örneğin, bir ölçüm belirli bir değeri bir dönem boyunca aştığında) ve bir veya daha fazla bildirim seçin.

Bir yakın gerçek zamanlı uyarı kullanarak da yapılandırabilirsiniz [REST API](). Daha fazla bilgi için bkz: [Azure portalında Azure Hizmetleri için yeni ölçüm uyarıları kullanın](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md)
## <a name="batch-diagnostics"></a>Toplu iş tanılama

Tanılama günlüklerini işlemi her bir kaynağın açıklayan Azure kaynaklar tarafından gösterilen bilgiler içerir. Toplu işlem için aşağıdaki günlüklerini toplayabilir:

* **Hizmet Günlükleri** bir havuz veya görev tek bir toplu kaynağın ömrü boyunca Azure Batch hizmeti tarafından oluşturulan olaylar gibi. 

* **Ölçümleri** hesap düzeyinde günlükleri. 

Tanılama günlükleri toplamayı etkinleştirmek için ayarlar varsayılan olarak etkin değildir. Açıkça izlemek istediğiniz her toplu işlem hesabı için tanılama ayarlarını etkinleştirin.

### <a name="log-destinations"></a>Günlük hedefleri

Bir Azure depolama hesabı günlük hedefi seçin ortak bir senaryodur. Azure depolama alanında günlükleri depolamak için günlükleri koleksiyonunu etkinleştirmeden önce hesabı oluşturun. Batch hesabınızla bir depolama hesabı ilişkilendirirseniz, bu hesaba günlük hedefi seçebilirsiniz. 

Tanılama günlükleri için isteğe bağlı diğer hedefleri:

* Toplu iş tanılama günlüğü olaylarını akışı bir [Azure olay hub'ı](../event-hubs/event-hubs-what-is-event-hubs.md). Olay hub'ları sonra dönüştürmek ve tüm gerçek zamanlı analiz sağlayıcısı kullanarak depolamak saniye başına milyonlarca olayı işleyebilen. 

* Tanılama günlüklerini gönderme [Azure günlük analizi](../log-analytics/log-analytics-overview.md), buradan Operations Management Suite (OMS) Portalı'nda çözümlemek, veya Power BI veya Excel'den analiz için bunları dışarı aktarma.

> [!NOTE]
> Depolama veya Azure hizmetleriyle tanılama günlük verilerini işlemek için ek ücrete neden olabilir. 
>

### <a name="enable-collection-of-batch-diagnostic-logs"></a>Toplu iş tanılama günlükleri toplamayı etkinleştir

1. Portalı'nda tıklatın **tüm hizmetleri** > **Batch hesapları**ve ardından, Batch hesabınızın adını tıklatın.
2. Altında **izleme**, tıklatın **tanılama günlükleri** > **tanılamayı açın**.
3. İçinde **tanılama ayarlarını**ayarı için bir ad girin ve bir günlük hedefi (var olan depolama hesabı, olay hub'ı veya günlük analizi) seçin. Ya da her ikisini de seçin **ServiceLog** ve **AllMetrics**.

    Bir depolama hesabı seçtiğinizde, isteğe bağlı olarak bir bekletme ilkesi ayarlayın. Veri saklama için gün sayısı belirtmezseniz, depolama hesabı'nın ömrü sırasında tutulur.

4. **Kaydet**’e tıklayın.

    ![Toplu iş tanılama](media/batch-diagnostics/diagnostics-portal.png)

Günlük toplama etkinleştirmek için diğer seçenekler şunlardır: portalda Azure İzleyicisi'ni kullanın tanılama ayarlarını yapılandırmak için kullanmak bir [Resource Manager şablonu](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md), veya Azure PowerShell veya Azure CLI kullanın. bkz: [toplamak ve Azure kaynaklarınızdan günlük verilerini tüketen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs).


### <a name="access-diagnostics-logs-in-storage"></a>Depolama erişim tanılama günlükleri

Bir depolama hesabı toplu tanılama günlüklerine arşivlerseniz ilgili olay oluştuktan hemen sonra bir depolama kapsayıcısı depolama hesabında oluşturulur. BLOB'ları, aşağıdaki adlandırma deseni göre oluşturulur:

```
insights-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/
RESOURCEGROUPS/{resource group name}/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/{batch account name}/y={four-digit numeric year}/
m={two-digit numeric month}/d={two-digit numeric day}/
h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
Örnek:

```
insights-metrics-pt1m/resourceId=/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/
RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/MYBATCHACCOUNT/y=2018/m=03/d=05/h=22/m=00/PT1H.json
```
JSON biçimli blob URL'SİNDE belirtilen saat içinde oluşan olaylarla her PT1H.json blob dosyası içerir (örneğin, h = 12). Mevcut saat boyunca, olaylar meydana geldikçe PT1H.json dosyasına eklenir. Dakika değeri (m 00 =) her zaman tanılama günlük olayları tek tek bloblar saat başına ayrılmış bu yana 00. (Tüm saatler UTC biçimindedir.)


Tanılama günlüklerini depolama hesabındaki şeması hakkında daha fazla bilgi için bkz: [arşiv Azure tanılama günlükleri](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

Depolama hesabınız günlüklerine program aracılığıyla erişmek için depolama API'leri kullanın. 

### <a name="service-log-events"></a>Hizmeti oturum açma olayları
Azure Batch hizmeti toplanan, günlükleri havuzu ya da görev gibi tek bir toplu iş kaynak kullanım ömrü süresince Azure Batch hizmeti tarafından oluşturulan olayları içerir. Batch tarafından gösterilen her olay JSON biçiminde günlüğe kaydedilir. Örneğin, bu bir örnek gövdesidir **havuzu oluşturma olayı**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "5",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Batch hizmeti şu anda aşağıdaki hizmeti oturum açma olayları gösterir. Bu makalede son güncelleştirmesinden bu yana ek olaylar eklenmemiş olabilir beri bu liste geniş kapsamlı, olmayabilir.

| **Hizmeti oturum açma olayları** |
| --- |
| [Havuz oluşturma](batch-pool-create-event.md) |
| [Havuzu silme Başlat](batch-pool-delete-start-event.md) |
| [Havuzu silme tamamlandı](batch-pool-delete-complete-event.md) |
| [Havuzu yeniden boyutlandırma Başlat](batch-pool-resize-start-event.md) |
| [Tam havuzu yeniden boyutlandırma](batch-pool-resize-complete-event.md) |
| [Görev Başlat](batch-task-start-event.md) |
| [Görev tamamlandı](batch-task-complete-event.md) |
| [Görev başarısız](batch-task-fail-event.md) |



## <a name="next-steps"></a>Sonraki adımlar

* Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
* Daha fazla bilgi edinmek [toplu çözümlerini izleme](monitoring-overview.md).
