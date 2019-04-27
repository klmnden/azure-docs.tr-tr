---
title: Azure İzleyici günlük sorguları işlevleri | Microsoft Docs
description: Bu makalede, Azure İzleyici'de başka bir günlük sorgusundan bir sorgu çağırmak için işlevleri kullanmayı açıklar.
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
ms.openlocfilehash: 7eb5f090026b415e3c980116a9317594f4ca8115
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60728075"
---
# <a name="using-functions-in-azure-monitor-log-queries"></a>Azure izleyici günlüğü sorgularda işlevlerini kullanma

> [!NOTE]
> Tamamlamanız gereken [Analytics portalı ile çalışmaya başlama](get-started-portal.md) ve [sorguları ile çalışmaya başlama](get-started-queries.md) dersin tamamlamadan önce.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]


Günlük sorgusu başka bir sorgu ile kullanmak için bir işlev olarak kaydedebilirsiniz. Bu, karmaşık sorgular parçaya bölerek basitleştirmenize olanak sağlar ve birden fazla sorgu içeren ortak kodun yeniden kullanmanıza izin verir.

## <a name="create-a-function"></a>İşlev oluşturma

Azure portalında Log Analytics ile tıklayarak bir işlev oluşturma **Kaydet** ve ardından aşağıdaki tablodaki bilgileri girdikten.

| Ayar | Açıklama |
|:---|:---|
| Ad           | Sorgu için görünen ad **sorgu Gezgini**. |
| Farklı kaydet        | İşlev |
| İşlev Diğer Adı | İşlev diğer sorguları kullanmak için kısa ad. Boşluk içeremez ve benzersiz olmalıdır. |
| Kategori       | Kaydedilmiş Sorgular ve işlevlerde düzenlemek için bir kategori **sorgu Gezgini**. |

> [!NOTE]
> Azure İzleyici'de bir işlev, başka bir işlev içeremez.

> [!NOTE]
> Bir işlev kaydetmek için sorgular Application ınsights'ı Azure izleyici günlüğü sorgularda, ancak şu anda mümkündür.



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
Azure İzleyici günlük sorguları yazma diğer dersler bakın:

- [Dize işlemleri](string-operations.md)
- [Tarih ve saat işlemleri](datetime-operations.md)
- [Toplama işlevleri](aggregations.md)
- [Gelişmiş toplamaları](advanced-aggregations.md)
- [JSON ve veri yapıları](json-data-structures.md)
- [Birleşimler](joins.md)
- [Grafikler](charts.md)
