---
title: Azure İzleyici'de faydalı işleçleri oturum sorgular | Microsoft Docs
description: Azure İzleyici günlük sorguları farklı senaryolar için kullanılacak ortak işlevler.
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
ms.openlocfilehash: d11445c3f31f9aced6fdb9783575d10a026de1f0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61424147"
---
# <a name="useful-operators-in-azure-monitor-log-queries"></a>Azure İzleyici günlük sorguları yararlı işleçleri

Aşağıdaki tabloda, farklı senaryolarda Azure İzleyici günlük sorguları için kullanılacak ortak bazı işlevler sağlar.

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

- Ders ile devam [Azure İzleyici'de günlük sorguları yazma](get-started-queries.md).
