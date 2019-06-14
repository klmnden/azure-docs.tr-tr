---
title: Azure İzleyici günlük sorguları dizeler ile çalışma | Microsoft Docs
description: Bu makalede, sorgu ve Azure İzleyici'de günlük verilerini analiz etmek için Azure portalında Azure İzleyici Log Analytics kullanmaya yönelik bir öğretici sağlar.
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
ms.openlocfilehash: 718b12c8a66d66a75796f88ef31b5f0f62abbbc4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60519638"
---
# <a name="working-with-json-and-data-structures-in-azure-monitor-log-queries"></a>JSON ve veri yapıları Azure İzleyici günlük sorguları içinde çalışma

> [!NOTE]
> Tamamlamanız gereken [Azure İzleyici Log Analytics ile çalışmaya başlama](get-started-portal.md) ve [Azure İzleyici günlük sorguları ile çalışmaya başlama](get-started-queries.md) dersin tamamlamadan önce.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

İç içe nesneler içeren bir dizi veya anahtar-değer çiftlerinin bir eşlem içindeki diğer nesnelerin nesnelerdir. Bu nesneler, JSON dize olarak temsil edilir. Bu makale, JSON verilerini almak ve iç içe geçmiş nesnelerde analiz etmek için nasıl kullanıldığını açıklar.

## <a name="working-with-json-strings"></a>JSON dizeler ile çalışma
Kullanım `extractjson` belirli bir JSON öğesi bilinen bir yolda erişmek için. Bu işlev, aşağıdaki kuralları kullanan bir yol ifadesi gerektirir.

- _$_ kök klasörü belirtmek için
- Aşağıdaki örneklerde gösterildiği gibi dizinleri ve öğeleri başvurmak için köşeli ayraç veya nokta gösterimi kullanın.


Öğeleri ayırmak için dizinler ve nokta için köşeli ayraç kullanın:

```Kusto
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report
| extend status = extractjson("$.hosts[0].status", hosts_report)
```

Bu yalnızca köşeli ayraçlar gösterimini kullanarak aynı sonucu.

```Kusto
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report 
| extend status = extractjson("$['hosts'][0]['status']", hosts_report)
```

Yalnızca bir öğe yoksa yalnızca nokta gösterimi kullanabilirsiniz:

```Kusto
let hosts_report='{"location":"North_DC", "status":"running", "rate":5}';
print hosts_report 
| extend status = hosts_report.status
```


## <a name="working-with-objects"></a>Nesneler ile çalışma

### <a name="parsejson"></a>parsejson
Birden çok öğe, json yapısındaki erişmek için dinamik bir nesne erişmek kolaydır. Kullanım `parsejson` metin verilerine dinamik bir nesne olarak atanamadı. Dinamik bir türe dönüştürülen ek işlevleri ve verileri çözümlemek için kullanılabilir.

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend status0=hosts_object.hosts[0].status, rate1=hosts_object.hosts[1].rate
```



### <a name="arraylength"></a>arraylength
Kullanım `arraylength` bir dizideki öğelerin sayısını:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend hosts_num=arraylength(hosts_object.hosts)
```

### <a name="mvexpand"></a>mvexpand
Kullanım `mvexpand` bir nesnenin özelliklerini farklı satırlara ayırmak için.

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| mvexpand hosts_object.hosts[0]
```

![mvexpand](media/json-data-structures/mvexpand.png)

### <a name="buildschema"></a>buildschema
Kullanım `buildschema` bir nesnenin tüm değerleri admits şeması get yapılmaya:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```

Bir şema JSON biçiminde çıktı.
```json
{
    "hosts":
    {
        "indexer":
        {
            "location": "string",
            "rate": "int",
            "status": "string"
        }
    }
}
```
Bu çıkış nesnesi alanları ve eşleşen veri türlerini açıklar. 

İç içe geçmiş nesnelerde aşağıdaki örnekte olduğu gibi farklı şemalar sahip olabilir:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"status":"stopped", "rate":"3", "range":100}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```


![Şema derleme](media/json-data-structures/buildschema.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure İzleyici'de günlük sorguları kullanmaya yönelik diğer dersler bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamaları](advanced-aggregations.md)
- [Gelişmiş sorgu yazma](advanced-query-writing.md)
- [Birleşimler](joins.md)
- [Grafikler](charts.md)