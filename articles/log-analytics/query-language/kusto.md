---
title: Azure İzleyici Log Analytics ve Kusto dil farkları | Microsoft Docs
description: Log Analytics sorgularını ve çekirdek Kusto dil arasındaki farklar açıklanmaktadır.
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
ms.openlocfilehash: 109ffa6abb34dad6a00210a5c2c726bdfdde094f
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47184917"
---
# <a name="log-analytics-and-kusto-language-differences"></a>Log Analytics ve Kusto dil farkları
[Analiz sorguları oturum](../log-analytics-queries.md) ile yazılmış [Kusto dil](/azure/kusto/query). Ancak bu makalede anlatıldığı gibi standart dili ve Log Analytics uygulaması arasındaki bazı farklar vardır.


## <a name="statements-not-supported-in-log-analytics"></a>Log Analytics'te desteklenmeyen ifadeler
Log Analytics'te, aşağıdaki deyimleri desteklenmiyor.

* [Diğer ad](/kusto/query/aliasstatement)
* [Sorgu parametreleri](/azure/kusto/query/queryparametersstatement)

## <a name="functions-not-supported-in-log-analytics"></a>Log Analytics'te desteklenmeyen işlevleri
Log Analytics'te aşağıdaki işlevleri desteklenmez.

* [Cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [Database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

## <a name="operators-not-supported-in-log-analytics"></a>Log Analytics'te desteklenmeyen işleçleri
Log Analytics'te aşağıdaki işleçleri desteklenmez.

* [Çapraz-küme birleştirme](/azure/kusto/query/joincrosscluster)
* [externaldata işleci](/azure/kusto/query/externaldata-operator)

## <a name="plugins-not-supported-in-log-analytics"></a>Log Analytics'te desteklenmeyen eklentileri
Aşağıdaki eklentiler, Log Analytics'te desteklenmez.
* [sql_request eklentisi](/azure/kusto/query/sqlrequestplugin)


## <a name="log-analytics-specific-operators"></a>Log Analytics belirli işleçleri
* [App()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Sonraki adımlar

- İçindeki sorgular hakkında okuyun [Log Analytics](../log-analytics-queries.md).
- Ders yazmak üzerinde size yol gösteren bir [Log Analytics sorgusu](/log-analytics/query-language/get-started-queries.md).
- Tam erişim [başvuru belgeleri için Kusto](/azure/kusto/query/).