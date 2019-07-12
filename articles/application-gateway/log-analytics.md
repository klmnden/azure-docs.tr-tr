---
title: Application Gateway Web uygulaması güvenlik duvarı günlüklerini incelemek için Azure Log Analytics'i kullanma
description: Bu makalede, Azure Log Analytics Application Gateway Web uygulaması güvenlik duvarı günlüklerini incelemek için nasıl kullanabileceğinizi gösterir.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 7/10/2019
ms.author: victorh
ms.openlocfilehash: aa867e33ef0faa96b6a66a9075a3a5b8b0b0bca4
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67712182"
---
# <a name="use-log-analytics-to-examine-application-gateway-web-application-firewall-logs"></a>Application Gateway Web uygulaması güvenlik duvarı günlüklerini incelemek için log Analytics'i kullanma

Application Gateway WAF, operasyonel hale geldikten sonra her bir istekle neler olduğunu denetlemek günlükleri etkinleştirebilirsiniz. Güvenlik duvarı günlükleri verin Insight hangi WAF için değerlendirme, eşleşen engelleme ve. Log Analytics ile hatta daha fazla öngörü vermek için güvenlik duvarı günlükleri içindeki verileri inceleyebilirsiniz. Bir Log Analytics çalışma alanı oluşturma hakkında daha fazla bilgi için bkz. [Azure portalında Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md). Günlük sorguları hakkında daha fazla bilgi için bkz: [günlüğüne genel bakış, Azure İzleyicisi'nde sorgular](../azure-monitor/log-query/log-query-overview.md).

## <a name="import-waf-logs"></a>WAF günlükleri içeri aktarma

Log Analytics'e, güvenlik duvarı günlükleri almak için bkz [arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway](application-gateway-diagnostics.md#diagnostic-logging). Güvenlik duvarı günlükleri Log Analytics çalışma alanınızda varsa, verileri görüntülemek, sorguları yazma, görselleştirmeler oluşturun ve bunları portal panonuza ekleyebilirsiniz.

## <a name="explore-data-with-examples"></a>Örnek verilerle keşfedin

Güvenlik Duvarı günlüğünde ham verileri görüntülemek için şu sorguyu çalıştırabilirsiniz:

```
AzureDiagnostics 
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
```

Bu, aşağıdaki sorguyu benzer görünecektir:

![Log Analytics sorgusu](media/log-analytics/log-query.png)

Verileri, detaya gitme ve grafik çizim veya buradan görselleştirmeler oluşturun. Aşağıdaki sorgularda, başlangıç noktası olarak bakın:

### <a name="matchedblocked-requests-by-ip"></a>IP eşleşen bloke istekleri

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by clientIp_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="matchedblocked-requests-by-uri"></a>URI ile eşleşen bloke istekleri

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by requestUri_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="top-matched-rules"></a>En iyi eşleşen kuralları

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by ruleId_s, bin(TimeGenerated, 1m)
| where count_ > 10
| render timechart
```

### <a name="top-five-matched-rule-groups"></a>İlk beş eşleşen kural grubu

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize Count=count() by details_file_s, action_s
| top 5 by Count desc
| render piechart
```

## <a name="add-to-your-dashboard"></a>Panonuza ekleme

Bir sorgu oluşturduğunuzda, bunu panonuza ekleyebilirsiniz.  Seçin **panoya Sabitle** üst sağında log analytics çalışma alanı. Önceki dört sorguları bir örnek panoya sabitlenmiş bu verileri bir bakışta görebilirsiniz.

![Pano](media/log-analytics/dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

[Arka uç sistem durumu, tanılama günlükleri ve ölçümler için Application Gateway](application-gateway-diagnostics.md)
