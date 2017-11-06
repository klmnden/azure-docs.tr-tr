---
title: "Android'de Mobile Apps ile kimlik doğrulaması ekleme | Microsoft Docs"
description: "Kimlik sağlayıcısı, Google, Facebook, Twitter ve Microsoft dahil olmak üzere çeşitli Android uygulamanızdaki kullanıcıların kimliklerini doğrulamak için Azure App Service Mobile Apps özelliğini kullanmayı öğrenin."
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 81331142aa6110d4e29e6fb30a90ce6e3a853439
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-authentication-to-your-android-app"></a>Android uygulamanıza kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Özet
Bu öğreticide, kimlik doğrulama todolist hızlı başlangıç projesi Android üzerinde desteklenen kimlik sağlayıcısı kullanarak ekleyin. Bu öğretici dayanır [Mobile Apps'i kullanmaya başlamak] önce tamamlamanız gereken öğretici.

## <a name="register"></a>Kimlik doğrulaması için uygulamanızı kaydetmenizi ve Azure uygulama hizmeti yapılandırın
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Uygulamanız için izin verilen dış yönlendirme URL'leri ekleme

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir. Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar. Bu öğreticide, URL şemasının kullanırız _appname_ boyunca. Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz. Mobil uygulamanız için benzersiz olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. [Azure portalında] uygulama hizmetinizi seçin.

2. Tıklatın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. İçinde **yeniden yönlendirme URL'lere izin**, girin `appname://easyauth.callback`.  _Appname_ Bu dize, mobil uygulamanız için URL düzenidir.  Bir protokol (harf kullanın ve yalnızca sayı ve bir harf ile başlar) için normal URL belirtimi izlemelisiniz.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak ihtiyaç duyacağınız seçtiğiniz dizeyi Not olmanız gerekir.

4. **Tamam** düğmesine tıklayın.

5. **Kaydet** düğmesine tıklayın.

## <a name="permissions"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* Bu öğreticiyi, tamamlandı proje Android Studio'da açmak [Mobile Apps'i kullanmaya başlamak]. Gelen **çalıştırmak** menüsünde tıklatın **uygulama çalıştırma**ve uygulama başladıktan sonra durum koduyla işlenmeyen bir özel durum 401 (yetkisiz) tetiklenir doğrulayın.

     Uygulama Kimliği doğrulanmamış bir kullanıcı olarak arka uçtaki erişmeye çünkü bu özel durumun olur ancak *Todoıtem* tablo artık kimlik doğrulaması gerektirir.

Ardından, Mobile Apps arka uçtan kaynakları istemeden önce kullanıcıların kimliklerini doğrulamak için uygulamayı güncelleştirme. 

## <a name="add-authentication-to-the-app"></a>Kimlik doğrulaması için uygulama ekleme
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <a name="cache-tokens"></a>Kimlik doğrulama belirteçleri istemcide önbelleğe alma
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu temel kimlik doğrulaması öğreticisini tamamladığınıza göre aşağıdaki öğreticiler birini açın etmeden göz önünde bulundurun:

* [Android uygulamanızı anında iletme bildirimleri ekleme](app-service-mobile-android-get-started-push.md).
  Anında iletme bildirimleri göndermek için Azure notification hubs'ı kullanmak için Mobile Apps arka uç yapılandırmayı öğrenin.
* [Android uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-android-get-started-offline-data.md).
  Mobile Apps arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı Eşitleme ile kullanıcılar mobil uygulama ile etkileşim kurabilen&mdash;görüntüleme, ekleme veya değiştirme veri&mdash;hiçbir ağ bağlantısı olduğunda bile.

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Mobile Apps'i kullanmaya başlamak]: app-service-mobile-android-get-started.md
