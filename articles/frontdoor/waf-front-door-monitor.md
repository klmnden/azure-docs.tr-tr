---
title: İzleme ve günlüğe kaydetme azure web uygulaması güvenlik duvarı
description: Web uygulaması Güvenlik Duvarı (WAF) ile izleme ve günlüğe kaydetme FrontDoor öğrenin
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: tyao;kumud
ms.openlocfilehash: 5368ed8d1e60a646366065e2cf617fb2f3735b53
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61459768"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>İzleme ve günlüğe kaydetme azure web uygulaması güvenlik duvarı 

Azure web uygulaması Güvenlik Duvarı (WAF) izleme ve günlüğe kaydetme ile günlüğe kaydetme ve Azure İzleyici ve Azure İzleyici ile tümleştirmesi günlükleri sağlanır.

> [!IMPORTANT]
> WAF izleme ve Azure ön kapısı özellik günlüğe kaydetme, şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="azure-monitor"></a>Azure İzleyici

WAF FrontDoor günlüğü ile ile tümleşiktir [Azure İzleyici](../azure-monitor/overview.md). Azure İzleyici, WAF uyarıları ve günlükleri de dahil olmak üzere tanılama bilgilerini izlemenize olanak tanır. WAF portalında altında ön kapısı kaynaktaki izleme yapılandırabilirsiniz **tanılama** sekmesinde veya Azure İzleyici hizmetinde doğrudan.

Azure portalından ön kapısı kaynak türü için gidin. Gelen **izleme**/**ölçümleri** solda, ekleyebilmeniz için sekmesinde **WebApplicationFirewallRequestCount** WAF kurallarını eşleşen istek sayısını izlemek için. Özel Filtreler eylem türleri ve kuralı adları göre oluşturulabilir.

![WAFMetrics](./media//waf-front-door-monitor/waf-frontdoor-metrics.png)

## <a name="logs-and-diagnostics"></a>Günlükleri ve tanılama

Ön kapısı WAF, algıladığı her tehdit ayrıntılı raporlama sağlar. Günlük kaydı Azure Tanılama günlükleri ile tümleştirilir ve uyarılar json biçiminde kaydedilir. Bu günlükleri ile tümleştirilebilir [Azure İzleyicisi](../azure-monitor/insights/azure-networking-analytics.md).

![WAFDiag](./media/waf-front-door-monitor/waf-frontdoor-diagnostics.png)

FrontdoorAccessLog müşteri arka uçları iletilen tüm istekleri günlüğe kaydeder. FrontdoorWebApplicationFirewallLog WAF kuralıyla eşleşen bir istek günlüğe kaydeder.

Aşağıdaki örnek sorgu engellenen isteklerde WAF günlükleri alır:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"

```

Aşağıdaki örnek sorgu AccessLogs girişleri alır:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"


```

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [ön kapısı](front-door-overview.md).

