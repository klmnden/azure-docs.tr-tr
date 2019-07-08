---
title: Mobile Apps ile Android uygulamanıza anında iletme bildirimleri ekleme | Microsoft Docs
description: Mobile Apps, Android uygulamasına anında iletme bildirimleri göndermek için nasıl kullanılacağını öğrenin.
services: app-service\mobile
documentationcenter: android
manager: crdun
editor: ''
author: elamalani
ms.assetid: 9058ed6d-e871-4179-86af-0092d0ca09d3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 529aa8327d31cdda044178b6d03035b602744db2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443667"
---
# <a name="add-push-notifications-to-your-android-app"></a>Android uygulamanıza anında iletme bildirimleri ekleme

[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

> [!NOTE]
> Visual Studio App Center, mobil uygulama geliştirme merkezi hizmetlerinde yeni ve tümleşik yatırım yapıyor. Geliştiriciler **derleme**, **Test** ve **Dağıt** hizmetlerinin sürekli tümleştirme ve teslim işlem hattı ayarlayın. Uygulama dağıtıldığında, geliştiriciler kendi uygulamasını kullanarak kullanımı ve durumu izleyebilirsiniz **Analytics** ve **tanılama** kullanarak kullanıcılarla etkileşim kurun ve hizmetlerini **anında iletme** hizmeti. Geliştiriciler de yararlanabilir **Auth** , kullanıcıların kimliğini doğrulamak ve **veri** kalıcı hale getirmek ve uygulama verilerini bulutta eşitleme hizmeti. Kullanıma [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-android-get-started-push) bugün.
>

## <a name="overview"></a>Genel Bakış

Bu öğreticide, anında iletme bildirimleri ekleme [Android Hızlı Başlangıç] anında iletme bildirimi kayıt eklenen her zaman cihaza gönderilir, böylece proje.

İndirilen hızlı başlangıç sunucu projesi kullanmazsanız, anında iletme bildirimi uzantı paketi gerekir. Daha fazla bilgi için [Azure Mobile Apps için .NET arka uç sunucu SDK'sı ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Önkoşullar

Aşağıdakiler gerekir:

* Projenizin arka uç bağlı olarak bir IDE:

  * [Android Studio](https://developer.android.com/sdk/index.html) bu uygulamanın bir Node.js arka ucu varsa.
  * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) veya bu uygulamanın bir Microsoft .NET arka ucu varsa sonraki bir sürümü.
* Android 2.3 veya üstü, Google deposu düzeltme 27 veya daha yeni ve Google Play Services 9.0.2 veya üstü Firebase Cloud Messaging için.
* Tamamlamak [Android Hızlı Başlangıç].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Firebase Cloud Messaging'i destekleyen bir proje oluşturma

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Bildirim hub’ını yapılandırma

[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Anında iletme bildirimleri göndermek için Azure'ı yapılandırma

[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Sunucu projesi için anında iletme bildirimlerini etkinleştirme

[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Uygulamanıza anında iletme bildirimleri ekleme

Bu bölümde, istemci Android uygulamanızı anında iletme bildirimleri işlemek için güncelleştirin.

### <a name="verify-android-sdk-version"></a>Android SDK sürümünü doğrula

[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Sonraki adımınız, Google Play Hizmetleri yüklemektir. Firebase Cloud Messaging olan bazı en düşük API düzeyi gereksinimlerini geliştirme ve test amacıyla, hangi **minSdkVersion** özellik bildiriminde uygun olması gerekir.

Eski bir cihazla sınıyorsanız başvurun [Firebase'i Android projenize ekleyin.] nasıl düşük, bu değer ayarlayabilir ve uygun şekilde ayarlanmış belirlemek için.

### <a name="add-firebase-cloud-messaging-to-the-project"></a>Firebase Cloud Messaging projeye Ekle

[!INCLUDE [Add Firebase Cloud Messaging](../../includes/app-service-mobile-add-firebase-cloud-messaging.md)]

### <a name="add-code"></a>Kod ekleme

[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Uygulamayı yayımlanan mobil hizmete karşı test etme

Bir Android telefonla bir USB kablosuyla doğrudan ekleyerek veya öykünücüde sanal cihazı kullanarak uygulamayı test edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticiyi tamamladığınıza göre aşağıdaki öğreticilerden birine açın etmeden göz önünde bulundurun:

* [Android uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-android-get-started-users.md).
  Todolist hızlı başlangıç projesi Android desteklenen kimlik sağlayıcısı kullanarak kimlik doğrulaması ekleme hakkında bilgi edinin.
* [Android uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-android-get-started-offline-data.md).
  Mobile Apps arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı Eşitleme ile kullanıcılar mobil uygulama ile etkileşim kurabilir&mdash;görüntüleme, ekleme veya verileri değiştirme&mdash;hiçbir ağ bağlantısı olduğunda bile.

<!-- URLs -->
[Android hızlı başlangıç]: app-service-mobile-android-get-started.md
[Firebase'i Android projenize ekleyin.]: https://firebase.google.com/docs/android/setup
