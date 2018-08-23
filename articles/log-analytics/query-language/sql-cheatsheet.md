---
title: Azure Log Analytics sorgu dil kuralları sayfası SQL | Microsoft Docs
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
ms.devlang: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 8cc05f02d97eac6bb24c4a0565df8c7335c5b33d
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "42447530"
---
# <a name="sql-to-log-analytics-query-language-cheat-sheet"></a>SQL için Log Analytics sorgu dil kuralları sayfası 

Aşağıdaki tabloda, Log Analytics sorgu dili öğrenmek için SQL ile ilgili bilgi sahibi olan kullanıcılara yardımcı olur. Yaygın senaryolar ve Log Analytics kullanarak eşdeğer çözmek için T-SQL komutunu göz vardır.

## <a name="sql-to-log-analytics"></a>Log analytics'e SQL

Açıklama                             |SQL sorgusu                                                                                          |Azure Log Analytics sorgusu
----------------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------
Tüm verileri bir tablo seçin            |`SELECT * FROM dependencies`                                                                       |<code>dependencies</code>
Belirli sütunları tablodan seçin    |`SELECT name, resultCode FROM dependencies`                                                        |<code>dependencies <br>&#124; project name, resultCode</code>
100 kayıt tablodan seçin         |`SELECT TOP 100 * FROM dependencies`                                                               |<code>dependencies <br>&#124; take 100</code>
Null değerlendirme                         |`SELECT * FROM dependencies WHERE resultCode IS NOT NULL`                                          |<code>dependencies <br>&#124; where isnotnull(resultCode)</code>
Dize karşılaştırma: Eşitlik             |`SELECT * FROM dependencies WHERE name = "abcde"`                                                  |<code>dependencies <br>&#124; where name == "abcde"</code>
Dize karşılaştırma: alt dize            |`SELECT * FROM dependencies WHERE like "%bcd%"`                                                    |<code>dependencies <br>&#124; where name contains "bcd"</code>
Dize karşılaştırma: joker karakter             |`SELECT * FROM dependencies WHERE name like "abc%"`                                                |<code>dependencies <br>&#124; where name startswith "abc"</code>
Tarih karşılaştırma: son 1 gün             |`SELECT * FROM dependencies WHERE timestamp > getdate()-1`                                         |<code>dependencies <br>&#124; where timestamp > ago(1d)</code>
Tarih karşılaştırma: tarih aralığı             |`SELECT * FROM dependencies WHERE timestamp BETWEEN '2016-10-01' AND '2016-11-01'`                 |<code>dependencies <br>&#124; where timestamp between (datetime(2016-10-01) .. datetime(2016-10-01))</code>
Boole karşılaştırma                      |`SELECT * FROM dependencies WHERE !(success)`                                                      |<code>dependencies <br>&#124; where success == "False" </code>
Sırala                                    |`SELECT name, timestamp FROM dependencies ORDER BY timestamp asc`                                  |<code>dependencies <br>&#124; order by timestamp asc </code>
Farklı                                |`SELECT DISTINCT name, type  FROM dependencies`                                                    |<code>dependencies <br>&#124; summarize by name, type </code>
Toplama, gruplandırma                   |`SELECT name, AVG(duration) FROM dependencies GROUP BY name`                                       |<code>dependencies <br>&#124; summarize avg(duration) by name </code>
Sütun diğer adları, Genişlet                  |`SELECT operation_Name as Name, AVG(duration) as AvgD FROM dependencies GROUP BY name`             |<code>dependencies <br>&#124; summarize AvgD=avg(duration) by operation_Name <br>&#124; project Name=operation_Name, AvgD</code>
Üst n recrods ölçü tarafından                |`SELECT TOP 100 name, COUNT(*) as Count FROM dependencies GROUP BY name ORDER BY Count asc`        |<code>dependencies <br>&#124; summarize Count=count() by name <br>&#124; top 100 by Count asc</code>
birleşim                                   |`SELECT * FROM dependencies UNION SELECT * FROM exceptions`                                        |<code>union dependencies, exceptions</code>
Union: tüm koşullar                  |`SELECT * FROM dependencies WHERE value > 4 UNION SELECT * FROM exceptions value < 5`              |<code>dependencies <br>&#124; where value > 4 <br>&#124; union (exceptions <br>&#124; where value < 5)</code>
Birleştir                                    |`SELECT * FROM dependencies JOIN exceptions ON dependencies.operation_Id = exceptions.operation_Id`|<code>dependencies <br>&#124; join (exceptions) on operation_Id == operation_Id</code>


## <a name="next-steps"></a>Sonraki adımlar

- Ders ile devam [Log Analytics'te sorgu yazma](get-started-queries.md).