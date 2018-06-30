---
title: Azure günlük analizi aramaları oturum | Microsoft Docs
description: Günlük analizi herhangi bir veri almak için bir günlük arama gerektirir.  Bu makalede aramaları günlük analizi kullanılan nasıl yeni bir günlük açıklar ve bir oluşturmadan önce anlamanız gereken kavramlar sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: bwren
ms.component: na
ms.openlocfilehash: d1cd4f938092c6a1312bd0c0ec9240d459d67e83
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131296"
---
# <a name="understanding-log-searches-in-log-analytics"></a>Günlük analizi günlük aramalarda anlama

Günlük analizi herhangi bir veri almak için bir günlük arama gerektirir.  Portal verileri analiz etme olup olmadığını belirli bir koşula bildirim almak için bir uyarı kuralı yapılandırma ya da günlük analizi API'sini kullanarak veri alma günlük arama istediğiniz verileri belirtmek için kullanın.  Bu makalede, günlük aramaları günlük analizi nasıl kullanıldığını açıklar ve bir oluşturmadan önce anlamanız gereken kavramlar sağlar. Bkz: [sonraki adımlar](#next-steps) bölüm oluşturma ve günlük aramaları düzenleme hakkında bilgi ve başvurular sorgu dili.

## <a name="where-log-searches-are-used"></a>Günlük aramaları kullanıldığı

Günlük analizi günlük aramaları kullanacağını farklı yolları aşağıdakileri içerir:

- **Portalları.** Azure portalında deposunda etkileşimli veri analizi gerçekleştirebilir veya [Advanced Analytics portalı](https://go.microsoft.com/fwlink/?linkid=856587).  Sorgunuzu düzenleme ve çeşitli biçimlerde ve görselleştirmeleri sonuçlarında çözümlemek sağlar.  Oluşturduğunuz sorguların çoğu portalları birinde başlar ve beklendiği gibi çalıştığını doğrulamak sonra sonra kopyalanır.
- **Uyarı kuralları.** [Uyarı kuralları](log-analytics-alerts.md) proaktif olarak çalışma alanınızdaki verilerden sorunları belirlemek.  Her uyarı kuralı düzenli aralıklarla otomatik olarak çalışacak bir günlük arama temel alır.  Sonuçları bir uyarı oluşturulup oluşturulmayacağını belirlemek için incelenir.
- **Görünümler.**  Kullanıcı panolarla dahil edilecek veri görselleştirmeleri oluşturabilirsiniz [Görünüm Tasarımcısı](log-analytics-view-designer.md).  Günlük aramaları sağlayan tarafından kullanılan veri [kutucukları](log-analytics-view-designer-tiles.md) ve [görselleştirme bölümleri](log-analytics-view-designer-parts.md) her görünümünde.  Görselleştirme parçalarını başka verileri analizler yapmak için günlük arama sayfasına ayrıntıya girebilirsiniz.
- **Dışarı aktarın.**  Verileri verdiğinizde günlük analizi çalışma alanından Excel'e veya [Power BI](log-analytics-powerbi.md), dışarı aktarmak için verileri tanımlamak için bir günlük arama oluşturun.
- **PowerShell.** Bir komut satırı veya kullanan bir Azure Otomasyonu runbook'u bir PowerShell betiğini çalıştırabilirsiniz [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) günlük analizi veri alınamadı.  Bu cmdlet, verileri almak için belirlemek üzere bir sorgu gerektiriyor.
- **Günlük analizi API.**  [Günlük analizi oturum arama API](log-analytics-log-search-api.md) veri çalışma alanından almak herhangi bir REST API istemcisi sağlar.  API isteği almak üzere veri belirlemek için günlük analizi karşı çalıştırmak bir sorgu içerir.

![Günlük aramalar](media/log-analytics-log-search-new/log-search-overview.png)

## <a name="how-log-analytics-data-is-organized"></a>Günlük analizi veri nasıl düzenlenir
Bir sorgu oluşturma sırasında hangi tabloları aradığınız verileriniz belirleyerek başlayın. Her [veri kaynağı](log-analytics-data-sources.md) ve [çözüm](../operations-management-suite/operations-management-suite-solutions.md) günlük analizi çalışma alanındaki özel tablolardaki verileri depolar.  Her bir veri kaynağı ve çözüm için belgeler oluşturur veri türünün adını ve açıklamasını her özelliklerini içerir.  Birçok sorgular yalnızca tek bir tablodan veri gerektirir ancak başkalarının birden çok tablodan veri dahil etmek için çeşitli seçenekler kullanabilir.

![Tablolar](media/log-analytics-log-search-new/queries-tables.png)


## <a name="writing-a-query"></a>Sorgu yazma
Günlük analizi aramalarda günlük özünde olan [kapsamlı sorgu dili](https://docs.loganalytics.io/) olanak tanıyan almak ve verilerini çeşitli şekillerde depodan çözümleme.  Bu aynı sorgu dili için kullanılan [Application Insights](../application-insights/app-insights-analytics.md).  Bir sorgu yazmak öğrenme günlük analizi günlük aramaları oluşturmak için kritik öneme sahiptir.  Genellikle temel sorgularıyla başlatın ve sonra gereksinimlerinizi daha karmaşık hale geldikçe daha gelişmiş işlevleri kullanmak için ilerleme.

Bir sorguyu temel yapısını işleçleri dikey çizgi karakteriyle ayrılmış bir dizi arkasından bir kaynak tablodur `|`.  Veri daraltın ve gelişmiş işlevleri gerçekleştirmek için birden çok işleç zincir.

Örneğin, çoğu hata olayları üst on bilgisayarlarla içinde son gün bulmak istediğinizi varsayalım.

    Event
    | where (EventLevelName == "Error")
    | where (TimeGenerated > ago(1days))
    | summarize ErrorCount = count() by Computer
    | top 10 by ErrorCount desc

Veya, bir sinyal son günü sahip olmayan bilgisayarları bulmak istediğiniz olabilir.

    Heartbeat
    | where TimeGenerated > ago(7d)
    | summarize max(TimeGenerated) by Computer
    | where max_TimeGenerated < ago(1d)  

Ne dersiniz geçen hafta her bilgisayar için işlemci kullanımı ile çizgi grafiği?

    Perf
    | where ObjectName == "Processor" and CounterName == "% Processor Time"
    | where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
    | summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
    | render timechart    

Bu hızlı örneklerinden sorgu yapısı birlikte çalıştığınız veri türü bağımsız olarak benzer olduğunu görebilirsiniz.  Bunu burada bir komut sonuç verileri ardışık düzen üzerinden sonraki komuta gönderilen ayrı adımlara ayırabilirsiniz.

Ayrıca, günlük analizi çalışma alanları arasında aboneliğinizi içinde verileri sorgulayabilir.

    union Update, workspace("contoso-workspace").Update
    | where TimeGenerated >= ago(1h)
    | summarize dcount(Computer) by Classification 


Öğreticiler ve dil başvurusu dahil olmak üzere Azure günlük analizi sorgu dili hakkında kapsamlı belgeler için bkz: [Azure günlük analizi sorgu dili belgelerini](https://docs.loganalytics.io/).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [oluşturmak ve düzenlemek için portalı oturum aramaları](log-analytics-log-search-portals.md).
- Kullanıma bir [sorgu yazmakla ilgili öğretici](log-analytics-tutorial-viewdata.md) yeni sorgu dilini kullanma.
