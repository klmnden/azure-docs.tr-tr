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
ms.openlocfilehash: 244a4ebe20863945bfc3b6236e70e786387c8909
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67116658"
---
1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Seçin **tüm hizmetleri** seçin ve soldaki menüden **Notification hubs'ı** içinde **mobil** bölümü. Yıldız simgesini hizmetine eklemek için hizmet adının yanındaki **Sık Kullanılanlar** sol menüde bölümü. Siz ekledikten sonra **Notification hubs'ı** için **Sık Kullanılanlar**, sol taraftaki menüde seçin.

      ![Azure portalı - bildirim hub'ları seçin](./media/notification-hubs-portal-create-new-hub/all-services-select-notification-hubs.png)

1. Üzerinde **Notification hubs'ı** sayfasında **Ekle** araç.

      ![Bildirim hub'ları - ekleme araç çubuğu düğmesi](./media/notification-hubs-portal-create-new-hub/add-toolbar-button.png)

1. Üzerinde **bildirim hub'ı** sayfasında, aşağıdaki adımları uygulayın:

    1. Bir ad girin **bildirim hub'ı**.  

    1. Bir ad girin **yeni bir ad alanı oluşturma**. Bir ad alanı, hub'ları bir veya daha fazla içerir.

    1. Bir değer seçerek **konumu** aşağı açılan liste kutusu. Bu değer, bildirim hub'ı oluşturmak istediğiniz konumu belirtir.

    1. Mevcut bir kaynak grubunda seçin **kaynak grubu**, ya da yeni bir kaynak grubu için bir ad oluşturun.

    1. **Oluştur**’u seçin.

        ![Azure portalı - Bildirim hub'ı özelliklerini ayarlama](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

1. Seçin **bildirimleri** (zil simgesi) seçip **kaynağa Git**. Üzerinde listeyi yenileyebilirsiniz **Notification hubs'ı** sayfasında ve bildirim hub'ınızı seçin.

      ![Azure portalı - bildirimler -> Kaynağa git](./media/notification-hubs-portal-create-new-hub/go-to-notification-hub.png)

1. Listeden **Erişim İlkeleri**'ni seçin. Verilen iki bağlantı dizesini not edin. Anında iletme bildirimleri işlemek için bunları daha sonra ihtiyacınız olacak.

      >[!IMPORTANT]
      >Yapmak *değil* kullanın **DefaultFullSharedAccessSignature** uygulamanızdaki ilkesi. Bu, yalnızca, arka uç kullanılmak üzere tasarlanmıştır.
      >

      ![Azure portalı - Bildirim hub'ı bağlantı dizeleri](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)
