---
title: Azure portalını kullanarak bir Azure bildirim hub'ı Oluştur | Microsoft Docs
description: Bu öğreticide, Azure portalını kullanarak bir Azure bildirim hub'ı oluşturma konusunda bilgi edinin.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: jowargo
ms.openlocfilehash: 62e72f27e48f7bf220901f4eb36090f926724a2a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60682038"
---
# <a name="create-an-azure-notification-hub-in-the-azure-portal"></a>Azure portalında bir Azure bildirim hub'ı oluşturma 
Azure Notification Hubs, herhangi bir arka uçtan (bulut ya da şirket içi) herhangi bir platforma (iOS, Android, Windows, Kindle, Baidu vb.) bildirim göndermenize olanak tanıyan, kullanımı kolay ve ölçeği artırılmış bir gönderme altyapısı sağlar. Hizmeti hakkında daha fazla bilgi için bkz. [Azure Notification Hubs nedir?](notification-hubs-push-notification-overview.md).

Bu hızlı başlangıçta, Azure portalında bir bildirim hub'ı oluşturun. İlk bölüm, o ad alanında bir bildirim hub'ları ad alanı ve bir hub oluşturmak için adımları sağlar. İkinci bölümü, varolan bir Notification Hubs ad alanında bir bildirim hub'ı oluşturmak için adımları sağlar. 

## <a name="create-a-namespace-and-a-notification-hub"></a>Bir ad alanı ve bildirim hub'ı oluşturma
Bu bölümde, bir ad alanı ve ad alanındaki bir hub'ı oluşturun. 

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

## <a name="create-a-notification-hub-in-an-existing-namespace"></a>Mevcut bir ad alanında bir bildirim hub'ı oluşturma
Bu bölümde, var olan bir ad alanında bir bildirim hub'ı oluşturun. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** sol taraftaki menüde arama **bildirim hub'ı**seçin **yıldız** (`*`) yanındaki **bildirimhub'ıadalanları** eklemek üzere **Sık Kullanılanlar** sol menüde bölümü. Seçin **bildirim hub'ı ad alanları**. 

      ![Azure portalı - bildirim hub'ı ad seçin](./media/create-notification-hub-portal/select-notification-hub-namespaces-all-services.png)
3. Üzerinde **bildirim hub'ı ad alanları** sayfasında, listeden ad seçin. 

      ![Listeden ad seçin](./media/create-notification-hub-portal/select-namespace.png)
1. Üzerinde **bildirim hub'ı Namespace** sayfasında **ekleme Hub** araç. 

      ![Bildirim hub'ı ad alanları - ekleme Hub düğmesi](./media/create-notification-hub-portal/add-hub-button.png)
4. Üzerinde **yeni bildirim hub'ı** sayfasında, bildirim hub'ı için bir ad girin ve seçin **Tamam**.

      ![Sayfa -> Yeni bir Notification Hub, hub'ınız için bir ad girin](./media/create-notification-hub-portal/new-notification-hub-page.png)
4. Seçin **bildirimleri** (zil simgesi) yeni hub'ın dağıtım durumunu görmek için üst. Seçin **X** bildirim penceresini kapatmak için sağ köşesindeki. 

      ![Dağıtım bildirimi](./media/create-notification-hub-portal/deployment-notification.png)
5. Yenileme **bildirim hub'ı ad alanları** listesinde yeni hub'ınıza görmek için web sayfası. 

      ![Azure portalı - bildirimler -> Kaynağa git](./media/create-notification-hub-portal/new-hub-in-list.png)
6. Seçin, **bildirim hub'ı** bildirim hub'ınıza giriş sayfasını görmek için. 

      ![Azure portalı - bildirimler -> Kaynağa git](./media/create-notification-hub-portal/hub-home-page.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, bir bildirim hub'ı oluşturuldu. Hub'ı ile platform bildirim sistemi (PNS) ayarları yapılandırma konusunda bilgi için bkz: [bir bildirim hub'ı PNS ayarlarla yapılandırın](configure-notification-hub-portal-pns-settings.md). 