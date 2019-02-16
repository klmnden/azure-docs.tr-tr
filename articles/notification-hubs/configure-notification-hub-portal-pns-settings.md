---
title: Azure bildirim hub'ı PNS ayarlarla yapılandırın. | Microsoft Docs
description: Bu hızlı başlangıçta, platform bildirim sistemi (PNS) ayarları ile Azure portalında bir bildirim hub'ı yapılandırma konusunda bilgi edinin.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: jowargo
ms.openlocfilehash: 7f7e4a4d75a8e118da6f026817bc4ecfcc7a60db
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56314134"
---
# <a name="configure-an-azure-notification-hub-with-platform-notification-system-settings-in-the-azure-portal"></a>Platform bildirim sistemi ayarları Azure portalında bir Azure bildirim hub'ı yapılandırın 
Azure Notification Hubs, herhangi bir arka uçtan (bulut ya da şirket içi) herhangi bir platforma (iOS, Android, Windows, Kindle, Baidu vb.) bildirim göndermenize olanak tanıyan, kullanımı kolay ve ölçeği artırılmış bir gönderme altyapısı sağlar. Hizmeti hakkında daha fazla bilgi için bkz. [Azure Notification Hubs nedir?](notification-hubs-push-notification-overview.md).

[Azure portalını kullanarak bir Azure bildirim hub'ı oluşturma](create-notification-hub-portal.md) , zaten yapmadıysanız. Bu hızlı başlangıçta, platform bildirim sistemi (PNS) ayarları ile Azure portalında bir bildirim hub'ı yapılandırma konusunda bilgi edinin.

## <a name="apple-push-notification-service-apns"></a>Apple anında iletilen bildirim servisi (APNS)
1. Üzerinde **bildirim hub'ı** select Azure portalında sayfası **Apple (APNS)** altında **ayarları** sol menüsünde.
2. Seçerseniz **sertifika**, ve aşağıdaki eylemleri gerçekleştirin:
    1. Seçin **dosya simgesi**seçip **.p12** dosyasını karşıya yükleyin. 
    2. Belirtin **parola**.
    3. **Korumalı alan** modunu seçin. **Üretim** seçeneğini yalnızca uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek istiyorsanız kullanın.

        ![Azure portalında APNS sertifikası yapılandırma](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)
3. Seçerseniz **belirteci**ve aşağıdaki adımları izleyin: 
    1. İçin değerler girin **anahtar kimliği**, **paket Kimliğini**, **kimliği takım**, ve **belirteci**.
    2. **Korumalı alan** modunu seçin. **Üretim** seçeneğini yalnızca uygulamanızı mağazadan satın alan kullanıcılara anında iletme bildirimleri göndermek istiyorsanız kullanın.

        ![Azure portalında APNS belirteç yapılandırma](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-token.png)

Azure Notification Hubs ve Apple anında iletilen bildirim servisi (APNS) kullanarak iOS cihazları için bildirimler gönderme tam öğretici için bkz: [Bu öğreticide](notification-hubs-ios-apple-push-notification-apns-get-started.md).

## <a name="google-firebase-cloud-messaging-fcm"></a>Google Firebase Cloud Messaging (FCM)
1. Üzerinde **bildirim hub'ı** select Azure portalında sayfası **Google (GCM/FCM)** altında **ayarları** sol menüsünde. 
2. Yapıştırma **sunucu anahtarı** daha önce kaydettiğiniz FCM projesi. 
3. Araç çubuğunda **Kaydet**’i seçin. 

    ![Azure Notification Hubs - Google (FCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)
4. Notification hubs'ı başarıyla güncelleştirildiğini uyarılar bir ileti görürsünüz. **Kaydet** düğmesi devre dışıdır. 

Azure Notification Hubs ve Google Firebase Cloud Messaging kullanarak Android cihazları için bildirimler gönderme tam öğretici için bkz: [Bu öğreticide](notification-hubs-android-push-notification-google-fcm-get-started.md).

## <a name="windows-push-notification-service-wns"></a>Windows anında bildirim hizmeti (WNS)
1. Üzerinde **bildirim hub'ı** select Azure portalında sayfası **Windows (WNS)** altında **ayarları** sol menüsünde.
2. İçin değerler girin **paket SID'si** ve **güvenlik anahtarı**.
3. Araç çubuğunda **Kaydet**’i seçin.

    ![Paket SID'si ve Güvenlik Anahtarı kutuları](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)


Windows cihaz üzerinde çalışan bir evrensel Windows Platformu (UWP) uygulamasına bildirimler gönderme tam öğretici için bkz: [Bu öğreticide](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).

## <a name="windows-phone---microsoft-push-notification-service"></a>Windows Phone - Microsoft anında bildirim hizmeti
1. Üzerinde **bildirim hub'ı** select Azure portalında sayfası **Windows Phone (MPNS)** altında **ayarları**.
2. Kimliği doğrulanmamış anında iletmeleri etkinleştir isteyip istemediğinizi seçin **kimliği doğrulanmamış anında iletmeleri etkinleştir**seçip **Kaydet** araç.

    ![Azure portalı - Kimliği doğrulanmamış anında iletme bildirimlerini etkinleştirme](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)
3. Kullanmak istiyorsanız **kimliği doğrulanmış** anında iletme, aşağıdaki adımları izleyin:
    1. Seçin **sertifikasını karşıya yükle** araç.
    2. Seçin **dosya simgesi** ve sertifika dosyasını seçin.
    3. Girin **parola** sertifikası. 
    4. Seçin **Tamam** kapatmak için **sertifikasını karşıya yükle** sayfası. 
    5. Üzerinde **Windows Phone(MPNS)** sayfasında **Kaydet** araç.

Microsoft anında iletme bildirimi Hizmeti'ni (MPNS) kullanarak bir Windows Phone 8 uygulama için bildirimleri gönderme tam öğretici için bkz: [Bu öğreticide](notification-hubs-windows-mobile-push-notifications-mpns.md).
      
## <a name="amazon-device-messaging-adm"></a>Amazon cihaz Mesajlaşma (ADM)
1. Üzerinde **bildirim hub'ı** select Azure portalında sayfası **Amazon (ADM)** altında **ayarları** sol menüsünde.
2. İçin değerler girin **istemci kimliği** ve **gizli**.
3. Araç çubuğunda **Kaydet**’i seçin.
    
    ![Azure Notification Hubs - ADM ayarları](./media/notification-hubs-kindle-get-started/notification-hub-adm-settings.png)

Azure Notification hubs'ı bir Kindle uygulamasına anında iletme bildirimlerini kullanarak tam öğretici için bkz. [Bu öğreticide](notification-hubs-kindle-amazon-adm-push-notification.md).

## <a name="baidu-android-china"></a>Baidu (Android China)
1. Üzerinde **bildirim hub'ı** select Azure portalında sayfası **Baidu (Android China)** altında **ayarları** sol menüsünde. 
2. Girin **API anahtarı** Baidu bulut anında iletme projesinde Baidu konsolundan elde. 
3. Girin **gizli anahtar** Baidu bulut anında iletme projesinde Baidu konsolundan aldığınız. 
4. Araç çubuğunda **Kaydet**’i seçin. 

    ![Azure Notification Hubs - Baidu (Android China)](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)
4. Notification hubs'ı başarıyla güncelleştirildiğini uyarılar bir ileti görürsünüz. **Kaydet** düğmesi devre dışıdır. 

Azure Notification Hubs ve Baidu bulut anında iletme bildirimleri gönderme tam öğretici için bkz: [Bu öğreticide](notification-hubs-baidu-china-android-notifications-get-started.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure portalında bir bildirim hub'ın farklı platform bildirim sistemlerinin yapılandırma öğrendiniz. 

Öğreticiler, bu farklı platformlar için bildirimler gönderme için tam adım adım yönergeler için bkz. **öğreticiler** bölümü.

- [İOS cihazlar için Azure Notification Hubs ve Apple anında iletilen bildirim servisi (APNS) kullanarak anında iletme bildirimleri](notification-hubs-ios-apple-push-notification-apns-get-started.md).
- [Android cihazlar için Azure Notification Hubs ve Google Firebase Cloud Messaging kullanarak anında iletme bildirimleri](notification-hubs-android-push-notification-google-fcm-get-started.md).
- [Windows cihaz üzerinde çalışan bir evrensel Windows Platformu (UWP) uygulamasına anında iletme bildirimleri](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
- [Bir Windows Phone 8 için Microsoft anında iletme bildirimi Hizmeti'ni (MPNS) kullanarak anında iletme bildirimleri uygulamasına](notification-hubs-windows-mobile-push-notifications-mpns.md).
- [Bir Kindle uygulamasına anında iletme bildirimleri](notification-hubs-kindle-amazon-adm-push-notification.md).
- [Azure Notification Hubs ve Baidu bulut anında iletme kullanarak anında iletme bildirimleri](notification-hubs-baidu-china-android-notifications-get-started.md).
