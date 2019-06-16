---
title: Azure İzleyici günlük sorgu kopya kağıdı SQL | Microsoft Docs
description: SQL ile ilgili bilgi sahibi olan kullanıcılar için Azure İzleyici'de günlük sorguları yazma yardımcı olur.
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
ms.openlocfilehash: b756b9484273c098dbeb6685430f70626b3af787
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65789226"
---
# <a name="sql-to-azure-monitor-log-query-cheat-sheet"></a>SQL Azure İzleyici günlük sorgu kopya kağıdı 

Aşağıdaki tabloda, Azure İzleyici'de günlük sorguları yazma Kusto sorgu dili öğrenmek için SQL ile ilgili bilgi sahibi olan kullanıcılara yardımcı olur. Yaygın senaryolar ve bir Azure İzleyici günlük sorgusu eşdeğer çözmek için T-SQL komutunu göz vardır.

## <a name="sql-to-azure-monitor"></a>SQL Azure izleme

Açıklama                             |SQL sorgusu                                                                                          |Azure İzleyici günlük sorgusu
----------------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------
Tüm verileri bir tablo seçin            |`SELECT * FROM dependencies`                                                                       |<code>dependencies</code>
Belirli sütunları tablodan seçin    |`SELECT name, resultCode FROM dependencies`                                                        |<code>dependencies <br>&#124; project name, resultCode</code>
100 kayıt tablodan seçin         |`SELECT TOP 100 * FROM dependencies`                                                               |<code>dependencies <br>&#124; take 100</code>
Null değerlendirme                         |`SELECT * FROM dependencies WHERE resultCode IS NOT NULL`                                          |<code>dependencies <br>&#124; where isnotnull(resultCode)</code>
Dize karşılaştırma: Eşitlik             |`SELECT * FROM dependencies WHERE name = "abcde"`                                                  |<code>dependencies <br>&#124; where name == "abcde"</code>
Dize karşılaştırma: alt dize            |`SELECT * FROM dependencies WHERE name like "%bcd%"`                                                   |<code>dependencies <br>&#124; where name contains "bcd"</code>
Dize karşılaştırma: joker karakter             |`SELECT * FROM dependencies WHERE name like "abc%"`                                                |<code>dependencies <br>&#124; where name startswith "abc"</code>
Tarih karşılaştırma: son 1 gün             |`SELECT * FROM dependencies WHERE timestamp > getdate()-1`                                         |<code>dependencies <br>&#124; where timestamp > ago(1d)</code>
Tarih karşılaştırma: tarih aralığı             |`SELECT * FROM dependencies WHERE timestamp BETWEEN '2016-10-01' AND '2016-11-01'`                 |<code>dependencies <br>&#124; where timestamp between (datetime(2016-10-01) .. datetime(2016-10-01))</code>
Boole karşılaştırma                      |`SELECT * FROM dependencies WHERE !(success)`                                                      |<code>dependencies <br>&#124; where success == "False"</code>
Sırala                                    |`SELECT name, timestamp FROM dependencies ORDER BY timestamp asc`                                  |<code>dependencies <br>&#124; order by timestamp asc</code>
Farklı                                |`SELECT DISTINCT name, type  FROM dependencies`                                                    |<code>dependencies <br>&#124; summarize by name, type</code>
Toplama, gruplandırma                   |`SELECT name, AVG(duration) FROM dependencies GROUP BY name`                                       |<code>dependencies <br>&#124; summarize avg(duration) by name</code>
Sütun diğer adları, Genişlet                  |`SELECT operation_Name as Name, AVG(duration) as AvgD FROM dependencies GROUP BY name`             |<code>dependencies <br>&#124; summarize AvgD=avg(duration) by operation_Name <br>&#124; project Name=operation_Name, AvgD</code>
Ölçü göre üst n kayıtlar                |`SELECT TOP 100 name, COUNT(*) as Count FROM dependencies GROUP BY name ORDER BY Count asc`        |<code>dependencies <br>&#124; summarize Count=count() by name <br>&#124; top 100 by Count asc</code>
Birleşim                                   |`SELECT * FROM dependencies UNION SELECT * FROM exceptions`                                        |<code>union dependencies, exceptions</code>
Union: tüm koşullar                  |`SELECT * FROM dependencies WHERE value > 4 UNION SELECT * FROM exceptions WHERE value < 5`                |<code>dependencies <br>&#124; where value > 4 <br>&#124; union (exceptions <br>&#124; where value < 5)</code>
Katıl                                    |`SELECT * FROM dependencies JOIN exceptions ON dependencies.operation_Id = exceptions.operation_Id`|<code>dependencies <br>&#124; join (exceptions) on operation_Id == operation_Id</code>


## <a name="next-steps"></a>Sonraki adımlar

- Dersler go [Azure İzleyici'de günlük sorguları yazma](get-started-queries.md).
