---
title: Android uygulamanızı Mobile Apps için anında iletme bildirimleri ekleme | Microsoft Docs
description: Mobile Apps Android uygulamanızı anında iletme bildirimleri göndermek için nasıl kullanılacağını öğrenin.
services: app-service\mobile
documentationcenter: android
manager: crdun
editor: ''
author: conceptdev
ms.assetid: 9058ed6d-e871-4179-86af-0092d0ca09d3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/17/2017
ms.author: crdun
ms.openlocfilehash: 9e9f7aba49c53a1a6fcc611ed771f266eb49c883
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
ms.locfileid: "27593037"
---
# <a name="add-push-notifications-to-your-android-app"></a>Android uygulamanızı anında iletme bildirimleri ekleme
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, anında iletme bildirimleri ekleme [Android Hızlı Başlangıç] proje böylece bir kayda eklenen her zaman bir anında iletme bildirimi cihaza gönderilir.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantısı paketi gerekir. Daha fazla bilgi için bkz: [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Önkoşullar
Aşağıdakiler gerekir:

* Projenizin arka uç bağlı olarak bir IDE:

  * [Android Studio](https://developer.android.com/sdk/index.html) bu uygulamanın bir Node.js arka ucu varsa.
  * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) veya bu uygulamanın Microsoft .NET arka ucu varsa sonraki.
* Android 2.3 ve üzeri, Google depo düzeltme 27 veya daha yeni ve Google Play hizmetlerini 9.0.2 veya Firebase Cloud Messaging daha yeni.
* Tamamlamak [Android Hızlı Başlangıç].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Firebase Cloud Messaging'i destekleyen bir proje oluşturma
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Bildirim hub'ı yapılandırma
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için Azure yapılandırma
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Sunucu projesi için anında iletme bildirimlerini etkinleştirin
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Uygulamanıza anında iletme bildirimleri ekleme
Bu bölümde, istemci Android uygulamanızı anında iletme bildirimleri işlemek için güncelleştirin.

### <a name="verify-android-sdk-version"></a>Android SDK sürümünü doğrula
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Sonraki adımınız, Google Play hizmetlerini yüklemektir. Firebase Cloud Messaging sahip bazı en az API düzeyi gereksinimlerini geliştirme ve test, hangi **minSdkVersion** bildiriminde özellik için uygun olmalıdır.

Eski bir aygıtla sınıyorsanız başvurun [ekleme Firebase Android projenize için] nasıl düşük bu değeri ayarlayabilir ve uygun şekilde ayarlanmış belirlemek için.

### <a name="add-firebase-cloud-messaging-to-the-project"></a>Firebase Cloud Messaging projeye ekleyin
[!INCLUDE [Add Firebase Cloud Messaging](../../includes/app-service-mobile-add-firebase-cloud-messaging.md)]

### <a name="add-code"></a>Kod ekleme
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Uygulamayı yayımlanan mobil hizmete karşı test etme
Doğrudan bir USB kablosu ile Android telefonla ekleme veya öykünücüde sanal cihazı kullanarak uygulamayı test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticiyi tamamladığınıza göre aşağıdaki öğreticiler birini açın etmeden göz önünde bulundurun:

* [Android uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-android-get-started-users.md).
  Todolist hızlı başlangıç projesi Android desteklenen kimlik sağlayıcısı kullanarak kimlik doğrulaması eklemeyi öğrenin.
* [Android uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-android-get-started-offline-data.md).
  Mobile Apps arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı Eşitleme ile kullanıcılar mobil uygulama ile etkileşim kurabilen&mdash;görüntüleme, ekleme veya değiştirme veri&mdash;hiçbir ağ bağlantısı olduğunda bile.

<!-- URLs -->
[Android Hızlı Başlangıç]: app-service-mobile-android-get-started.md
[ekleme Firebase Android projenize için]:https://firebase.google.com/docs/android/setup
