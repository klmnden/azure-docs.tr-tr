---
title: Azure İzleyici günlük sorguları | Microsoft Docs
description: Azure İzleyici'de günlük sorguları yazmayı öğrenmek için kaynaklara başvurular.
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
ms.date: 01/11/2019
ms.author: bwren
ms.openlocfilehash: 6fae245e4d72f7f6f7809d85da17023deb1518e5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65560843"
---
# <a name="azure-monitor-log-queries"></a>Azure İzleyici günlük sorguları
Azure İzleyici günlüklerine Azure Veri Gezgini oluşturulur ve Azure İzleyici günlük sorguları aynı Kusto sorgu dil sürümü kullanın. [Kusto sorgu dili belgeleri](/azure/kusto/query) tüm dil ayrıntılarını sahiptir ve Azure İzleyici günlük sorguları yazmak için birincil kaynak olmalıdır. Bu sayfa, sorgu yazmayı öğrenmek ve farkları dilinin Azure İzleyici uygulaması ile diğer kaynakların bağlantılarını sağlar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="getting-started"></a>Başlarken

- [Azure İzleyici Log Analytics ile çalışmaya başlama](get-started-portal.md) sorguları yazma ve Azure portalında sonuçları ile çalışmak için bir Ders şu oldu.
- [Azure İzleyici günlük sorguları kullanmaya başlama](get-started-queries.md) Azure İzleyici günlük verilerini kullanarak sorgular yazmak için bir Ders şu oldu.

## <a name="concepts"></a>Kavramlar
- [Azure İzleyici'de günlük verilerini analiz etmek](../../azure-monitor/log-query/log-query-overview.md) verir günlük kısa bir genel bakış sorgular ve Azure İzleyici günlük verilerini nasıl yapılandırıldığını açıklar.
- [Azure İzleyici'de günlük verilerini çözümleme ve görüntüleme](../../azure-monitor/log-query/portals.md) oluşturduğunuz ve günlük sorguları çalıştırma portallar açıklamaktadır.

## <a name="reference"></a>Başvuru

- [Sorgu dili başvurusu](/azure/kusto/query) Kusto sorgu dili için tam dil başvurudur.
- [Azure İzleyici günlük sorgu dili farklılıkları](data-explorer-difference.md) Kusto sorgu dil sürümü arasındaki farkları açıklar.
- [Azure İzleyici'de standart özellikler günlük kayıtlarının](../../azure-monitor/platform/log-standard-properties.md) tüm Azure İzleyici günlük verileri için standart özellikler açıklanmaktadır.
- [Azure İzleyici'de kaynaklar arası günlük sorguları gerçekleştirmek](../../azure-monitor/log-query/cross-workspace-query.md) verileri birden fazla Log Analytics çalışma alanları ve Application Insights uygulamaları kullanarak günlük sorguları yazma işlemini açıklamaktadır.


## <a name="examples"></a>Örnekler

- [Azure İzleyici günlük sorgu örnekleri](examples.md) Azure İzleyici günlük verilerini kullanarak örnek sorguları sağlar.



## <a name="lessons"></a>Dersler

- [Azure İzleyici günlük sorguları dizelerle çalışma](string-operations.md) dize verileri ile çalışma açıklar.
- [Azure İzleyici günlük sorguları tarih saat değerleri ile çalışma](datetime-operations.md) tarih ve saat verileri ile çalışma açıklar. 
- [Azure İzleyici toplamaları sorguları oturum](aggregations.md) ve [Azure İzleyici günlük sorguları toplamalara Gelişmiş](advanced-aggregations.md) toplamak ve verilerini özetle açıklar.
- [Azure izleyici günlüğü sorgularda birleşimler](joins.md) birden çok tablodan veri birleştirme işlemini açıklamaktadır.
- [JSON ve veri yapıları Azure izleyici günlüğü sorgularda çalışma](json-data-structures.md) json verileri ayrıştırmak açıklar.
- [Gelişmiş yazma, sorgular Azure İzleyici'de oturum](advanced-query-writing.md) karmaşık sorgular oluşturma ve kod yeniden stratejilerini açıklar.
- [Azure izleyici günlüğü sorgularından grafikleri ve diyagramları oluşturma](charts.md) günlük sorgusu verilerini görselleştirmek açıklar.

## <a name="cheatsheets"></a>Başvuru sayfaları

-  [Azure İzleyici günlük sorgusu SQL](sql-cheatsheet.md) SQL ile ilgili bilgi sahibi olan kullanıcılara yardımcı olur.
-  [Azure İzleyici günlük sorgusu için Splunk](splunk-cheatsheet.md) Splunk ile ilgili bilgi sahibi olan kullanıcılara yardımcı olur.
 
## <a name="next-steps"></a>Sonraki adımlar

- Tam erişim [başvuru belgeleri Kusto sorgu dili için](/azure/kusto/query/).
