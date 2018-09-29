---
title: Azure İzleyici Log Analytics dil başvurusu | Microsoft Docs
description: Log Analytics tarafından kullanılan Kusto dili için başvuru bilgileri. Log Analytics'e özgü ek öğeler ve Log Analytics sorguları desteklenmiyor öğeleri içerir.
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
ms.date: 09/25/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 5173790436a29fa9947346d711da1a2ddb32bf62
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451077"
---
# <a name="log-analytics-query-language-reference"></a>Günlük analizi sorgu dili başvurusu
[Analiz sorguları oturum](../log-analytics-queries.md) altyapısı tarafından kullanılan ve aynı sorgu dilini kullanın [Azure Veri Gezgini](/azure/data-explorer/). Dil Başvurusu ve diğer ayrıntıları dili hakkında aşağıdaki konumdan erişebilirsiniz: [Kusto dil başvurusu](/azure/kusto/query)



## <a name="kusto-elements-not-support-in-log-analytics"></a>Kusto öğeleri Log Analytics'te desteklemiyor.
Log Analytics sorguları Kusto uygulaması kullanırken, aşağıdaki bölümlerde açıklandığı gibi desteklemiyor bazı Kusto öğeler vardır.

### <a name="statements-not-supported-in-log-analytics"></a>Log Analytics'te desteklenmeyen ifadeler

* [Diğer ad](/kusto/query/aliasstatement)
* [Sorgu parametreleri](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-log-analytics"></a>Log Analytics'te desteklenmeyen işlevleri

* [Cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [Database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-log-analytics"></a>Log Analytics'te desteklenmeyen işleçleri

* [Çapraz-küme birleştirme](/azure/kusto/query/joincrosscluster)
* [externaldata işleci](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-log-analytics"></a>Log Analytics'te desteklenmeyen eklentileri

* [sql_request eklentisi](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-log-analytics"></a>Log analytics'te ek işleçleri
Belirli bir Log Analytics özellikleri desteklemek için aşağıdaki ek Kusto işleçleri sağlanır, Log Analytics dışında kullanılamaz. 

* [App()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Sonraki adımlar

- İçindeki sorgular hakkında okuyun [Log Analytics](../log-analytics-queries.md).
- Ders yazmak üzerinde size yol gösteren bir [Log Analytics sorgusu](/log-analytics/query-language/get-started-queries.md).
- Tam erişim [başvuru belgeleri için Kusto](/azure/kusto/query/).