---
title: Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme
description: Hizmet durumu bildirimlerine, Microsoft Azure tarafından yayınlanan hizmet sistem durumu iletileri görüntülemenize olanak sağlar.
author: stephbaron
ms.author: stbaron
services: monitoring
ms.service: service-health
ms.topic: conceptual
ms.date: 6/27/2019
ms.subservice: ''
ms.openlocfilehash: d2e18ae28e79590cb04ee0045341ea817be4a3bc
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538360"
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Azure portalını kullanarak hizmet durumu bildirimlerini görüntüleme

Hizmet durumu bildirimlerine, Azure altyapısına tarafından yayımlanır [Azure etkinlik günlüğü](../azure-monitor/platform/activity-logs-overview.md).  Bildirimleri aboneliğiniz kapsamındaki kaynaklar hakkında bilgiler içerir. Etkinlik günlüğü'nde depolanan bilgileri muhtemelen büyük hacmi göz önünde bulundurulduğunda, görüntüleyin ve hizmet durumu bildirimlerine uyarılar ayarlamak daha kolay hale getirmek için ayrı bir kullanıcı arabirimi yoktur. 

Hizmet durumu bildirimlerine bilgilendirici veya sınıf bağlı olarak eyleme dönüştürülebilir olabilir.

Hizmet durumu bildirimlerine çeşitli sınıfları hakkında daha fazla bilgi için bkz. [hizmet durumu bildirimlerini özellikleri](service-health-notifications-properties.md).

## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Azure portalında, hizmet durumu bildirimlerini görüntüleme

1. İçinde [Azure portalında](https://portal.azure.com)seçin **İzleyici**.

    ![Ekran görüntüsü, Azure portal menüsünde, seçilen İzleyici](./media/service-notifications/home-monitor.png)

    Azure İzleyici, izleme ayarlarınızı ve verilerinizi tek bir birleştirilmiş görünümde getirmektedir. İlk olarak **Etkinlik günlüğü** bölümü açılır.

1. Seçin **uyarılar**.

    ![Seçili uyarıları ile izleme ekran etkinlik günlüğü](./media/service-notifications/service-health-summary.png)

1. Seçin **+ etkinlik günlüğü uyarısı Ekle**, gelecek hizmet bildirimleri için bildirim emin olmak için bir alarm ayarlayın. Daha fazla bilgi için [etkinlik günlüğü uyarıları hizmet bildirimlerinde oluşturma](../azure-monitor/platform/alerts-activity-log-service-notifications.md).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [etkinlik günlüğü uyarıları](../azure-monitor/platform/activity-log-alerts.md).
