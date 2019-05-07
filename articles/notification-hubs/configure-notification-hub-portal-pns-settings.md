---
title: Azure Notification hubs'ı anında iletme bildirimlerini ayarlama | Microsoft Docs
description: Platform bildirim sistemi (PNS) ayarları kullanarak Azure portalında Azure Notification Hubs ' kurmayı öğrenin.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: jowargo
ms.openlocfilehash: ee627a168e6ca9bb758d994a3f75cc6185976971
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65203699"
---
# <a name="set-up-push-notifications-in-a-notification-hub-in-the-azure-portal"></a>Azure portalında bir bildirim hub'ı anında iletme bildirimleri ayarlayın

Azure Notification hubs'ı, Ölçeklendirmesi eşitlenene ve kullanımı kolay olan bir anında iletme altyapısı sağlar. Herhangi bir arka uç (Bulut veya şirket içi) herhangi bir platformda (iOS, Android, Windows, Kindle, Baidu) bildirimleri göndermek için Notification Hubs'ı kullanın. Daha fazla bilgi için [Azure Notification Hubs nedir?](notification-hubs-push-notification-overview.md).

Bu hızlı başlangıçta, birden çok platformda anında iletme bildirimleri için ayarlamak için bildirim hub'ları, platform bildirim sistemi (PNS) ayarları kullanacaksınız. Hızlı başlangıçta Azure Portalı'nda atılması gereken adımları gösterilmektedir.

Bildirim hub'ı henüz oluşturmadıysanız, şimdi oluşturun. Daha fazla bilgi için [Azure portalında bir Azure bildirim hub'ı oluşturma](create-notification-hub-portal.md). 

## <a name="apple-push-notification-service"></a>Apple Anında İletilen Bildirim Servisi

Apple anında iletilen bildirim servisi (APNS ayarlama) ayarlamak için:

1. Azure portalında, üzerinde **bildirim hub'ı** sayfasında **Apple (APNS)** sol menüden.

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

Daha fazla bilgi için [Azure Notification Hubs'ı kullanarak anında iletme bildirimlerini iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md).

## <a name="google-firebase-cloud-messaging"></a>Google Firebase Cloud Messaging

Anında iletme bildirimleri için Google Firebase Cloud Messaging (FCM) ayarlamak için:

1. Azure portalında, üzerinde **bildirim hub'ı** sayfasında **Google (GCM/FCM)** sol menüden. 
2. Yapıştırma **API anahtarı** daha önce kaydettiğiniz FCM projesi. 
3. **Kaydet**’i seçin. 

   ![Bildirim hub'ları için Google FCM yapılandırma gösteren ekran görüntüsü](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)

Bu adımları tamamladıktan sonra bir uyarı bildirim hub'ı başarıyla güncelleştirildiğini belirtir. **Kaydet** düğmesi devre dışıdır. 

Daha fazla bilgi için [anında iletme bildirimleri Android cihazlar için Notification Hubs ve FCM Google'ı kullanarak](notification-hubs-android-push-notification-google-fcm-get-started.md).

## <a name="windows-push-notification-service"></a>Windows anında iletilen bildirim servisi

Windows anında bildirim hizmeti (WNS ayarlama) ayarlamak için:

1. Azure portalında, üzerinde **bildirim hub'ı** sayfasında **Windows (WNS)** sol menüden.
2. İçin değerler girin **paket SID'si** ve **güvenlik anahtarı**.
3. **Kaydet**’i seçin.

   ![Paket SID'si ve güvenlik anahtarı kutuları gösteren ekran görüntüsü](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Bilgi için [bildirimlerini gönderir UWP uygulamaları için Azure Notification Hubs'ı kullanarak](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).

## <a name="microsoft-push-notification-service-for-windows-phone"></a>Windows Phone için Microsoft anında bildirim hizmeti

Windows Phone için Microsoft anında iletme bildirimi Hizmeti'ni (MPNS ayarlama) ayarlamak için: 

1. Azure portalında, üzerinde **bildirim hub'ı** sayfasında **Windows Phone (MPNS)** sol menüden.
1. Ya da kimliği doğrulanmamış veya kimliği doğrulanmış bir anında iletme bildirimlerini etkinleştirin:

   a. Kimliği doğrulanmamış anında iletme bildirimleri etkinleştirmek için seçin **kimliği doğrulanmamış anında iletmeleri etkinleştir** > **Kaydet**.

      ![Kimliği doğrulanmamış anında iletme bildirimlerini etkinleştirme gösteren ekran görüntüsü](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

   b. Kimliği doğrulanmış bir anında iletme bildirimlerini etkinleştirmek için:
      * Araç çubuğunda **sertifikasını karşıya yükle**.
      * Dosya simgesini seçin ve ardından sertifika dosyasını seçin.
      * Sertifika için parola belirtin.
      * **Tamam**’ı seçin.
      * Üzerinde **Windows Phone (MPNS)** sayfasında **Kaydet**.

Daha fazla bilgi için [Notification Hubs'ı kullanarak anında iletme bildirimleri için Windows Phone uygulamaları](notification-hubs-windows-mobile-push-notifications-mpns.md).
      
## <a name="amazon-device-messaging"></a>Amazon Device Messaging

Anında iletme bildirimleri Amazon Device Messaging'i (ADM) ayarlamak için:

1. Azure portalında, üzerinde **bildirim hub'ı** sayfasında **Amazon (ADM)** sol menüden.
2. İçin değerler girin **istemci kimliği** ve **gizli**.
3. **Kaydet**’i seçin.
    
   ![Azure portalının ekran görüntüsü, ADM ayarları](./media/notification-hubs-kindle-get-started/notification-hub-adm-settings.png)

Daha fazla bilgi için [Kindle uygulamaları için Notification Hubs ile çalışmaya başlama](notification-hubs-kindle-amazon-adm-push-notification.md).

## <a name="baidu-android-china"></a>Baidu (Android China)

Baidu anında iletme bildirimleri için ayarlamak için:

1. Azure portalında, üzerinde **bildirim hub'ı** sayfasında **Baidu (Android China)** sol menüden. 
2. Girin **API anahtarı** Baidu bulut anında iletme projesinde Baidu konsolundan aldığınız. 
3. Girin **gizli anahtar** Baidu bulut anında iletme projesinde Baidu konsolundan aldığınız. 
4. **Kaydet**’i seçin. 

    ![Ekran görüntüsü, bildirim anında iletme bildirimleri Baidu (Android China) yapılandırmasını gösteren hub'ları](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)

Bu adımları tamamladıktan sonra bir uyarı bildirim hub'ı başarıyla güncelleştirildiğini belirtir. **Kaydet** düğmesi devre dışıdır. 

Daha fazla bilgi için [Baidu kullanarak Notification Hubs ile çalışmaya başlama](notification-hubs-baidu-china-android-notifications-get-started.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure portalında bir bildirim hub'ı platform bildirim sistemi ayarlarını yapılandırmak nasıl öğrendiniz. 

Çeşitli platformlar için anında iletme bildirimleri hakkında daha fazla bilgi edinmek için bu öğreticileri bakın:

- [İOS cihazları için bildirim hub'ları ve APNS aracılığıyla anında iletme bildirimleri](notification-hubs-ios-apple-push-notification-apns-get-started.md)
- [Notification Hubs ve FCM Google'ı kullanarak Android cihazlarına anında iletme bildirimleri](notification-hubs-android-push-notification-google-fcm-get-started.md)
- [Windows cihaz üzerinde çalışan bir UWP uygulamasına anında iletme bildirimleri](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
- [MPNS kullanarak bir Windows Phone 8 uygulaması anında iletme bildirimleri](notification-hubs-windows-mobile-push-notifications-mpns.md)
- [Bir Kindle uygulamasına anında iletme bildirimleri gönderme](notification-hubs-kindle-amazon-adm-push-notification.md)
- [Notification Hubs ve Baidu bulut anında iletme kullanarak anında iletme bildirimleri](notification-hubs-baidu-china-android-notifications-get-started.md)
