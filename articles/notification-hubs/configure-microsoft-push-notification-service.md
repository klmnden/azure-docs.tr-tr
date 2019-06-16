---
title: Microsoft anında bildirim hizmeti, Azure Notification hubs'ı yapılandırma | Microsoft Docs
description: Bir Azure bildirim hub'ı Microsoft anında bildirim hizmeti ayarlarını yapılandırmayı öğrenin.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: 1c76b44438e6527439d0a370c92f4120424b8da5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60240306"
---
# <a name="configure-microsoft-push-notification-service-mpns-settings-for-a-notification-hub-in-the-azure-portal"></a>Azure portalında bir bildirim hub'ı Microsoft anında iletme bildirimi Hizmeti'ni (MPNS) ayarlarını yapılandırın
Bu makalede Azure portalını kullanarak bir Azure bildirim hub'ı Microsoft anında iletme bildirimi Hizmeti'ni (MPNS) ayarlarını yapılandırmak nasıl gösterir. 

## <a name="prerequisites"></a>Önkoşullar
Bildirim hub'ı henüz oluşturmadıysanız, şimdi oluşturun. Daha fazla bilgi için [Azure portalında bir Azure bildirim hub'ı oluşturma](create-notification-hub-portal.md). 

## <a name="configure-microsoft-push-notification-service-mpns"></a>Microsoft anında bildirim hizmeti (MPNS) yapılandırma

Aşağıdaki yordam, bildirim hub'ı Microsoft anında iletme bildirimi Hizmeti'ni (MPNS) ayarlarını yapılandırmak için adımları sunar: 

1. Azure portalında, üzerinde **bildirim hub'ı** sayfasında **Windows Phone (MPNS)** sol menüsünde.
1. Ya da kimliği doğrulanmamış veya kimliği doğrulanmış bir anında iletme bildirimlerini etkinleştirin:

   a. Kimliği doğrulanmamış anında iletme bildirimleri etkinleştirmek için seçin **kimliği doğrulanmamış anında iletmeleri etkinleştir** > **Kaydet**.

      ![Kimliği doğrulanmamış anında iletme bildirimlerini etkinleştirme gösteren ekran görüntüsü](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. Kimliği doğrulanmış bir anında iletme bildirimlerini etkinleştirmek için:
      * Araç çubuğunda **sertifikasını karşıya yükle**.
      * Dosya simgesini seçin ve ardından sertifika dosyasını seçin.
      * Sertifika için parola belirtin.
      * **Tamam**’ı seçin.
      * Üzerinde **Windows Phone (MPNS)** sayfasında **Kaydet**.

## <a name="next-steps"></a>Sonraki adımlar
Azure Notification hubs'ı ve Microsoft anında iletme bildirimi Hizmeti'ni (MPNS) kullanarak Windows Phone cihazları için bildirimler göndermek için bir öğretici için adım adım yönergeler içeren bkz [bildirimi kullanarak anında iletme bildirimleri için Windows Phone uygulamaları Hub'ları](notification-hubs-windows-mobile-push-notifications-mpns.md).

