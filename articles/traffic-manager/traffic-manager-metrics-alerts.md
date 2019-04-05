---
title: Ölçümler ve uyarılar Azure Traffic Manager'da
description: Bu makalede, Azure Traffic Manager için mevcut olan ölçümler açıklanır.
services: traffic-manager
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: kumud
ms.openlocfilehash: c251cc851b34f708a2150d3b0444f235d2bc50d6
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045313"
---
# <a name="traffic-manager-metrics-and-alerts"></a>Traffic Manager ölçümleri ve Uyarıları

Traffic Manager DNS tabanlı yük birden fazla yönlendirme yöntemleri ve uç nokta izleme seçenekleri içeren Dengeleme ile sunar. Bu makalede, Ölçümler ve müşterileri tarafından kullanılabilir ilişkili uyarıları açıklanır. 

## <a name="metrics-available-in-traffic-manager"></a>Trafik Yöneticisi'nde mevcut olan ölçümler 

Traffic Manager profili başına temelinde müşteriler kullanımlarını Traffic manager'ın ve uç noktaları altında bu profili durumunu anlamak için kullanabileceğiniz aşağıdaki ölçümleri sunar.  

### <a name="queries-by-endpoint-returned"></a>Sorgu tarafından döndürülen uç noktası
Kullanım [Bu ölçüm](../azure-monitor/platform/metrics-supported.md) belirtilen bir süredeki bir Traffic Manager profili işler sorguların sayısını görüntülemek için. Birden çok kez bir uç nokta nasıl anlamanıza yardımcı, trafik Yöneticisi'nden sorgu yanıt döndürüldü bir uç nokta düzeyine ayrıntılı, aynı bilgileri de görüntüleyebilirsiniz.

Aşağıdaki örnekte, Şekil 1, Traffic Manager profilini döndüren sorgu yanıtlarını görüntüler. 

  
![Tüm sorguların toplam görüntüleme](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Şekil 1: Tüm sorguları birleşik görünüm*
  
Şekil 2 aynı bilgileri gösterir, ancak uç noktaları tarafından ayrılır. Sonuç olarak, birimin belirli bir uç noktaya döndürülen sorgu yanıtlarının görebilirsiniz.

![Traffic Manager ölçümleri - Bölünmüş Görünüm sorgu birimin uç noktası başına](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Şekil 2: Uç nokta başına döndürülen gösterilen sorgu birimle Bölünmüş Görünüm*

## <a name="endpoint-status-by-endpoint"></a>Uç nokta olarak uç nokta durumu
Kullanım [Bu ölçüm](../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) profilinde bir uç nokta sistem durumunu anlamak için. Bu iki değerleri alır:
 - kullanma **1** uç nokta çalışıyorsa.
 - kullanma **0** uç noktası kapalı olduğunda.

Bu ölçüm tüm ölçümler (Şekil 3) durumunu temsil eden bir toplam değer olarak gösterilemeyecek kadar veya bunu (bkz: Şekil 4) belirli Uç noktalara durumunu göstermek için bölünebilir. Eski, eğer olarak toplama düzeyinde seçtiyseniz **ortalama**, tüm uç noktalar durumunu aritmetik ortalamasını bu ölçüm değeri. Örneğin, iki uç nokta bir profili var ve yalnızca biri sağlıklı olup, bu ölçüm bir değeri vardır **0,50** Şekil 3'te gösterildiği gibi. 


![Traffic Manager ölçümleri - endpoint durumunun birleşik görünümünü](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Şekil 3: Uç nokta durumu ölçüm – seçili "Ortalama" toplama bileşik görünümü*


![Traffic Manager ölçümleri - Bölünmüş Görünüm uç nokta durumu](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Şekil 4: Uç nokta durum ölçümlerinin Bölünmüş Görünüm*

Bu ölçümleri aracılığıyla tüketebileceği [Azure İzleyici hizmeti](../azure-monitor/platform/metrics-supported.md)ın portal [REST API](https://docs.microsoft.com/rest/api/monitor/), [Azure CLI](https://docs.microsoft.com/cli/azure/monitor), ve [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.applicationinsights), veya Traffic Manager'ın portal deneyimi ölçümleri bölümü.

## <a name="alerts-on-traffic-manager-metrics"></a>Traffic Manager ölçümler ile ilgili uyarılar
Azure İzleyici, işleme ve trafik Yöneticisi'nden ölçümü görüntüleniyor ek olarak, yapılandırmak ve bu ölçümleri ile ilişkili uyarıları almak müşterilerin sağlar. Bu ölçümler gerçekleşmesi bir uyarı için karşılanması gereken koşullar gerekenler, ne sıklıkta bu koşullara izlenmesi gereken ve nasıl uyarılar size gönderilmesi gereken seçebilirsiniz. Daha fazla bilgi için [Azure İzleyici uyarılarına belgeleri](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Azure İzleyici hizmeti](../azure-monitor/platform/metrics-supported.md)
- Bilgi edinmek için nasıl [Azure İzleyicisi'ni kullanarak bir grafik oluşturma](../azure-monitor/platform/metrics-charts.md#create-a-new-chart)
