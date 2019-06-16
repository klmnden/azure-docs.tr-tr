---
title: Azure App Service ile Xamarin.iOS uygulamanıza anında iletme bildirimleri ekleme
description: Xamarin.iOS uygulamanıza anında iletme bildirimleri göndermek için Azure App Service'ı kullanmayı öğrenin
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 2921214a-49f8-45e1-a306-a85ce21defca
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: crdun
ms.openlocfilehash: de24b64ecd95eec79d7508f978acda9f0ae5a8d6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62097537"
---
# <a name="add-push-notifications-to-your-xamarinios-app"></a>Xamarin.iOS uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, anında iletme bildirimleri ekleme [Xamarin.iOS Hızlı Başlangıç](app-service-mobile-xamarin-ios-get-started.md) anında iletme bildirimi kayıt eklenen her zaman cihaza gönderilir, böylece proje.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantı paketi gerekir. Bkz: [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) daha fazla bilgi için.

## <a name="prerequisites"></a>Önkoşullar

* Tamamlamak [Xamarin.iOS hızlı](app-service-mobile-xamarin-ios-get-started.md) öğretici.
* Bir fiziksel bir iOS cihaz. Anında iletme bildirimleri, iOS simulator tarafından desteklenmez.

## <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Apple'nın Geliştirici portalında anında iletme bildirimleri için uygulamayı kaydetme

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-your-mobile-app-to-send-push-notifications"></a>Mobil anında iletme bildirimleri göndermek için uygulamanızı yapılandırın

[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için sunucu projesi güncelleştirme

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-your-xamarinios-project"></a>Xamarin.iOS projenizi yapılandırın

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

## <a name="add-push-notifications-to-your-app"></a>Uygulamanıza anında iletme bildirimleri ekleme

1. İçinde **QSTodoService**, aşağıdaki özelliği ekleyin böylece **AppDelegate** mobil istemci elde edebilirsiniz:

    ```csharp
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
    ```

2. Aşağıdaki `using` üstüne deyimi **AppDelegate.cs** dosya.

    ```csharp
    using Microsoft.WindowsAzure.MobileServices;
    using Newtonsoft.Json.Linq;
    ```

3. İçinde **AppDelegate**, geçersiz kılma **FinishedLaunching** olay:

   ```csharp
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
    ```

4. Aynı dosyada geçersiz kılma `RegisteredForRemoteNotifications` olay. Bu kodda sunucu tarafından desteklenen tüm platformlarda gönderilecek basit bir şablon bildirim kaydettirmekte olduğunuz.

    Notification Hubs ile şablonları hakkında daha fazla bilgi için bkz. [şablonları](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).

    ```csharp
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
    ```

5. Ardından, geçersiz kılma **DidReceivedRemoteNotification** olay:

   ```csharp
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
    ```

Uygulamanızı anında iletme bildirimlerini desteklemek için güncelleştirilmiştir.

## <a name="test"></a>Uygulamanıza anında iletme bildirimleri test

1. Tuşuna **çalıştırma** projeyi oluşturun ve uygulamayı yeteneğine sahip bir iOS cihazının başlatın düğmesine ve ardından tıklayın **Tamam** anında iletme bildirimleri kabul etmek için.

   > [!NOTE]
   > Anında iletme bildirimleri uygulamanızdan açıkça kabul etmeniz gerekir. Bu istek, yalnızca uygulamayı çalıştıran ilk kez oluşur.

2. Uygulamasında, bir görev yazın ve ardından artı ( **+** ) simgesi.
3. Bir bildirim alındıktan sonra tıklayın doğrulayın **Tamam** bildirimi kapatmak için.
4. 2\. adımı yineleyin, hemen uygulamayı kapatın ve sonra bir bildirim gösterildiğini doğrulayın.

Bu öğreticiyi tamamladınız.
