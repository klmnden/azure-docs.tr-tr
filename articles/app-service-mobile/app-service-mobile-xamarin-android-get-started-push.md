---
title: "Xamarin.Android uygulamanıza anında iletme bildirimleri ekleme | Microsoft Docs"
description: "Xamarin.Android uygulamanıza anında iletme bildirimleri göndermek için Azure App Service ve Azure bildirim hub'ları kullanmayı öğrenin"
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: c3757d56fb1792092710740dc5ffbd64f18730cf
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Xamarin.Android uygulamanıza anında iletme bildirimleri ekleme
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, anında iletme bildirimleri ekleme [Xamarin.Android Hızlı Başlangıç](app-service-mobile-windows-store-dotnet-get-started.md) proje böylece bir kayda eklenen her zaman bir anında iletme bildirimi cihaza gönderilir.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantısı paketi gerekir. Bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) daha fazla bilgi için.

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici için aşağıdakiler gereklidir:

* Etkin bir Google hesabı. Bir Google hesabı için kaydolabilirsiniz [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
* [Google Cloud Messaging istemci bileşeni](http://components.xamarin.com/view/GCMClient/).

## <a name="configure-hub"></a>Bildirim hub'ı yapılandırma
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Firebase etkinleştirmek bulut Mesajlaşma
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-to-send-push-requests"></a>Gönderme istekleri göndermek için Azure yapılandırma
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a id="update-server"></a>Güncelleştirme anında iletme bildirimleri göndermek için sunucu projesi
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>İstemci projesi anında iletme bildirimleri için yapılandırma
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>Uygulamanıza anında iletme bildirimleri kod ekleme
[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Test, uygulamanızda anında iletme bildirimleri
Öykünücüde sanal cihazı kullanarak uygulamayı test edebilirsiniz. Bir öykünücü üzerinde çalışırken gerekli ek yapılandırma adımları vardır.

1. Dağıtma ve Google API'leri hedef olarak ayarlanmış Android sanal cihazı (AVD) Yöneticisi'nde aşağıda gösterildiği gibi sanal cihaza hata ayıklama emin olun.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)
2. Tıklayarak Android aygıtına bir Google hesabı ekleyin **uygulamaları** > **ayarları** > **hesabı eklemek**, ardından yönergeleri izleyin.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)
3. Önce Yapılacaklar listesi uygulaması gibi çalıştırabilir ve yeni bir Yapılacaklar öğesi ekleyin. Bu süre, bildirim alanında bir bildirim simgesi görüntülenir. Tam metin bildirimi görüntülemek için bildirim çekmecesini açabilirsiniz.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
