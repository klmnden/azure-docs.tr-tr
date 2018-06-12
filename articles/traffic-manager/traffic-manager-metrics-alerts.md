---
title: Ölçümleri ve Uyarıları Azure trafik Yöneticisi'nde | Microsoft Docs
description: Bu makalede Azure trafik Yöneticisi için kullanılabilir ölçümleri açıklanmaktadır.
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: kumud
ms.openlocfilehash: 424782be2d814df6d598591198b5005fb494d3da
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35304208"
---
# <a name="traffic-manager-metrics-and-alerts"></a>Trafik Yöneticisi ölçümleri ve uyarılar

Trafik Yöneticisi DNS tabanlı Yük Dengeleme, birden çok yönlendirme yöntemleri ve uç nokta izleme seçenekleri içerir sağlar. Bu makalede ölçümleri ve müşterilerin kullanımına ilişkili uyarı açıklanmaktadır. 

## <a name="metrics-available-in-traffic-manager"></a>Trafik Yöneticisi'nde kullanılabilir ölçümler 

Trafik Yöneticisi uç noktaları bu profili altında durumunu ve trafik Yöneticisi kendi kullanımını anlamak için müşteriler tarafından kullanılabilecek bir profil başına aşağıdaki ölçümleri sağlar.  

### <a name="queries-by-endpoint-returned"></a>Sorgular tarafından döndürülen uç noktası
Kullanım [Bu ölçüm](../monitoring-and-diagnostics/monitoring-supported-metrics.md) trafik Yöneticisi profili tarafından belirtilen bir süredeki işlenmiş olan sorgu sayısını görüntülemek için. Birçok kez bir uç nokta nasıl anlamanıza yardımcı olur, sorgu yanıtlarında trafik Yöneticisi'nden döndürülen bir uç nokta düzeyi ayrıntı düzeyi, aynı bilgileri de görüntüleyebilirsiniz.

Aşağıdaki örnekte, Şekil 1 trafik Yöneticisi profili tarafından döndürülen tüm sorgu yanıtları görüntüler. 

  
![Trafik Yöneticisi ölçümleri - tüm sorgular Birleşik görünümü](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Şekil 1: tüm sorguları görünümüyle toplama*
  
Şekil 2 aynı bilgiyi görüntüler, ancak uç noktaları tarafından ayrılır. Sonuç olarak, belirli bir uç döndürülen sorgu yanıtları hacmi görebilirsiniz.

![Trafik Yöneticisi ölçümleri - sorgu birim uç nokta başına görünümünü bölme](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Şekil 2: Bölünmüş görünümlü döndürülen uç nokta gösterilen sorgu birimle*

## <a name="endpoint-status-by-endpoint"></a>Bitiş noktası tarafından uç nokta durumu
Kullanım [Bu ölçüm](../monitoring-and-diagnostics/monitoring-supported-metrics.md) profil uç noktalardan sistem durumunu anlamak için. İki değerleri alır:
 - kullanmak **1** , uç nokta yukarı ise.
 - kullanmak **0** uç noktası kapalı olduğunda.

Bu ölçüm tüm ölçümleri (Şekil 3) durumunu temsil eden bir birleşik değer olarak ya da gösterilebilir ya da (bkz: Şekil 4) belirli Uç noktalara durumunu göstermek için bölünebilir. Eski olarak toplama düzeyi seçtiyseniz, söz konusu olduğunda **Avg**, bu değeri Ölçüm, tüm uç noktaları durumunu aritmetik ortalama. Örneğin, iki uç nokta bir profil varsa ve yalnızca bir sağlam, bu ölçüm bir değeri olur **0,50** Şekil 3'te gösterildiği gibi. 


![Trafik Yöneticisi ölçümleri - uç nokta durumu bileşik görünümü](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Şekil 3: Bileşik uç nokta durumu ölçüm – seçili "Ortalama" toplama görünümünü*


![Trafik Yöneticisi ölçümleri - uç nokta durumu görünümünü bölme](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Şekil 4: Bölünmüş görünümlü uç nokta durumu ölçümleri*

Bu ölçümler aracılığıyla tüketebileceği [Azure Monitor Hizmeti](../monitoring-and-diagnostics/monitoring-supported-metrics.md)'s portal, [REST API](https://docs.microsoft.com/rest/api/monitor/), [Azure CLI](https://docs.microsoft.com/cli/azure/monitor), ve [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights), veya Trafik Yöneticisi'nin portal deneyimi ölçümleri bölümü.

## <a name="alerts-on-traffic-manager-metrics"></a>Trafik Yöneticisi ölçümleri uyarılar
İşleme ve ölçümleri trafik Yöneticisi'nden görüntüleme ek olarak, Azure İzleyici yapılandırmak ve bu ölçümleri ile ilişkili uyarı almak müşterilerin sağlar. Bu ölçümler gerçekleşmesi bir uyarı için karşılanması gereken koşulları gerekenler, ne sıklıkta izlenmesi Bu koşullar gerekir ve nasıl uyarılar size gönderilmesi gerektiğini seçebilirsiniz. Daha fazla bilgi için bkz: [Azure İzleyici uyarıları belgelerine](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [Azure Monitor Hizmeti](../monitoring-and-diagnostics/monitoring-supported-metrics.md)
- Bilgi edinmek için nasıl [Azure İzleyicisi'ni kullanarak yeni bir grafik oluşturma](../monitoring-and-diagnostics/monitoring-metric-charts.md#how-do-i-create-a-new-chart)