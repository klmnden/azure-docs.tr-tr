---
title: Apple anında iletilen bildirim servisi Azure Notification hubs'ı yapılandırma | Microsoft Docs
description: Apple anında iletilen bildirim servisi (APNS) ayarları ile bir Azure bildirim hub'ı yapılandırmayı öğrenin.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 03/25/2019
ms.author: jowargo
ms.openlocfilehash: 9a9db9f05895569b050e56cdeec1ee2ee25af0df
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60237822"
---
# <a name="configure-apple-push-notification-service-apns-settings-for-a-notification-hub-in-the-azure-portal"></a>Azure portalında bir bildirim hub'ı için Apple anında iletilen bildirim servisi (APNS) ayarlarını yapılandırın
Bu makalede Azure portalını kullanarak bir Azure bildirim hub'ı Apple anında iletilen bildirim servisi (APNS) ayarlarını yapılandırmak nasıl gösterir. 

## <a name="prerequisites"></a>Önkoşullar
Bildirim hub'ı henüz oluşturmadıysanız, şimdi oluşturun. Daha fazla bilgi için [Azure portalında bir Azure bildirim hub'ı oluşturma](create-notification-hub-portal.md). 

## <a name="configure-apple-push-notification-service"></a>Apple anında iletilen bildirim servisi yapılandırma

Aşağıdaki yordam, bildirim hub'ı Apple anında iletilen bildirim servisi (APNS) ayarlarını yapılandırmak için adımları sunar:

1. Azure portalında, üzerinde **bildirim hub'ı** sayfasında **Apple (APNS)** sol menüsünde.

1. İçin **kimlik doğrulama modu**, şunlardan birini seçin **sertifika** veya **belirteci**.

   a. Seçerseniz **sertifika**:
   * Dosya simgesini seçin ve ardından *.p12* karşıya yüklemek istediğiniz dosya.
   * Parola girin.
   * **Korumalı alan** modunu seçin. Uygulama Mağazası'ndan satın almış kullanıcılara anında iletme bildirimleri göndermek için seçin **üretim** modu.

     ![Ekran görüntüsü bir APNS sertifikası yapılandırma Azure portalında](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

   b. Seçerseniz **belirteci**:

   * İçin değerler girin **anahtarı kimliği**, **paket kimliği**, **Takım Kimliği**, ve **belirteci**.
   * **Korumalı alan** modunu seçin. Uygulama Mağazası'ndan satın almış kullanıcılara anında iletme bildirimleri göndermek için seçin **üretim** modu.

     ![Belirteç yapılandırma Azure portalında bir APNS ekran görüntüsü](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-token.png)

## <a name="next-steps"></a>Sonraki adımlar
İOS cihazlarına bildirim göndermek için bir öğretici için adım adım yönergeler içeren aşağıdaki makaleye bakın: [İOS cihazları için bildirim hub'ları ve APNS aracılığıyla anında iletme bildirimleri](notification-hubs-ios-apple-push-notification-apns-get-started.md)
