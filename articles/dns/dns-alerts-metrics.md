---
title: Azure DNS ölçümleri ve Uyarıları | Microsoft Docs
description: Azure DNS ölçümleri ve uyarılar hakkında bilgi edinin.
services: dns
documentationcenter: na
author: KumudD
manager: jennoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2018
ms.author: kumud
ms.openlocfilehash: 54c4df446ee5c1bf8d29dd6c33b304f39ce8f1b8
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31591628"
---
# <a name="azure-dns-metrics-and-alerts"></a>Azure DNS ölçümleri ve uyarılar
Azure DNS ad çözümlemesi Microsoft Azure altyapı kullanarak sağlayan bir barındırma DNS etki alanı için hizmetidir. Bu makalede, ölçümleri ve Azure DNS hizmeti için uyarıları açıklanmaktadır.

## <a name="azure-dns-metrics"></a>Azure DNS ölçümleri

Azure DNS, müşterilerin kendi DNS hizmetinde barındırılan olanları belirli yönlerini izlemek bunları etkinleştirmek ölçümleri sağlar. Ayrıca, Azure DNS ölçümleri ile yapılandırın ve ilgi koşullara göre uyarılar alırsınız. Ölçümleri aracılığıyla sağlanan [Azure Monitor Hizmeti](../monitoring-and-diagnostics/index.yml). Azure DNS, DNS bölgeleri için Azure İzleyicisi aracılığıyla aşağıdaki ölçümleri sağlar:

-   QueryVolume
-   RecordSetCount
-   RecordSetCapacityUtilization

Ayrıca bkz [bu ölçümleri tanımını](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkdnszones) Azure İzleyicisi belgeleri sayfasında.
>[!NOTE]
> Şu anda bu ölçümleri yalnızca Azure DNS'de barındırılan ortak DNS bölgeleri için kullanılabilir. Özel Azure DNS'de barındırılan bölgeleri varsa, bu ölçümleri Bu bölgeler için veri sağlamaz. Ayrıca, ölçümleri ve uyarı verme özelliği yalnızca desteklenen Azure genel bulutunda. Destek sovereign Bulutlar için daha sonraki bir zamanda izler. 

Bu ölçümler için boyut parçalı düzeyde DNS bölgesidir.

### <a name="query-volume"></a>Sorgu birim

*Sorgu toplu* ölçüm Azure DNS'de DNS bölgenizi Azure DNS tarafından alınan DNS sorguları (sorgu trafiği) hacmini gösterir. Ölçü birimi sayısı ve toplama bir süre boyunca alınan tüm sorguların toplamı. Bu ölçüm görüntülemek için Azure portalında İzleyici sekmesi ölçümleri (Önizleme) explorer deneyimi seçin. Kaynak açılan listeden DNS bölgenizi seçin, sorgu birimi ölçümünü seçin ve toplam toplama seçin. Ekran görüntüsünün altında bir örneği gösterir.  Ölçüm Gezgini hakkında daha fazla bilgi için deneyimi ve grafik, bkz: [Azure İzleyici ölçüm Gezgini](../monitoring-and-diagnostics/monitoring-metric-charts.md).

![Sorgu birim](./media/dns-alerts-metrics/dns-metrics-query-volume.png)

*Şekil: Azure DNS sorgu toplu ölçümleri*

### <a name="record-set-count"></a>Kayıt kümesi sayısı
*Kayıt kümesi sayısı* ölçüm DNS bölgenizi Azure DNS'ye kayıt kümeleri sayısını gösterir. Kayıt kümeleri, bölge içinde tanımlanan tüm sayılır. Ölçü birimi sayısı ve toplama tüm kayıt kümelerinin sayısı. Bu ölçüm görüntülemek için seçin **ölçümleri (Önizleme)** explorer deneyimlerden **İzleyici** Azure portalında sekmesi. DNS bölgenizi seçin **kaynak** açılan listesinde, select **kayıt kümesi sayısı** ölçüm ve ardından **Max** olarak **toplama** . Ölçüm Gezgini hakkında daha fazla bilgi için deneyimi ve grafik, bkz: [Azure İzleyici ölçüm Gezgini](../monitoring-and-diagnostics/monitoring-metric-charts.md). 

![Kayıt kümesi sayısı](./media/dns-alerts-metrics/dns-metrics-record-set-count.png)

*Şekil: Azure DNS kayıt kümesi sayısı ölçümleri*


### <a name="record-set-capacity-utilization"></a>Kayıt kümesi kapasite kullanımı
*Kayıt kümesi kapasite kullanımı* ölçüm Azure DNS'de bir DNS bölgesi için kayıt kümesi kapasite kullanımını yüzdesini gösterir. Azure DNS'de her DNS bölgesinin bölge için izin verilen kayıt kümeleri en fazla sayısını tanımlar bir kayıt kümesi sınırı tabidir (bkz [DNS sınırları](dns-zones-records.md#limits)). Bu nedenle, bu ölçüm, ne kadar yakın, kayıt kümesi sınırına ulaşması için gösterilir. Örneğin, DNS bölgenizi için yapılandırılmış 500 kayıt kümeleri varsa ve bölge 5000 varsayılan kayıt kümesi sınırına sahip RecordSetCapacityUtilization ölçüm 10 (bölme 500 tarafından 5000 tarafından alınır) ' % değerini gösterir. Ölçü birimidir **yüzde** ve **toplama** türü **maksimum**. Bu ölçüm görüntülemek için Azure portalında İzleyici sekmesi ölçümleri (Önizleme) explorer deneyimi seçin. Kaynak açılan listeden DNS bölgenizi seçin, kayıt kümesi kapasite kullanımı ölçümü seçin ve Max toplama seçin. Ekran görüntüsünün altında bir örneği gösterir. Ölçüm Gezgini hakkında daha fazla bilgi için deneyimi ve grafik, bkz: [Azure İzleyici ölçüm Gezgini](../monitoring-and-diagnostics/monitoring-metric-charts.md). 

![Kayıt kümesi sayısı](./media/dns-alerts-metrics/dns-metrics-record-set-capacity-uitlization.png)

*Şekil: Azure DNS kayıt kümesi kapasite kullanımı ölçümleri*

## <a name="alerts-in-azure-dns"></a>Azure DNS'de uyarıları
Azure İzleyicisi uyarı kullanılabilir ölçüm değerleri karşı yeteneği sağlar. DNS ölçümleri yeni uyarı yapılandırma deneyimi kullanılabilir. Ayrıntılı olarak açıklandığı gibi [Azure İzleyici uyarıları belgelerine](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md), kaynak olarak DNS bölgesini seçin, ölçüm sinyal türü seçin ve uyarı mantığı ve diğer parametreleri gibi yapılandırmadan **süresi**ve **sıklığı**. Daha fazla tanımlayabilirsiniz bir [eylem grubu](../monitoring-and-diagnostics/monitoring-action-groups.md) uyarı seçtiğiniz Eylemler teslim edilecek aslına için ne zaman Uyarı koşulu, karşılanır. Azure İzleyici ölçümlerini uyarı yapılandırma hakkında daha fazla bilgi için bkz: [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). 

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [Azure DNS](dns-overview.md).
