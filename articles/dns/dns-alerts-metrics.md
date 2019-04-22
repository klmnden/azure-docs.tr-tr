---
title: Azure DNS ölçümleri ve Uyarıları | Microsoft Docs
description: Azure DNS ölçümleri ve Uyarıları hakkında bilgi edinin.
services: dns
documentationcenter: na
author: vhorne
manager: jennoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2018
ms.author: victorh
ms.openlocfilehash: baa2a09adeba133c5348449b12e037d4a9cb3213
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59683044"
---
# <a name="azure-dns-metrics-and-alerts"></a>Azure DNS ölçümleri ve Uyarıları
Azure DNS, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlayan bir barındırma DNS etki alanları için hizmetidir. Bu makalede, Ölçümler ve Uyarılar için Azure DNS hizmeti açıklanır.

## <a name="azure-dns-metrics"></a>Azure DNS ölçümleri

Azure DNS, bunları hizmette barındırılan kendi DNS bölgelerinin belirli işlevlerinin izlemek etkinleştirmek, müşteriler için ölçümleri sağlar. Ayrıca, Azure DNS ölçümlerindeki ile yapılandırın ve ilgi koşullara göre uyarı alacak. Ölçümleri aracılığıyla sağlanan [Azure İzleyici hizmeti](../azure-monitor/index.yml). Azure DNS, DNS bölgeleriniz için Azure İzleyici aracılığıyla aşağıdaki ölçümleri sunar:

-   QueryVolume
-   RecordSetCount
-   RecordSetCapacityUtilization

Ayrıca bkz [Bu ölçümler tanımını](../azure-monitor/platform/metrics-supported.md#microsoftnetworkdnszones) Azure İzleyici belgeleri sayfasında.
>[!NOTE]
> Şu anda bu ölçümler yalnızca Azure DNS'de barındırılan bir ortak DNS bölgeleri için kullanılabilir. Azure DNS'de barındırılan özel bölgeleri varsa, bu ölçümleri veri bölgeleri için sağlamaz. Ayrıca, Ölçümler ve uyarı özelliği yalnızca desteklenir Azure genel bulutunda. Bağımsız Bulutlar için destek, daha sonraki bir zamanda izler. 

Bu ölçümler için boyut parçalı düzeyde DNS bölgesidir.

### <a name="query-volume"></a>Sorgu birim

*Sorgu toplu* ölçümü Azure DNS'de DNS bölgenizi Azure DNS tarafından alınan DNS sorguları (sorgu trafiği) hacmini gösterir. Ölçü birimi sayı ve toplama işlemini bir süre alınan tüm sorguların toplamıdır. 

Bu ölçüm görüntülemek için Azure portalında izleme sekmesi Gezgini deneyimi ölçümler (Önizleme) seçin. DNS bölgenizi kaynak açılan listeden seçin, sorgu toplu ölçümü seçin ve Sum toplama seçin. Aşağıdaki ekran görüntüsünde, bir örnek gösterilmektedir.  Ölçüm Gezgini hakkında daha fazla bilgi için deneyimi ve grafik, bkz: [Azure İzleyici ölçüm Gezgini'ni](../azure-monitor/platform/metrics-charts.md).

![Sorgu birim](./media/dns-alerts-metrics/dns-metrics-query-volume.png)

*Şekil: Azure DNS sorgu toplu ölçümleri*

### <a name="record-set-count"></a>Kayıt kümesi sayısı
*Kayıt kümesi sayısı* ölçüm için DNS bölgenizi Azure DNS'ye kayıt kümeleri sayısını gösterir. Tüm kayıt kümelerini diliminizi tanımlı sayılır. Ölçü birimi sayısı ve toplama tüm kayıt kümelerini sayısı. Bu ölçüm görüntülemek için seçin **ölçümler (Önizleme)** Gezgini deneyiminden **İzleyici** Azure portalında sekmesi. DNS bölgenizi seçin **kaynak** açılan listesinde, select **kayıt kümesi sayısı** ölçüm ve ardından **Max** olarak **toplama** . Ölçüm Gezgini hakkında daha fazla bilgi için deneyimi ve grafik, bkz: [Azure İzleyici ölçüm Gezgini'ni](../azure-monitor/platform/metrics-charts.md). 

![Kayıt kümesi sayısı](./media/dns-alerts-metrics/dns-metrics-record-set-count.png)

*Şekil: Azure DNS kayıt kümesi sayısı ölçümleri*


### <a name="record-set-capacity-utilization"></a>Kayıt kümesi kapasite kullanımı
*Kayıt kümesi kapasite kullanımı* ölçümü Azure DNS'de bir DNS bölgesi için kayıt kümesi kapasite kullanımı yüzdesini gösterir. Her DNS bölgesinin Azure DNS'de bölge için izin verilen kayıt kümeleri maksimum sayısını tanımlayan bir kayıt kümesi sınırı tabidir (bkz [DNS sınırları](dns-zones-records.md#limits)). Bu nedenle, bu ölçüm, ne kadar yakın, kayıt sınırına ulaşması için gösterilir. Örneğin, DNS bölgeniz için yapılandırılmış 500 kayıt kümeleri vardır ve bölge varsayılan kayıt kümesi sınırı 5000 RecordSetCapacityUtilization ölçüm 10 (bölme 500 tarafından 5000 tarafından alınır) % değerini gösterir. Ölçü birimi **yüzdesi** ve **toplama** türü **maksimum**. Bu ölçüm görüntülemek için Azure portalında izleme sekmesi Gezgini deneyimi ölçümler (Önizleme) seçin. DNS bölgenizi kaynak açılan listeden seçin, kayıt kümesi kapasite kullanımı ölçümü seçin ve Max toplama seçin. Aşağıdaki ekran görüntüsünde, bir örnek gösterilmektedir. Ölçüm Gezgini hakkında daha fazla bilgi için deneyimi ve grafik, bkz: [Azure İzleyici ölçüm Gezgini'ni](../azure-monitor/platform/metrics-charts.md). 

![Kayıt kümesi sayısı](./media/dns-alerts-metrics/dns-metrics-record-set-capacity-uitlization.png)

*Şekil: Azure DNS kayıt kümesi kapasite kullanım ölçümleri*

## <a name="alerts-in-azure-dns"></a>Azure DNS içindeki uyarılar
Azure İzleyici uyarı kullanılabilir ölçüm değerleri karşı yeteneği sağlar. DNS ölçümleri yeni uyarı yapılandırma deneyiminde kullanılabilir. Ayrıntılı olarak açıklandığı gibi [Azure İzleyici uyarılarına belgeleri](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md), DNS bölgesi kaynağı seçin, ölçüm sinyal türü seçin ve uyarı mantığının ve diğer parametreleri gibi yapılandırma **süresi**ve **sıklığı**. Daha fazla tanımlayabilirsiniz bir [eylem grubu](../azure-monitor/platform/action-groups.md) uyarı seçtiğiniz eylemleri teslim edilecek gerçekleştirilmesine için ne zaman Uyarı koşulu, karşılanır. Azure İzleyici ölçümleri için uyarı yapılandırma hakkında daha fazla bilgi için bkz. [oluşturun, görüntüleyin ve Azure İzleyicisi'ni kullanarak Uyarıları yönetme](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). 

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [Azure DNS](dns-overview.md).
