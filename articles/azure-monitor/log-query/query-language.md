---
title: Azure İzleyici Log Analytics sorgu diline | Microsoft Docs
description: Log Analytics'te sorgu yazmayı öğrenmek için kaynaklara başvurular.
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
ms.date: 10/29/2018
ms.author: bwren
ms.openlocfilehash: 32e64ce7772d562ea34a0d74afbd737be27d247d
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968877"
---
# <a name="log-analytics-query-language"></a>Analytics sorgu dili oturum
Log Analytics, Azure İzleyici'için günlük toplama ve analiz sağlar. Azure Veri Gezgini yerleşik olarak bulunur ve aynı sorgu dil sürümünü kullanır. [Azure Veri Gezgini sorgu dili belgeleri](/azure/kusto/query) tüm dil ayrıntılarını sahiptir ve Log Analytics sorguları yazma, birincil kaynak olmalıdır. Bu sayfa, sorgu yazmayı öğrenmek ve farkları dilinin Log Analytics uygulaması ile diğer kaynakların bağlantılarını sağlar.

## <a name="getting-started"></a>Başlarken

- [Azure portalında log Analytics ile çalışmaya başlama](get-started-portal.md) sorguları yazma ve Azure portalında sonuçları ile çalışmak için bir Ders şu oldu.
-  [Log Analytics sorguları kullanmaya başlama](get-started-queries.md) Log Analytics verilerini kullanan sorgular yazmak için bir Ders şu oldu.

## <a name="concepts"></a>Kavramlar
- [Azure İzleyici'de log Analytics verilerini çözümleme](../../azure-monitor/log-query/log-query-overview.md) verir günlük kısa bir genel bakış sorgular ve Log Analytics verilerini nasıl yapılandırıldığını açıklar.
- [Log analytics'te verileri çözümleme ve görüntüleme](../../azure-monitor/log-query/portals.md) oluşturduğunuz ve Log Analytics sorguları çalıştırma portallar açıklamaktadır.

## <a name="reference"></a>Başvuru

- [Sorgu dili başvurusu](/azure/kusto/query) Veri Gezgini sorgu dili için tam dil başvurudur.
- [Analytics sorgu dili farklar oturum](data-explorer-difference.md) Veri Gezgini sorgu dil sürümü arasındaki farkları açıklar.
- [Log Analytics kayıtları standart özelliklerinde](../../azure-monitor/platform/log-standard-properties.md) tüm Log Analytics verilerini standart özellikleri açıklar.
- [Log Analytics'te günlük kaynaklar arası aramalar gerçekleştirmek](../../azure-monitor/log-query/cross-workspace-query.md) birden fazla Log Analytics çalışma alanları ve Application Insights uygulamalardan veri kullanan sorguları yazma işlemini açıklamaktadır.


## <a name="examples"></a>Örnekler

- [Analytics sorgu örnekleri oturum](examples.md) Log Analytics verilerini kullanan örnek sorgular sağlar.



## <a name="lessons"></a>Dersler

- [Log Analytics sorguları dizelerle çalışma](string-operations.md) dize verileri ile çalışma açıklar.
- [Log Analytics sorgu tarih saat değerleri ile çalışma](datetime-operations.md) tarih ve saat verileri ile çalışma açıklar. 
- [Log Analytics sorguları toplamalara](aggregations.md) ve [Log Analytics sorguları toplamalara Gelişmiş](advanced-aggregations.md) toplamak ve verilerini özetle açıklar.
- [Log Analytics sorguları birleşimler](joins.md) birden çok tablodan veri birleştirme işlemini açıklamaktadır.
- [JSON ve veri yapıları Log Analytics sorguları çalışma](json-data-structures.md) json verileri ayrıştırmak açıklar.
- [Gelişmiş Log Analytics sorguları yazma](advanced-query-writing.md) karmaşık sorgular oluşturma ve kod yeniden stratejilerini açıklar.
- [Log Analytics sorguları grafikleri ve diyagramları oluşturma](charts.md) sorgu verilerini görselleştirmek açıklar.

## <a name="cheatsheets"></a>Başvuru sayfaları

-  [Log Analytics sorgu dil kuralları sayfası SQL](sql-cheatsheet.md) SQL ile ilgili bilgi sahibi olan kullanıcılara yardımcı olur.
-  [Log Analytics sorgu dil kuralları sayfası için Splunk](sql-cheatsheet.md) Splunk ile ilgili bilgi sahibi olan kullanıcılara yardımcı olur.
 
## <a name="next-steps"></a>Sonraki adımlar

- Tam erişim [başvuru belgeleri için Veri Gezgini'ni sorgu dili](/azure/kusto/query/).