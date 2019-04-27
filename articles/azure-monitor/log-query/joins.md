---
title: Azure izleyici günlüğü sorgularda birleşimler | Microsoft Docs
description: Bu makale, Azure İzleyici günlük sorguları birleşimlerde'ı kullanarak bir ders içerir.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.openlocfilehash: 2ea5b4e3af6591e6e25a863998baa7cecb3e29e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60520105"
---
# <a name="joins-in-azure-monitor-log-queries"></a>Azure izleyici günlüğü sorgularda birleştirir

> [!NOTE]
> Tamamlamanız gereken [Azure İzleyici Log Analytics ile çalışmaya başlama](get-started-portal.md) ve [Azure İzleyici günlük sorguları](get-started-queries.md) dersin tamamlamadan önce.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Birleşimler, aynı sorguda birden çok tablodan veri çözümleme sağlar. İki veri kümesi satırlarını tarafından belirtilen sütun eşleşen değerleri birleştirecek.


```Kusto
SecurityEvent 
| where EventID == 4624     // sign-in events
| project Computer, Account, TargetLogonId, LogonTime=TimeGenerated
| join kind= inner (
    SecurityEvent 
    | where EventID == 4634     // sign-out events
    | project TargetLogonId, LogoffTime=TimeGenerated
) on TargetLogonId
| extend Duration = LogoffTime-LogonTime
| project-away TargetLogonId1 
| top 10 by Duration desc
```

Bu örnekte, ilk veri kümesi için tüm oturum açma olayları filtreler. Bu işlem için tüm oturum kapatma olayları filtreler ikinci bir veri kümesi ile birleştirilir. Öngörülen sütunlar _bilgisayar_, _hesabı_, _TargetLogonId_, ve _TimeGenerated_. Veri kümeleri, paylaşılan bir sütuna göre bağıntılı olan _TargetLogonId_. Çıktı, oturum açma ve oturum kapatma süresi olan bağıntı başına tek bir kaydıdır.

Her iki veri kümeleri aynı ada sahip sütun varsa, bu örnekte sonuçları gösterir şekilde sağ taraftaki DataSet sütunları bir dizin numarasını verilmesi _TargetLogonId_ sol taraftaki tablonun değerlerini ve  _TargetLogonId1_ sağ taraftaki tabloda değerler. Bu durumda, ikinci _TargetLogonId1_ sütun kullanarak kaldırıldı `project-away` işleci.

> [!NOTE]
> Performansı artırmak için birleştirilmiş veri kümesinin-kullanarak, yalnızca ilgili sütunları tutun `project` işleci.


İki veri kümesi katılmak için aşağıdaki sözdizimini kullanın ve iki tablo arasında farklı bir ad alanına katılmış anahtar vardır:
```
Table1
| join ( Table2 ) 
on $left.key1 == $right.key2
```

## <a name="lookup-tables"></a>Arama tabloları
Birleşim yaygın kullanımı, değerleri kullanarak statik eşleme kullanarak `datatable` , sonuçları daha edileni yolu dönüştürme de yardımcı olabilir. Örneğin, güvenlik zenginleştirmek için olay verilerini her olay için olay adıyla kimliği.

```Kusto
let DimTable = datatable(EventID:int, eventName:string)
  [
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4658, "The handle to an object was closed",
    4656, "A handle to an object was requested",
    4690, "An attempt was made to duplicate a handle to an object",
    4663, "An attempt was made to access an object",
    5061, "Cryptographic operation",
    5058, "Key file operation"
  ];
SecurityEvent
| join kind = inner
 DimTable on EventID
| summarize count() by eventName
```

![Bir datatable ile birleştirme](media/joins/dim-table.png)

## <a name="join-kinds"></a>Tür katılın
Katılma türü belirtin _tür_ bağımsız değişken. Her tür, aşağıdaki tabloda açıklandığı gibi belirli tablolar kayıtları arasında farklı bir eşleşme gerçekleştirir.

| Katılım Türü | Açıklama |
|:---|:---|
| innerunique | Varsayılan JOIN modu budur. Sol Tablo üzerinde eşleşen sütunun değerlerini ilk bulunan ve yinelenen değerler kaldırılır.  Ardından benzersiz değerler kümesini karşı sağ tablodaki eşleştirilir. |
| İç | Her iki tablodaki yalnızca eşleşen kayıtları sonuçlara dahil edilir. |
| leftouter | Sol tablodaki tüm kayıtları ve sağ tablodaki eşleşen kayıtları sonuçlara dahil edilir. Eşleşmeyen bir çıkış özellikleri null değerler içerir.  |
| leftanti | Soldan sağa eşleşme yok kayıtları sonuçlara dahil edilir. Sonuçlar tablosu, yalnızca sol tablodaki sütun içeriyor. |
| leftsemi | Soldan sağa eşleşen kayıtları sonuçlara dahil edilir. Sonuçlar tablosu, yalnızca sol tablodaki sütun içeriyor. |


## <a name="best-practices"></a>En iyi uygulamalar

En iyi performans için aşağıdaki noktaları göz önünde bulundurun:

- Zaman filtresi her tabloda birleştirme için değerlendirilmesi gereken kayıtları azaltmak için kullanın.
- Kullanım `where` ve `project` girdi tablolarında birleştirme önce içindeki satırları ve sütunları sayıda azaltmak için.
- Bir tablodaki her zaman diğerinden daha küçükse, birleştirmenin sol tarafındaki kullanın.


## <a name="next-steps"></a>Sonraki adımlar
Azure İzleyici günlük sorguları kullanmaya yönelik diğer dersler bakın:

- [Dize işlemleri](string-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamaları](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Grafikler](charts.md)