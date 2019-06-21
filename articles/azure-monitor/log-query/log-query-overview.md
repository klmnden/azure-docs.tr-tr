---
title: Günlüğüne genel bakış, Azure İzleyicisi'nde sorgular | Microsoft Docs
description: Sık sorulan soruların yanıtlarını sorguları günlük aramayla ilgili ve bunları kullanarak başlamanızı sağlar.
services: log-analytics
author: bwren
ms.service: log-analytics
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: bwren
ms.openlocfilehash: b395b7bccbf93b56e84d5e7b5a4ed7355eaca335
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296323"
---
# <a name="overview-of-log-queries-in-azure-monitor"></a>Azure İzleyici'de günlük sorguları genel bakış
Günlük sorguları değeri tam olarak yararlanmak için yardımcı olarak toplanan veriler [Azure İzleyici günlüklerine](../platform/data-platform-logs.md). Güçlü bir sorgu dili, birden çok tablodan veri katılın, büyük veri kümelerini bir araya ve karmaşık işlemleri olabildiğince az kodla olanak tanır. Herhangi bir sorunun yanıtlanması gereken ve destekleyici veri toplanan ve doğru bir sorgu oluşturmak öğrenme sürece çözümlemenin.

Azure İzleyici gibi bazı özellikler [ınsights](../insights/insights-overview.md) ve [çözümleri](../insights/solutions-inventory.md) temel sorgular sokmadan günlük verileri işleyin. Azure İzleyicisi'nin diğer özellikleri tam olarak yararlanmak için sorguların nasıl oluşturulur ve Azure İzleyici günlüklerine verileri etkileşimli olarak çözümlemek için bunları nasıl kullanabileceğiniz anlamanız gerekir.

Bu makalede, Azure İzleyici'de günlük sorguları hakkında daha fazla bilgi için bir başlangıç noktası olarak kullanın. Sık sorulan soruları yanıtlar ve daha fazla ayrıntı ve çıkarılan dersler sağlayan diğer belgeler için bağlantılar sağlar.

## <a name="how-can-i-learn-how-to-write-queries"></a>Sorguları yazma konusunda bilgi nasıl edinebilirim?
Bir şeyler sağ atlamak istiyorsanız, aşağıdaki öğreticileri ile başlayabilirsiniz:

- [Azure İzleyici'de Log Analytics ile çalışmaya başlama](get-started-portal.md).
- [Azure İzleyici'de günlük sorguları kullanmaya başlama](get-started-queries.md).

Aşağı temelleri oluşturduktan sonra kendi veri veya tanıtım ortamımızda başlayarak verileri kullanarak birden çok dersleri yol: 

- [Azure İzleyici günlük sorguları dizelerle çalışma](string-operations.md)
 
## <a name="what-language-do-log-queries-use"></a>Hangi dilde sorguları kullanmak günlüğe?
Azure İzleyici günlüklerine dayanır [Azure Veri Gezgini](/azure/data-explorer), ve günlük sorguları aynı Kusto sorgu dili (KQL) kullanarak yazılır. Bu kolay okumak tasarlanmış zengin bir dil ve yazar ve minimum yönlendirme ile kullanarak başlatabilmeniz.

Bkz: [Azure Veri Gezgini KQL belgeleri](/azure/kusto/query) KQL ve kullanılabilir farklı işlevlerde başvuru kapsamlı belgeler için.<br>
Bkz: [Azure İzleyici'de günlük sorguları kullanmaya başlama](get-started-queries.md) dilinin Azure İzleyici günlüklerine verilerini kullanarak hızlı bir kılavuz.
Bkz [Azure İzleyici günlük sorgu dili farklar](data-explorer-difference.md) küçük farklılıkları KQL Azure İzleyici tarafından kullanılan sürümü.

## <a name="what-data-is-available-to-log-queries"></a>Hangi verilerin sorgular günlüğe kullanılabilir mi?
Azure İzleyici günlüklerine toplanan tüm verileri almak ve günlük sorguları analiz etmek kullanılabilir. Farklı veri kaynakları, farklı tablolara verilerini yazar, ancak birden çok tablo verilerini birden fazla kaynak arasında analiz etmek için tek bir sorgu ekleyebilirsiniz. Bir sorgu oluşturduğunuzda, Azure İzleyici günlüklerine verileri nasıl yapılandırıldığını, en az bir temel anlamış olmanız gerekir böylece hangi tabloların, aradığınız veriniz belirleyerek işe başlayın.

Bkz: [kaynakları Azure İzleyici günlüklerine](../platform/data-platform-logs.md#sources-of-azure-monitor-logs), bir liste farklı veri kaynakları için Azure İzleyici günlüklerine doldurun.<br>
Bkz [Azure İzleyici günlüklerine yapısı](logs-structure.md) verileri nasıl yapılandırıldığını'nın açıklaması.

## <a name="what-does-a-log-query-look-like"></a>Günlük sorgusu ne gibi görünüyor?
Bir sorgu, bu tablodan tüm kayıtları almak için tek bir tablo adı olarak basit olabilir:

```Kusto
Syslog
```

Veya belirli kayıtları filtreleyin, özetleyin ve sonuçları bir grafikte görselleştirme:

```
SecurityEvent
| where TimeGenerated > ago(7d)
| where EventID == 4625
| summarize count() by Computer, bin(TimeGenerated, 1h)
| render timechart 
```

Daha karmaşık bir analiz için sonuçları birlikte analiz etmek için bir birleşim kullanarak birden çok tablolarından verileri alın.

```Kusto
app("ContosoRetailWeb").requests
| summarize count() by bin(timestamp,1hr)
| join kind= inner (Perf
    | summarize avg(CounterValue) 
      by bin(TimeGenerated,1hr))
on $left.timestamp == $right.TimeGenerated
```
KQL ile tanıdık olmayan olsa bile, bu sorgular tarafından kullanılan temel mantığı en az ekleyeceğimi olmalıdır. Bunlar, bir tablonun adı ile başlatın ve ardından bu verileri filtreleme ve birden çok komut eklemek. Bir sorgu herhangi bir sayıda komutları kullanabilirsiniz ve daha karmaşık sorgular farklı KQL komutlarla öğrenmeniz olarak yazabilirsiniz.

Bkz: [Azure İzleyici'de günlük sorguları kullanmaya başlama](get-started-queries.md) dil ve ortak işlevleri tanıtan günlük sorguları öğretici.<br>


## <a name="what-is-log-analytics"></a>Log Analytics nedir?
Log Analytics, günlük sorguları yazma ve sonuçları etkileşimli olarak çözümlemek için Azure portalında birincil araçtır. Başka bir yerde Azure İzleyici'de kullanılan günlük sorgu bile genellikle yazma ve sorgu testi ilk Log Analytics kullanarak.

Log Analytics, Azure portalında çeşitli yerlerde başlayabilirsiniz. Log Analytics'e kullanılabilir verilerin kapsamını nasıl başlattığınızda tarafından belirlenir. Bkz: [sorgu kapsamı](scope.md) daha fazla ayrıntı için.

- Seçin **günlükleri** gelen **Azure İzleyici** menüsü veya **Log Analytics çalışma alanları** menüsü.
- Seçin **Analytics** gelen **genel bakış** bir Application Insights uygulama sayfası.
- Seçin **günlükleri** menüsünden bir Azure kaynağı.

![Log Analytics](media/log-query-overview/log-analytics.png)

Bkz: [Azure İzleyici'de Log Analytics ile çalışmaya başlama](get-started-portal.md) öğretici kılavuz Log Analytics, çeşitli özellikleri tanıtır.

## <a name="where-else-are-log-queries-used"></a>Günlük sorguları başka nerede kullanılır?
Etkileşimli olarak günlük sorguları ve sonuçlarını Log analytics'teki çalışma ek olarak, Azure İzleyici sorguları burada kullanacağınız alanları şunlardır:

- **Uyarı kuralları.** [Uyarı kuralları](../platform/alerts-overview.md) çalışma alanınızdaki veri sorunları proaktif olarak belirleyin.  Her uyarı kuralı otomatik olarak düzenli aralıklarla çalışan bir günlük araması temel alır.  Sonuçları bir uyarının oluşturulması gerektiğini belirlemek için incelenir.
- **Panolar.** Herhangi bir sorgu sonuçlarını sabitleyebilirsiniz bir [Azure panosuna](../learn/tutorial-logs-dashboards.md) olan izin, günlük ve ölçüm verileri bir araya görselleştirip, isteğe bağlı olarak diğer Azure kullanıcıları ile paylaşın.
- **Görünümler.**  Kullanıcı panolarla dahil edilecek veri görselleştirmeleri oluşturabilirsiniz [Görünüm Tasarımcısı](../platform/view-designer.md).  Günlük sorguları tarafından kullanılan verileri sağlar [kutucukları](../platform/view-designer-tiles.md) ve [görselleştirme bölümleri](../platform/view-designer-parts.md) her görünümde.  
- **Dışarı aktarın.**  Aktardığınızda günlük verilerini Azure İzleyici'den Excel'e veya [Power BI](../platform/powerbi.md), dışarı aktarmak için verileri tanımlamak için bir günlük sorgusu oluşturun.
- **PowerShell.** Bir komut satırı veya kullanan bir Azure Otomasyonu runbook'u bir PowerShell Betiği çalıştırabilirsiniz [Get-AzOperationalInsightsSearchResults](/powershell/module/az.operationalinsights/get-azoperationalinsightssearchresult) günlük verilerini Azure İzleyici'den alınamadı.  Bu cmdlet, alınacak verileri belirlemek üzere bir sorgu gerektirir.
- **Azure İzleyici günlüklerine API.**  [Azure İzleyici günlüklerine API](../platform/alerts-overview.md) çalışma alanından günlük verilerini almak herhangi bir REST API istemcisi sağlar.  API isteği almak için verileri belirlemek için Azure İzleyici'karşı çalışan bir sorgu içerir.


## <a name="next-steps"></a>Sonraki adımlar
- İzlenecek yol bir [Azure portalında Log Analytics kullanma Öğreticisi](get-started-portal.md).
- İzlenecek yol bir [sorgu yazmakla ilgili öğretici](get-started-queries.md).
