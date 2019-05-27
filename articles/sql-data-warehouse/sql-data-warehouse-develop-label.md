---
title: SQL veri ambarı'nda gereç sorgular için etiketler kullanarak | Microsoft Docs
description: Çözümleri geliştirmek için Azure SQL veri ambarı'nda gereç sorgular için etiketleri kullanma hakkında ipuçları.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: query
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 5c53fc3594d02c92ea6a238f89417e31dad4818c
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65861779"
---
# <a name="using-labels-to-instrument-queries-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'nda gereç sorgular için etiketleri kullanma
Çözümleri geliştirmek için Azure SQL veri ambarı'nda gereç sorgular için etiketleri kullanma hakkında ipuçları.


## <a name="what-are-labels"></a>Etiketleri nelerdir?
SQL veri ambarı, sorgu etiketleri adlı bir kavram destekler. Herhangi bir derinliğe geçmeden önce bir örneğe göz atalım:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Son satırı sorgu dizesine 'Sorgu etiketi' etiketleri. Etiket Dmv'ler sorgu-mümkün olduğundan bu etiket, özellikle yararlı olur. Etiketler için sorgulama sorguları bulmak ve çalıştırmak için bir ELT ilerlemeyi tanımlamaya yardımcı olmaya yönelik bir mekanizma sağlar.

İyi bir adlandırma kuralı gerçekten yardımcı olur. Örneğin, etiket proje ile başlayarak, yordam, DEYİM veya YORUM sorgu kaynak denetimindeki tüm kod arasında benzersiz olarak tanımlanabilmesi için yardımcı olur.

Aşağıdaki sorgu, etiketle aramak için bir dinamik yönetim görünümünü kullanır.

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Sorgulanırken köşeli parantezler veya word etiket etrafına çift tırnak işareti koymak için gereklidir. Etiket, ayrılmış bir sözcük ve değil sınırlı olduğunda bir hataya neden olur. 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).


