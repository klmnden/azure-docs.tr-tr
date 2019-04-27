---
title: Azure İzleyici günlük sorguları App() ifadesinde | Microsoft Docs
description: Uygulama ifadesi, bir Azure İzleyici günlük sorgusu aynı kaynak grubunu, başka bir kaynak grubu veya başka bir aboneliğe belirli bir Application Insights uygulamasından veri almak için kullanılır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/25/2019
ms.author: bwren
ms.openlocfilehash: a1a605bc733597430f64dceeb6c485db0abf657b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60589241"
---
# <a name="app-expression-in-azure-monitor-query"></a>Azure İzleyici sorgu ifadesinde App()

`app` İfade, aynı kaynak grubunu, başka bir kaynak grubu veya başka bir aboneliğe belirli bir Application Insights uygulamasından veri almak için Azure İzleyici sorgusu kullanılır. Uygulama verileri bir Application Insights sorgu birden çok uygulama arasında bir Azure İzleyici günlük sorgu ve veri eklemek kullanışlıdır.



## <a name="syntax"></a>Sözdizimi

`app(`*tanımlayıcı*`)`


## <a name="arguments"></a>Bağımsız Değişkenler

- *tanımlayıcı*: Aşağıdaki tabloda biçimlerden birini kullanarak uygulamayı tanımlar.

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
* App() ifade şu anda arama sorgusu oluşturmak için Azure portalını kullanarak, desteklenmeyen bir [özel günlük araması uyarı kuralı](../platform/alerts-log.md)sürece bir Application Insights uygulaması için uyarı kuralı kaynağı olarak kullanılır.

## <a name="examples"></a>Örnekler

```Kusto
app("fabrikamapp").requests | count
```
```Kusto
app("AI-Prototype/Fabrikam/fabrikamapp").requests | count
```
```Kusto
app("b438b4f6-912a-46d5-9cb1-b44069212ab4").requests | count
```
```Kusto
app("/subscriptions/7293b69-db12-44fc-9a66-9c2005c3051d/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [çalışma ifade](workspace-expression.md) bir Log Analytics çalışma alanına başvurmak için.
- Nasıl çalıştıracağınızı okuyun [Azure İzleyici veri](../../azure-monitor/log-query/log-query-overview.md) depolanır.
- Erişim için tüm belgeler [Kusto sorgu dili](/azure/kusto/query/).
