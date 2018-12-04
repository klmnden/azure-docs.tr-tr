---
title: Azure Log Analytics sorgu ifadesinde Workspace() | Microsoft Docs
description: Çalışma alanı ifade bir Log Analytics sorgu, aynı kaynak grubunu, başka bir kaynak grubu veya başka bir aboneliğe belirli bir çalışma alanını veri almak için kullanılır.
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
ms.date: 09/10/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 2909f586ce102b319ca9dfeea191c7812e3f6b06
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842671"
---
# <a name="workspace-expression-in-log-analytics-query"></a>Log Analytics sorgu ifadesinde Workspace()

`workspace` İfade, aynı kaynak grubunu, başka bir kaynak grubu veya başka bir aboneliğe belirli bir çalışma alanını veri almak için bir Log Analytics sorgu kullanılır. Günlük verileri log sorguda birden çok çalışma alanında bir Application Insights sorgu ve veri eklemek kullanışlıdır.


## <a name="syntax"></a>Sözdizimi

`workspace(`*tanımlayıcı*`)`

## <a name="arguments"></a>Bağımsız Değişkenler

- *Tanımlayıcı*: Aşağıdaki tabloda biçimlerden birini kullanarak çalışma alanını tanımlar.

| Tanımlayıcı | Açıklama | Örnek
|:---|:---|:---|
| Kaynak Adı | İnsan tarafından okunabilir çalışma alanının adını (AKA "bileşen adı") | Workspace("contosoretail") |
| Tam adı | Formunda çalışma alanının tam adı: "resourceGroup/subscriptionName/componentName" | Workspace('Contoso/ContosoResource/ContosoWorkspace') |
| Kimlik | Çalışma alanının GUID | Workspace("b438b3f6-912a-46d5-9db1-b42069242ab4") |
| Azure kaynak kimliği | Azure kaynak tanımlayıcısı | Workspace("/Subscriptions/e4227-645-44e-9c67-3b84b5982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/Workspaces/contosoretail") |


## <a name="notes"></a>Notlar

* Çalışma alanına okuma erişimi olmalıdır.
* İlgili bir ifadedir `app` Application Insights uygulamalar arasında sorgu olanak tanır.

## <a name="examples"></a>Örnekler

```Kusto
workspace("contosoretail").Update | count
```
```Kusto
workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count
```
```Kusto
workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
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

- Bkz: [uygulama ifade](workspace-expression.md) Application Insights uygulamaya başvurmak için.
- Nasıl çalıştıracağınızı okuyun [Log Analytics verilerini](../../azure-monitor/log-query/log-query-overview.md) depolanır.