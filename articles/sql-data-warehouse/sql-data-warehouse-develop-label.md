---
title: SQL veri ambarı'nda gereç sorgular için etiketleri kullanma | Microsoft Docs
description: Çözümleri geliştirme için Azure SQL Data Warehouse'da gereç sorgulara etiketleri kullanma ipuçları.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/12/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 48fe625573639c0ec98e672f02a35e4a9ae268e8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="using-labels-to-instrument-queries-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse'da gereç sorgular için etiketleri kullanma
Çözümleri geliştirme için Azure SQL Data Warehouse'da gereç sorgulara etiketleri kullanma ipuçları.


## <a name="what-are-labels"></a>Etiketleri nelerdir?
SQL veri ambarı sorgusu etiketleri adlı bir kavram destekler. Herhangi bir derinliğe geçmeden önce bir örneğe bakalım:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Son satırı sorgu dizesine 'My sorgu etiketi' etiketler. Bu etiket etiket-Dmv'leri sorgulayabilir olduğundan özellikle yararlıdır. Etiketler için sorgulama sorun sorguları bulma ve Çalıştır ELT ilerlemeyi tanımlamak için yardımcı olmak için bir mekanizma sağlar.

İyi bir adlandırma kuralı gerçekten yardımcı olur. Örneğin, etiket projesi ile başlayarak, yordam, DEYİMİ veya YORUM sorgu kaynak denetiminde tüm kod arasında benzersiz şekilde tanımlamak için yardımcı olur.

Aşağıdaki sorgu dinamik yönetim görünümünü etikete göre aramak için kullanır.

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Köşeli ayraçlar veya word etiketi çift tırnak sorgulanırken koymak için gereklidir. Etiket ayrılmış bir sözcük ve değil ayrılmış bir hata neden olur. 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).


