---
title: Azure Log Analytics'e İşlevler | Microsoft Docs
description: Bu makalede, başka bir sorgu Log analytics'te sorgu çağırmanıza işlevler kullanılacağını açıklar.
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
ms.date: 11/15/2018
ms.author: bwren
ms.openlocfilehash: f5b668c69917cc1ba652be4c93e9da4cb5ef6671
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52681425"
---
# <a name="using-functions-in-azure-monitor-log-analytics"></a>Azure İzleyici Log Analytics'te işlevlerini kullanma

> [!NOTE]
> Tamamlamanız gereken [Analytics portalı ile çalışmaya başlama](get-started-analytics-portal.md) ve [sorguları ile çalışmaya başlama](get-started-queries.md) dersin tamamlamadan önce.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]


Başka bir sorguyla Log Analytics sorgusu kullanmak için bir işlev olarak kaydedebilirsiniz. Bu, karmaşık sorgular parçaya bölerek basitleştirmenize olanak sağlar ve birden fazla sorgu içeren ortak kodun yeniden kullanmanıza izin verir.

## <a name="create-a-function"></a>İşlev oluşturma

Tıklayarak Azure portalında bir işlev oluşturma **Kaydet** ve ardından aşağıdaki tablodaki bilgileri girdikten.

| Ayar | Açıklama |
|:---|:---|
| Ad           | Sorgu için görünen ad **sorgu Gezgini**. |
| Farklı kaydet        | İşlev |
| İşlev Diğer Adı | İşlev diğer sorguları kullanmak için kısa ad. Boşluk içeremez ve benzersiz olmalıdır. |
| Kategori       | Kaydedilmiş Sorgular ve işlevlerde düzenlemek için bir kategori **sorgu Gezgini**. |

> [!NOTE]
> Log analytics'te bir işlev, başka bir işlev içeremez.

> [!NOTE]
> Bir işlev kaydetmek için sorgular Application ınsights'ı Log Analytics sorguları, ancak şu anda mümkündür.



## <a name="use-a-function"></a>Bir işlevi kullanın
Diğer adının başka bir sorguya dahil ederek bir işlevi kullanın. Diğer tablolar gibi kullanılabilir.

## <a name="example"></a>Örnek
Aşağıdaki örnek sorguda, son gün içinde bildirilen tüm eksik güvenlik güncelleştirmelerini döndürür. Bu sorgu, bir işlev diğer adı ile Kaydet _security_updates_last_day_. 

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

Başka bir sorgu ve başvuru oluşturma _security_updates_last_day_ SQL ile ilgili gerekli güvenlik güncelleştirmeleri için aranacak işlev.

```Kusto
security_updates_last_day | where Title contains "SQL"
```

## <a name="next-steps"></a>Sonraki adımlar
Log Analytics sorgu dilini kullanarak için diğer dersler bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamaları](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Birleşimler](joins.md)
- [Grafikler](charts.md)
