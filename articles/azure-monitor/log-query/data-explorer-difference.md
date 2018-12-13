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
ms.topic: article
ms.date: 10/31/2018
ms.author: bwren
ms.openlocfilehash: 645750ec40f0aba2ef58c096a72125fad2947719
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53186268"
---
# <a name="log-analytics-query-language-differences"></a>Analytics sorgu dili farklar oturum

Sırada [Log Analytics](log-query-overview.md) üzerine kurulmuştur [Azure Veri Gezgini](/azure/data-explorer) ve kullandığı [aynı sorgu dilini](/azure/kusto/query), dil sürümü bazı farklılıkları vardır. Bu makalede, Veri Gezgini için kullanılan dil sürümünü ve Log Analytics sorguları için kullanılan sürümü arasındaki farkları öğeleri tanımlar.

## <a name="data-explorer-elements-not-supported-in-log-analytics"></a>Veri Gezgini elemanlarını log Analytics'te desteklenmiyor
Aşağıdaki bölümlerde, Log Analytics tarafından desteklenmeyen veri Gezgini sorgu dilinin öğelerini açıklar.

### <a name="statements-not-supported-in-log-analytics"></a>Log Analytics'te desteklenmeyen ifadeler

* [Diğer ad](/azure/kusto/query/aliasstatement)
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

- Başvurular farklı alma [Log Analytics sorguları yazma kaynakları](query-language.md).
- Tam erişim [başvuru belgeleri için Veri Gezgini'ni sorgu dili](/azure/kusto/query/).
