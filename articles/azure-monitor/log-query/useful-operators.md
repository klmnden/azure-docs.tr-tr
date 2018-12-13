---
title: Azure Log Analytics sorguları yararlı işleçler | Microsoft Docs
description: Log Analytics sorgu farklı senaryolar için kullanılacak ortak işlevler.
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
ms.date: 08/21/2018
ms.author: bwren
ms.openlocfilehash: 060b1e469a31c335f062ccd332157d13e64f9318
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53183991"
---
# <a name="useful-operators-in-log-analytics-queries"></a>Log Analytics sorguları yararlı işleçleri

Aşağıdaki tabloda, Log Analytics sorgu farklı senaryolar için kullanılacak ortak bazı işlevler sağlar.

## <a name="useful-operators"></a>Yararlı işleçler

Kategori                                |İlgili Analytics işlevi
----------------------------------------|----------------------------------------
Seçim ve sütun diğer adları            |`project`, `project-away`, `extend`
Geçici tablolar ve sabitler          |`let scalar_alias_name = …;` <br> `let table_alias_name =  …  …  … ;`| 
Karşılaştırma ve dize işleçleri         |`startswith`, `!startswith`, `has`, `!has` <br> `contains`, `!contains`, `containscs` <br> `hasprefix`, `!hasprefix`, `hassuffix`, `!hassuffix`, `in`, `!in` <br> `matches regex` <br> `==`, `=~`, `!=`, `!~`
Ortak dize işlevleri                 |`strcat()`, `replace()`, `tolower()`, `toupper()`, `substring()`, `strlen()`
Ortak matematik işlevleri                   |`sqrt()`, `abs()` <br> `exp()`, `exp2()`, `exp10()`, `log()`, `log2()`, `log10()`, `pow()` <br> `gamma()`, `gammaln()`
Metni ayrıştırma                            |`extract()`, `extractjson()`, `parse`, `split()`
Çıkış sınırlama                         |`take`, `limit`, `top`, `sample`
Tarih işlevleri                          |`now()`, `ago()` <br> `datetime()`, `datepart()`, `timespan` <br> `startofday()`, `startofweek()`, `startofmonth()`, `startofyear()` <br> `endofday()`, `endofweek()`, `endofmonth()`, `endofyear()` <br> `dayofweek()`, `dayofmonth()`, `dayofyear()` <br> `getmonth()`, `getyear()`, `weekofyear()`, `monthofyear()`
Gruplandırma ve toplama                |`summarize by` <br> `max()`, `min()`, `count()`, `dcount()`, `avg()`, `sum()` <br> `stddev()`, `countif()`, `dcountif()`, `argmax()`, `argmin()` <br> `percentiles()`, `percentile_array()`
Birleşimler ve birleşimler                        |`join kind=leftouter`, `inner`, `rightouter`, `fullouter`, `leftanti` <br> `union`
Sıralama, sipariş                             |`sort`, `order` 
Dinamik Nesne (JSON ve dizi)         |`parsejson()` <br> `makeset()`, `makelist()` <br> `split()`, `arraylength()` <br> `zip()`, `pack()`
Mantıksal işleçler                       |`and`, `or`, `iff(condition, value_t, value_f)` <br> `binary_and()`, `binary_or()`, `binary_not()`, `binary_xor()`
Makine öğrenimi                        |`evaluate autocluster`, `basket`, `diffpatterns`, `extractcolumns`


## <a name="next-steps"></a>Sonraki adımlar

- Ders ile devam [Log Analytics'te sorgu yazma](get-started-queries.md).
