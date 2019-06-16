---
title: Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme
description: Hizmet durumu bildirimlerine, Microsoft Azure tarafından yayınlanan hizmet sistem durumu iletileri görüntülemenize olanak sağlar.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 4/12/2018
ms.author: dukek
ms.subservice: logs
ms.openlocfilehash: 8cf8c3eb86f55b5595ef9a1af83ea50580bf638b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069345"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme

Hizmet durumu bildirimlerine Azure altyapısı tarafından yayımlanır. Aboneliğiniz kapsamındaki kaynaklar hakkındaki bilgileri içerirler. Bu bildirimleri bir alt sınıfı etkinlik günlüğü olaylarını olan ve bu nedenle de etkinlik günlüğü'nde bulunabilir. Hizmet durumu bildirimlerine bilgilendirici veya sınıf bağlı olarak eyleme dönüştürülebilir olabilir.

Hizmet durumu bildirimlerine çeşitli sınıfları hakkında daha fazla bilgi için bkz. [hizmet durumu bildirimlerini özellikleri](../../service-health/service-health-notifications-properties.md).

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Azure portalında, hizmet durumu bildirimlerini görüntüleme

1. İçinde [Azure portalında](https://portal.azure.com)seçin **İzleyici**.

    ![Ekran görüntüsü, Azure portal menüsünde, seçilen İzleyici](./media/service-notifications/home-monitor.png)

    Azure İzleyici, izleme ayarlarınızı ve verilerinizi tek bir birleştirilmiş görünümde getirmektedir. İlk olarak **Etkinlik günlüğü** bölümü açılır.

1. Seçin **uyarılar**.

    ![Seçili uyarıları ile izleme ekran etkinlik günlüğü](./media/service-notifications/service-health-summary.png)

1. Seçin **+ etkinlik günlüğü uyarısı Ekle**, gelecek hizmet bildirimleri için bildirim emin olmak için bir alarm ayarlayın. Daha fazla bilgi için [etkinlik günlüğü uyarıları hizmet bildirimlerinde oluşturma](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="next-steps"></a>Sonraki adımlar

* Alma [uyarı bildirimleri hizmet durumu bildirimi her](../../azure-monitor/platform/alerts-activity-log-service-notifications.md) gönderilir.  
* Daha fazla bilgi edinin [etkinlik günlüğü uyarıları](../../azure-monitor/platform/activity-log-alerts.md).
