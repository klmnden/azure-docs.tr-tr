---
title: "SQL veri ambarı'nda gereç sorgular için etiketleri kullanma | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL Data Warehouse'da gereç sorgulara etiketleri kullanma ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 44988de8-04c1-4fed-92be-e1935661a4e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 9e75bbe528a427724a623305fbd45e2277e9d0af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>SQL veri ambarı'nda gereç sorgular için etiketleri kullanma
SQL veri ambarı sorgusu etiketleri adlı bir kavram destekler. Herhangi bir derinliğe şimdi geçmeden önce bir bir örneğe bakın:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Bu son satırında sorgu dizesine 'My sorgu etiketi' etiketler. Etiket-Dmv'leri sorgulayabilir olduğu gibi bu seçenek özellikle yararlıdır. Bu bize sorun sorguları izlemek ve ETL Çalıştır ilerlemeyi tanımlamaya yardımcı olmak için bir mekanizma sağlar.

İyi bir adlandırma kuralı gerçekten burada yardımcı olur. Örneğin şuna benzer ' proje: yordamı: DEYİMİ: Açıklama ' sorguyu kaynak denetiminde tüm kod arasında benzersiz şekilde tanımlamak için yardımcı.

Etikete göre aramak için dinamik yönetim görünümlerini kullanan aşağıdaki sorgu kullanabilirsiniz:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Sorgulanırken köşeli veya çift tırnak word etiket kaydırma gereklidir. Etiket ayrılmış bir sözcük ve onu Sınırlanmamış bir hata neden olur.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
