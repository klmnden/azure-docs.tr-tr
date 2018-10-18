---
title: Azure ExpressRoute izleme, Ölçümler ve uyarılar | Microsoft Docs
description: Bu sayfa ExpressRoute izleme hakkında bilgi sağlar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: fcdaf10ad30cee0e7b79304ff21d67560522875c
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377806"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>ExpressRoute izleme, ölçümler ve uyarılar

 Bu makalede, ExpressRoute izleme, Ölçümler ve uyarılar anlamanıza yardımcı olur. Azure İzleyici, uyarıları, tüm Azure tanılama günlükleri sorunlarınız için tüm ölçümleri tek sağlanır.

## <a name="circuit-metrics"></a>Bağlantı hattı ölçümleri

Gidilecek **ölçümleri**, izlemek istediğiniz bağlantı hattı için ExpressRoute sayfasını tıklatın. Altında **izleme** görüntüleyebileceğiniz **ölçümleri**. BitsInPerSecond veya BitsOutPerSecond ve toplama'yı seçin. İsteğe bağlı olarak, eşleme türü başına ölçümler gösterecektir bölme uygulayabilirsiniz.

![Bağlantı hattı ölçümleri](./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg)

## <a name="metrics-per-peering"></a>Eşleme başına ölçümler

Özel, genel ve Microsoft bit/saniye içinde eşleme için ölçümleri görüntüleyebilir.

![Eşleme başına ölçümler](./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg) 

## <a name="expressroute-gateway-connections-in-bitsseconds"></a>ExpressRoute ağ geçidi bağlantıları bit/saniye

![Ağ Geçidi bağlantıları](./media/expressroute-monitoring-metrics-alerts/erconnections.jpg ) 

## <a name="alerts-for-expressroute-gateway-connections"></a>ExpressRoute ağ geçidi bağlantıları için uyarılar

1. Uyarıları yapılandırmak için gidin **Azure İzleyici**, ardından **uyarılar**.

  ![alerts](./media/expressroute-monitoring-metrics-alerts/eralertshowto.jpg)

2. Tıklayın **+ seçin hedef** ve ExpressRoute ağ geçidi bağlantı kaynağı seçin.

  ![hedef]( ./media/expressroute-monitoring-metrics-alerts/alerthowto2.jpg)
3. Uyarı ayrıntılarını tanımlayın.

  ![eylem grubu](./media/expressroute-monitoring-metrics-alerts/alerthowto3.jpg)


4. Tanımlamak ve eylem grubunu ekleyin.

  ![eylem grubu Ekle](./media/expressroute-monitoring-metrics-alerts/actiongroup.png)

## <a name="alerts-based-on-each-peering"></a>Her eşleme bağlı uyarılar

 ![ne](./media/expressroute-monitoring-metrics-alerts/basedpeering.jpg)

## <a name="configure-alerts-for-activity-logs-on-circuits"></a>Bağlantı hatları üzerinde etkinlik günlükleri için uyarıları yapılandırın

İçinde **Uyarı ölçütleri**, seçebileceğiniz **etkinlik günlüğü** sinyal türü ve sinyal seçin.

  ![başka bir](./media/expressroute-monitoring-metrics-alerts/alertshowto6activitylog.jpg)

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute bağlantınızı yapılandırın.
  
  * [Bağlantı hattı oluşturma ve değiştirme](expressroute-howto-circuit-arm.md)
  * [Eşleme yapılandırması oluşturma ve değiştirme](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)
