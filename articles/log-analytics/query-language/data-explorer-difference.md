---
title: Azure İzleyici Log Analytics dil başvurusu | Microsoft Docs
description: Log Analytics tarafından kullanılan veri Gezgini sorgu dili için başvuru bilgileri. Log Analytics'e özgü ek öğeler ve Log Analytics sorguları desteklenmiyor öğeleri içerir.
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
ms.date: 10/29/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: e6d097749dae49cf6f1d710bcf01cf99dcd98a4c
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244125"
---
# <a name="log-analytics-query-language-differences"></a>Analytics sorgu dili farklar oturum

Sırada [Log Analytics](../log-analytics-queries.md) üzerine kurulmuştur [Azure Veri Gezgini](/azure//data-explorer) ve kullandığı [aynı sorgu dilini](/azure/kusto/query), dil sürümü bazı farklılıkları vardır. Bu makalede, Veri Gezgini için kullanılan dil sürümünü ve Log Analytics sorguları için kullanılan sürümü arasındaki farkları öğeleri tanımlar.

## <a name="data-explorer-elements-not-supported-in-log-analytics"></a>Veri Gezgini elemanlarını log Analytics'te desteklenmiyor
Aşağıdaki bölümlerde, Log Analytics tarafından desteklenmeyen veri Gezgini sorgu dilinin öğelerini açıklar.

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
Aşağıdaki işleçleri belirli Log Analytics özellikleri destekler ve Log Analytics dışında kullanılamaz.

* [App()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Sonraki adımlar

<<<<<<< HEAD:articles/log-analytics/query-language/data-explorer-difference.md
- Başvurular farklı alma [Log Analytics sorguları yazma kaynakları](kusto.md).
- Tam erişim [başvuru belgeleri için Veri Gezgini'ni sorgu dili](/azure/kusto/query/).
=======
- İçindeki sorgular hakkında okuyun [Log Analytics](../log-analytics-queries.md).
- Ders yazmak üzerinde size yol gösteren bir [Log Analytics sorgusu](/log-analytics/query-language/get-started-queries.md).
- Tam erişim [başvuru belgeleri için Kusto](/azure/kusto/query/).
>>>>>>> 4bccab5ecb17c887658a4d2ed1bab6b22bf29ffd:articles/log-Analytics/Query-Language/kusto.MD
