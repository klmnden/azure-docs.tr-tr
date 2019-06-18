---
title: Xamarin.Android uygulamanıza anında iletme bildirimleri ekleme | Microsoft Docs
description: Xamarin.Android uygulamanıza anında iletme bildirimleri göndermek için Azure App Service ve Azure Notification Hubs'ı kullanmayı öğrenin
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: crdun
ms.openlocfilehash: f2c58d64a662a6e2be355c1f672c01e5a679b3f8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62127955"
---
# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Xamarin.Android uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, anında iletme bildirimleri ekleme [Xamarin.Android hızlı](app-service-mobile-windows-store-dotnet-get-started.md) anında iletme bildirimi kayıt eklenen her zaman cihaza gönderilir, böylece proje.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantı paketi gerekir. Daha fazla bilgi için [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) Kılavuzu.

## <a name="prerequisites"></a>Önkoşullar

Bu öğretici, Kurulum gerektirir:

* Etkin bir Google hesabı. Bir Google hesabı için kaydolabilirsiniz [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302).
* [Google Cloud Messaging istemci bileşeni](https://components.xamarin.com/view/GCMClient/).

## <a name="configure-hub"></a>Bildirim hub'ı yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Etkinleştirme Firebase Cloud Messaging

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-to-send-push-requests"></a>Anında iletme istekleri göndermek için Azure'ı yapılandırma

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a id="update-server"></a>Anında iletme bildirimleri göndermek için sunucu projesi güncelleştirme

[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>Anında iletme bildirimleri için istemci projesi yapılandırma

[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>Uygulamanıza anında iletme bildirimleri kod ekleme

[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Uygulamanıza anında iletme bildirimleri test

Öykünücüde sanal cihazı kullanarak uygulamayı test edebilirsiniz. Öykünücüde çalıştırırken gereken ek yapılandırma adımları vardır.

1. Sanal cihaz bir Android sanal cihazı (AVD) Yöneticisi'ni hedef olarak Google API'leri olması gerekir.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Tıklayarak Android cihaza bir Google hesabı ekleyin **uygulamaları** > **ayarları** > **Hesap Ekle**, ardından istemleri uygulayın.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Önce Yapılacaklar listesi uygulaması olarak çalıştırmak ve yeni bir todo öğesini ekleyin. Bu kez, bildirim alanında bir bildirim simgesi görüntülenir. Bildirimin tam metin görüntülemek için bildirim çekmecesini açabilirsiniz.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: https://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: https://components.xamarin.com/view/azure-mobile-services/
