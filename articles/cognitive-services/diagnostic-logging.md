---
title: Tanılama günlüğüne kaydetme
titleSuffix: Azure Cognitive Services
description: Bu kılavuzda, Azure Bilişsel hizmet için tanılama günlük kaydını etkinleştirmek için adım adım yönergeler sağlar. Bu günlükleri, sorunu tanımlama ve hata ayıklama için kullanılan zengin, sık sık veri kaynağının işlemiyle ilgili sağlar.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.topic: article
ms.date: 06/14/2019
ms.author: erhopf
ms.openlocfilehash: e1a6a44d7ff9d5786388fc47245ef5c79cb9be82
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155736"
---
# <a name="enable-diagnostic-logging-for-azure-cognitive-services"></a>Azure Bilişsel hizmetler için tanılama günlüğünü etkinleştirme

Bu kılavuzda, Azure Bilişsel hizmet için tanılama günlük kaydını etkinleştirmek için adım adım yönergeler sağlar. Bu günlükleri, sorunu tanımlama ve hata ayıklama için kullanılan zengin, sık sık veri kaynağının işlemiyle ilgili sağlar. Devam etmeden önce en az bir Bilişsel hizmet aboneliği bir Azure hesabıyla gibi olmalıdır [Bing Web araması](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/overview), [konuşma Hizmetleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/overview), veya [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis).

## <a name="prerequisites"></a>Önkoşullar

Tanılama günlük kaydını etkinleştirmek için günlük verileri depolamak için bir yere gerekir. Bu öğreticide, Azure depolama ve Log Analytics kullanılır.

* [Azure depolama](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) -ilke denetimi, statik analiz veya yedekleme için tanılama günlüklerini korur. Ayarı yapılandıran kullanıcının her iki abonelik için uygun RBAC erişimine sahip olduğu sürece, günlükleri yayan kaynak ile aynı abonelikte olması depolama hesabı yok.
* [Log Analytics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics) -bir Azure kaynağı tarafından oluşturulan işlenmemiş günlüklerin analiz için sağlayan bir esnek günlük arama ve analiz aracı.

> [!NOTE]
> Ek yapılandırma seçenekleri kullanılabilir. Daha fazla bilgi için bkz. [toplamak ve Azure kaynaklarınızdan günlük verilerini kullanma](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="enable-diagnostic-log-collection"></a>Tanılama günlük toplamayı etkinleştir  

Azure portalını kullanarak günlüğe kaydetme tanılama etkinleştirerek başlayalım.

> [!NOTE]
> PowerShell veya Azure CLI kullanarak bu özelliği etkinleştirmek için bölümlerinde sağlanan yönergeleri kullanın. [toplamak ve Azure kaynaklarınızdan günlük verilerini kullanma](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

1. Azure portalına gidin. Bulun ve Bilişsel hizmetler kaynağı seçin. Örneğin, Bing Web araması aboneliğinize.   
2. Ardından, sol taraftaki gezinti menüsünden bulun **izleme** seçip **tanılama ayarları**. Bu ekran, bu kaynak için daha önce oluşturulan tüm tanılama ayarları içerir.
3. Kullanmak istediğiniz önceden oluşturulmuş bir kaynak varsa, bunu şimdi seçebilirsiniz. Aksi takdirde seçin **+ tanılama ayarı ekleme**.
4. Ayar için bir ad girin. Ardından **bir depolama hesabında arşivle** ve **log Analytics göndermek**.
5. Yapılandırmak için istendiğinde, depolama hesabı ve tanılama günlüklerini depolamak için kullanmak istediğiniz bir OMS çalışma alanı seçin. **Not**: Bir depolama hesabı veya OMS çalışma alanı yoksa, oluşturmak için istemleri izleyin.
6. Seçin **denetim**, **RequestResponse**, ve **AllMetrics**. Ardından, tanılama günlük veri saklama süresini ayarlayın. Bir bekletme ilkesi, sıfır olarak ayarlanırsa, olay günlüğü kategori için süresiz olarak depolanır.
7. **Kaydet**’e tıklayın.

Uygulamanın iki günlük verilerini sorgulanması ve çözümlenmesi kullanılabilir olmadan önce saate kadar sürebilir. Herhangi bir şey hemen görmüyorsanız, bu nedenle endişelenmeyin.

## <a name="view-and-export-diagnostic-data-from-azure-storage"></a>Görüntüleyebilir ve Tanılama verileri Azure depolama biriminden dışarı aktarma

Azure depolama, büyük miktarda yapılandırılmamış veriyi depolamak için optimize edilmiştir sağlam nesne bir depolama çözümüdür. Bu bölümde, depolama hesabınız için toplam işlem bir 30 günlük süre içinde sorgulamak ve Excel verileri dışarı aktarma öğreneceksiniz.

1. Azure portalından son bölümde oluşturduğunuz Azure depolama kaynağı bulun.
2. Sol taraftaki gezinti menüsünden bulun **izleme** seçip **ölçümleri**.
3. Sorgunuzu yapılandırmak için kullanılabilen açılır listeleri kullanın. Bu örnekte, zaman aralığı ayarlayalım **son 30 gün** ve ölçümü için **işlem**.
4. Sorgu tamamlandıktan sonra işlemi için son 30 güne ilişkin bir görselleştirmedir görürsünüz. Bu verileri dışarı aktarmak için kullanın **Excel'e** düğmesi, sayfanın en üstünde bulunan.

Tanılama verileri ile yapabilecekleriniz hakkında daha fazla bilgi [Azure depolama](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).

## <a name="view-logs-in-log-analytics"></a>Log Analytics’te günlükleri görüntüleme

Log analytics veri kaynağınız için keşfetmek için bu yönergeleri izleyin.

1. Azure portalından bulun ve seçin **Log Analytics** sol taraftaki gezinti menüsünde.
2. Bulun ve tanılama etkinleştirilirken oluşturduğunuz kaynağın seçin.
3. Altında **genel**bulup seçin **günlükleri**. Bu sayfadan günlüklerinizi karşı sorgular çalıştırabilirsiniz.

### <a name="sample-queries"></a>Örnek sorgular

Günlük verilerinizi keşfetmek için kullanabileceğiniz birkaç temel Kusto sorgu aşağıda verilmiştir.

Belirli bir süre için Azure Bilişsel Hizmetler'in sunduğu tüm tanılama günlükleri için bu sorguyu çalıştırın:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
```

10 en son günlükleri görmek için bu sorguyu çalıştırın:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| take 10
```

Grup işlemleri tarafından için bu sorguyu çalıştırmak **kaynak**:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES" |
summarize count() by Resource
```
Bir işlemi gerçekleştirmek için geçen ortalama süreyi bulmak için bu sorguyu çalıştırın:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| summarize avg(DurationMs)
by OperationName
```

Toplu işlem sayısı için her 10s binned olan tarafından OperationName bölme zaman içinde görüntülemek için bu sorguyu çalıştırın.

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| summarize count()
by bin(TimeGenerated, 10s), OperationName
| render areachart kind=unstacked
```

## <a name="next-steps"></a>Sonraki adımlar

* Günlüğe kaydetme ve ayrıca çeşitli Azure Hizmetleri tarafından desteklenen Ölçümler ve günlük kategorileri etkinleştirme anlamak için hem de okuma [ölçümlere genel bakış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) Microsoft azure'da ve [genel bakış, Azure tanılama günlükleri ](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview) makaleler.
* Event hubs hakkında bilgi edinmek için bu makaleleri okuyun:
  * [Azure Event Hubs nedir?](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs)
  * [Event Hubs kullanmaya başlayın](https://docs.microsoft.com/azure/event-hubs/event-hubs-csharp-ephcs-getstarted)
* Okuma [Azure Depolama'dan ölçümleri ve tanılama günlüklerini indirin](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#download-blobs).
* Okuma [anlayın günlük aramaları Azure İzleyici günlüklerine](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-search-new).
