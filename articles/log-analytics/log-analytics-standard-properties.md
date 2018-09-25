---
title: Azure İzleyici Log Analytics kayıtları standart özelliklerinde | Microsoft Docs
description: Azure İzleyici Log analytics'te birden çok veri türü için ortak olan özellikleri açıklar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 663f0b04c528c180e4130c1c157441cbc0ceb98b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955876"
---
# <a name="standard-properties-in-log-analytics-records"></a>Log Analytics kayıtları standart özellikler
Verileri [Log Analytics](../log-analytics/log-analytics-queries.md) kümesi her bir özellik kümesi olan bir özel veri türü ile kayıt olarak depolanır. Birçok veri türleri, birden çok türlerinde ortak olan standart özellikleri olacaktır. Bu makalede, bu özellikleri açıklar ve nasıl bunları sorgularında kullanabileceğiniz örnekler sağlar.

Bazı veri türleri, ancak henüz diğerleri bunları görebilirsiniz bu özelliklerin bazıları hala uygulanan sürecinde, olduğundan.


## <a name="resourceid"></a>_ResourceId
**_ResourceId** özelliği, kaydı ile ilişkili kaynak için benzersiz bir tanımlayıcı tutar. Bu, sorgunuzu kayıtlarına yalnızca belirli bir kaynaktan kapsam veya ilgili verileri birden çok tabloda katılmak için kullanılacak bir standart özelliği sağlar.

Değerini, Azure kaynakları için **_ResourceId** olduğu [Azure kaynak kimliği URL](../azure-resource-manager/resource-group-template-functions-resource.md). Şu anda Azure kaynaklarına sınırlı bir özelliğidir, ancak şirket içi bilgisayarlar gibi Azure dışındaki kaynaklar için genişletilir. 

### <a name="examples"></a>Örnekler
Aşağıdaki örnek, performans ve her bilgisayar için olay verilerini birleştiren bir sorgu gösterir. Kimliğine sahip tüm olaylar gösterir _101_ ve işlemci kullanımı % 50 üzerindeki.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

Aşağıdaki örnekte gösterilmektedir birleştiren bir sorgu _AzureActivity_ kayıtlar _SecurityEvent_ kaydeder. Bu, tüm etkinlik işlemleri bu makinelerde oturum açmış kullanıcıları gösterir.

```Kusto
AzureActivity 
| where  
    OperationName in ("Restart Virtual Machine", "Create or Update Virtual Machine", "Delete Virtual Machine")  
    and ActivityStatus == "Succeeded"  
| join kind= leftouter (    
   SecurityEvent 
   | where EventID == 4624  
   | summarize LoggedOnAccounts = makeset(Account) by _ResourceId 
) on _ResourceId  
```

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl hakkında daha fazla bilgiyi [Log Analytics verilerinin depolandığı](../log-analytics/log-analytics-queries.md).
- Ders almak [Log Analytics'te sorgu yazma](../log-analytics/query-language/get-started-queries.md).
- Ders almak [Log Analytics sorguları tabloları birleştirme](../log-analytics/query-language/joins.md).