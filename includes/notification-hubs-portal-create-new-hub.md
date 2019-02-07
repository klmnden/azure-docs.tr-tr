---
title: include dosyası
description: include dosyası
services: notification-hubs
author: jwargo
ms.service: notification-hubs
ms.topic: include
ms.date: 01/17/2019
ms.author: jowargo
ms.custom: include file
ms.openlocfilehash: 00b7ffcba876b6abea59cff170331c7413a61d39
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823326"
---
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** seçin ve soldaki menüden **Notification hubs'ı** içinde **mobil** bölümü. Yıldızı seçin (`*`) eklemek için hizmet adının yanındaki **Sık Kullanılanlar** sol menüde bölümü. Sonra **Notification hubs'ı** eklenir **Sık Kullanılanlar**, sol taraftaki menüde seçin. 

      ![Azure portalı - bildirim hub'ları seçin](./media/notification-hubs-portal-create-new-hub/all-services-select-notification-hubs.png)
3. Üzerinde **Notification hubs'ı** sayfasında **Ekle** araç. 

      ![Bildirim hub'ları - ekleme araç çubuğu düğmesi](./media/notification-hubs-portal-create-new-hub/add-toolbar-button.png)
4. Üzerinde **bildirim hub'ı** sayfasında, aşağıdaki adımları uygulayın: 
    1. Belirtin bir **adı** bildirim **hub**.  
    2. Belirtin bir **adı** için **ad alanı**.
    3. Seçin bir **konumu** bildirim hub'ı oluşturmak istediğiniz. 
    4. Mevcut bir kaynak grubunu seçin veya yeni bir ad girin **kaynak grubu**.
    5. **Oluştur**’u seçin. 

        ![Azure portalı - Bildirim hub'ı özelliklerini ayarlama](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)
4. Seçin **bildirimleri** (zil simgesi) seçip **kaynağa Git**, veya listeden yenileme **Notification hubs'ı** sayfasında ve bildirim hub'ınızı seçin. 

      ![Azure portalı - bildirimler -> Kaynağa git](./media/notification-hubs-portal-create-new-hub/go-to-notification-hub.png)
5. Listeden **Erişim İlkeleri**'ni seçin. Verilen iki bağlantı dizesini not edin. Bu dizelere daha sonra anında iletme bildirimleri için ihtiyaç duyacaksınız.

      >[!IMPORTANT]
      >Uygulamanızda DefaultFullSharedAccessSignature **KULLANMAYIN**. Bunu yalnızca arka ucunuzda kullanmanız gerekir.
      >

      ![Azure portalı - Bildirim hub'ı bağlantı dizeleri](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)
