---
title: Azure Log Analytics sorguları grafikleri ve diyagramları oluşturma | Microsoft Docs
description: Azure Log Analytics, verilerinizi farklı şekillerde göstermek için çeşitli görselleştirmeler açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 359e5e671287c4d330deeb2d3573877d9ee5d1c5
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "40190334"
---
# <a name="creating-charts-and-diagrams-from-log-analytics-queries"></a>Log Analytics sorguları grafikleri ve diyagramları oluşturma

> [!NOTE]
> Tamamlamanız gereken [Log Analytics sorguları toplamalara Gelişmiş](advanced-aggregations.md) dersin tamamlamadan önce.

Bu makalede, Azure Log Analytics, verilerinizi farklı şekillerde göstermek için çeşitli görselleştirmeler açıklanır.

## <a name="charting-the-results"></a>Sonuç grafiği
Son bir saat işletim sistemi vardır kaç bilgisayarın gözden geçirerek başlayın:

```OQL
Heartbeat
| where TimeGenerated > ago(1h)
| summarize count(Computer) by OSType  
```

Varsayılan olarak, sonuçları bir tablo olarak görüntülenir:

![Tablo](media/charts/table-display.png)

Daha iyi bir görünüm elde edin seçin **grafik**ve **pasta** sonuçların görselleştirilmesi için seçeneği:

![Pasta grafiği](media/charts/charts-and-diagrams-pie.png)


## <a name="timecharts"></a>Timecharts
Depo 1 saat ortalama, 50. ve 95. yüzdebirlik değerleri işlemci zamanı gösterir. Sorgu birden fazla seri oluşturur ve hangi serisi zaman grafikte gösterilecek daha sonra seçebilirsiniz:

```OQL
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
```

Seçin **satırı** grafik görüntüleme seçeneği:

![Çizgi grafik](media/charts/charts-and-diagrams-multiSeries.png)

### <a name="reference-line"></a>Başvuru çizgisi

Başvuru çizgisi ölçüm, belirli bir eşiği aşılırsa bir kolayca belirlemenize yardımcı olabilir. Grafiğe bir satır eklemek için bir sabit sütun kümesiyle genişletin:

```OQL
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
| extend Threshold = 20
```

![Başvuru çizgisi](media/charts/charts-and-diagrams-multiSeriesThreshold.png)

## <a name="multiple-dimensions"></a>Birden çok boyut
Birden çok ifadelerinde `by` yan tümcesi `summarize` birden çok satır sonuçları her değerlerinin birleşimi için bir tane oluşturun.

```OQL
SecurityEvent
| where TimeGenerated > ago(1d)
| summarize count() by tostring(EventID), AccountType, bin(TimeGenerated, 1h)
```

Sonuçları bir grafik olarak görüntüleyin, ilk sütunu kullandığı `by` yan tümcesi. Aşağıdaki örnek, yığılmış sütun grafiği ile gösterir _EventID._ Boyutlar olmalıdır `string` türü, bu örnekte bunu _EventID_ dizeye dönüştürün. 

![Çubuk grafik EventID](media/charts/charts-and-diagrams-multiDimension1.png)

Sütun adıyla açılır menüyü seçerek arasında geçiş yapabilirsiniz. 

![Çubuk grafik AccountType](media/charts/charts-and-diagrams-multiDimension2.png)

## <a name="next-steps"></a>Sonraki adımlar
Log Analytics sorgu dilini kullanarak için diğer dersler bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamaları](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleşimler](joins.md)