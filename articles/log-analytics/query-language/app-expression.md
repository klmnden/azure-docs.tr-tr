---
title: Azure Log Analytics sorgu ifadesinde App() | Microsoft Docs
description: Uygulama ifadesi bir Log Analytics sorgu aynı kaynak grubunu, başka bir kaynak grubu veya başka bir aboneliğe belirli bir Application Insights uygulamasından veri almak için kullanılır.
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
ms.topic: article
ms.date: 09/10/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: b6e3311c378f648caeae72ff1f8aff123515226b
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45637439"
---
# <a name="app-expression-in-log-analytics-query"></a>Log Analytics sorgu ifadesinde App()

`app` İfade, aynı kaynak grubunu, başka bir kaynak grubu veya başka bir aboneliğe belirli bir Application Insights uygulamasından veri almak için bir Log Analytics sorgu kullanılır. Uygulama verileri bir Application Insights sorgu birden çok uygulama arasında bir Log Analytics sorgu ve veri eklemek kullanışlıdır.



## <a name="syntax"></a>Sözdizimi

`app(`*tanımlayıcı*`)`


## <a name="arguments"></a>Bağımsız Değişkenler

- *Tanımlayıcı*: Aşağıdaki tabloda biçimlerden birini kullanarak uygulamayı tanımlar.

| Tanımlayıcı | Açıklama | Örnek
|:---|:---|:---|
| Kaynak Adı | İnsan tarafından okunabilir adını (AKA "bileşen adı") | App("fabrikamapp") |
| Tam adı | Form uygulamasında tam adı: "resourceGroup/subscriptionName/componentName" | App('AI-prototype/Fabrikam/fabrikamapp') |
| Kimlik | Uygulama GUID'si | App("988ba129-363e-4415-8fe7-8cbab5447518") |
| Azure kaynak kimliği | Azure kaynak tanımlayıcısı |App("/Subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/Microsoft.insights/Components/fabrikamapp") |


## <a name="notes"></a>Notlar

* Uygulamanın okuma erişimi olmalıdır.
* Bir uygulama adıyla tanımlama, tüm erişilebilir abonelikler arasında benzersiz olduğundan varsayar. Belirtilen ada sahip birden çok uygulamalarınız varsa, sorgu belirsizlik nedeniyle başarısız olur. Bu durumda diğer tanımlayıcılarla birini kullanmanız gerekir.
* İlgili ifade kullanın [çalışma](workspace-expression.md) Log Analytics çalışma alanları arasında sorgulanamıyor.

## <a name="examples"></a>Örnekler

```
app("fabrikamapp").requests | count
```
```
app("AI-Prototype/Fabrikam/fabrikamapp").requests | count
```
```
app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count
```
```
app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
```
```
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [çalışma ifade](workspace-expression.md) Log Analytics çalışma alanına başvurmak için.
- Nasıl çalıştıracağınızı okuyun [Log Analytics verilerini](../../log-analytics/log-analytics-log-search.md) depolanır.