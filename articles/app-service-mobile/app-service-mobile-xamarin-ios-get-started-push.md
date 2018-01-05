---
title: "Xamarin.iOS uygulamanızı Azure App Service için anında iletme bildirimleri ekleme"
description: "Azure uygulama hizmeti Xamarin.iOS uygulamanızı anında iletme bildirimleri göndermek için nasıl kullanılacağını öğrenin"
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: 
ms.assetid: 2921214a-49f8-45e1-a306-a85ce21defca
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: crdun
ms.openlocfilehash: b8d5a8d8725e2e9412cef7c377b17a77f34be27d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="add-push-notifications-to-your-xamarinios-app"></a>Xamarin.iOS uygulamanıza anında iletme bildirimleri ekleme
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, anında iletme bildirimleri ekleme [Xamarin.iOS Hızlı Başlangıç](app-service-mobile-xamarin-ios-get-started.md) proje böylece bir kayda eklenen her zaman bir anında iletme bildirimi cihaza gönderilir.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantısı paketi gerekir. Bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) daha fazla bilgi için.

## <a name="prerequisites"></a>Önkoşullar
* Tamamlamak [Xamarin.iOS quickstart](app-service-mobile-xamarin-ios-get-started.md) Öğreticisi.
* Fiziksel bir iOS cihazına. Anında iletme bildirimlerini iOS simülatörü tarafından desteklenmez.

## <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Apple Geliştirici portalındaki anında iletme bildirimleri için uygulamanızı kaydetmeniz
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için mobil uygulama yapılandırma
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Güncelleştirme anında iletme bildirimleri göndermek için sunucu projesi
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a>Xamarin.iOS projesi yapılandırma
[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-to-your-app"></a>Uygulamanıza anında iletme bildirimleri ekleme
1. İçinde **QSTodoService**, aşağıdaki özellik eklemek için **AppDelegate** mobil istemci elde edebilirsiniz:
   
            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }
2. Aşağıdakileri ekleyin `using` deyimi üstüne **AppDelegate.cs** dosya.
   
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
3. İçinde **AppDelegate**, geçersiz kılma **FinishedLaunching** olay:
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());
   
            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();
   
            return true;
        }
4. Aynı dosyada geçersiz kılma **RegisteredForRemoteNotifications** olay. Bu kod sunucu tarafından tüm desteklenen platformlarda gönderilecek basit bir şablon bildirim kaydediyorsunuz.
   
    Notification Hubs ile şablonları hakkında daha fazla bilgi için bkz: [şablonları](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


1. Ardından, geçersiz kılma **DidReceivedRemoteNotification** olay:
   
        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;
   
            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();
   
            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Uygulamanıza anında iletme bildirimlerini desteklemek üzere güncelleştirilmiştir.

## <a name="test"></a>Test, uygulamanızda anında iletme bildirimleri
1. Tuşuna **çalıştırmak** projeyi oluşturun ve uygulamayı bir iOS özellikli aygıtı Başlat düğmesine ve ardından **Tamam** anında iletme bildirimleri kabul etmek için.
   
   > [!NOTE]
   > Anında iletme bildirimleri, uygulamanızdan açıkça kabul etmeniz gerekir. Bu istek, yalnızca uygulamayı çalıştıran ilk kez oluşur.
   > 
   > 
2. Uygulamasında bir görevi yazın ve ardından artı (**+**) simgesi.
3. Bir bildirim alındı ve ardından doğrulamak **Tamam** bildirim kapatılamadı.
4. 2. adımı yineleyin ve hemen uygulamasını kapatın, sonra bir bildirim göründüğünü doğrulayın.

Bu öğreticiyi başarıyla tamamladınız.

<!-- Images. -->

<!-- URLs. -->



