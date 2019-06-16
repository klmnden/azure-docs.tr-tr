---
title: Android'de Mobile Apps ile kimlik doğrulaması ekleme | Microsoft Docs
description: Kimlik sağlayıcıları, Google, Facebook, Twitter ve Microsoft gibi çeşitli Android uygulamanızdaki kullanıcıların kimliğini doğrulamak için Azure App Service Mobile Apps özelliğini kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: android
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 1fc8e7c1-6c3c-40f4-9967-9cf5e21fc4e1
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/16/2017
ms.author: crdun
ms.openlocfilehash: 7b80c1148cf2716e71308d953ac445c4bb50cbc5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62119782"
---
# <a name="add-authentication-to-your-android-app"></a>Android uygulamanıza kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Özet
Bu öğreticide, kimlik doğrulaması todolist hızlı başlangıç projesi Android için desteklenen kimlik sağlayıcısı kullanarak ekleyin. Bu öğreticide dayanır [Mobile Apps'i kullanmaya başlama] Öğreticisi, öncelikle tamamlamanız gerekir.

## <a name="register"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve Azure App Service'ı yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>İzin verilen dış yönlendirme URL'leri uygulamanıza ekleyin

Uygulamanız için yeni bir URL şemasını tanımlamak güvenli kimlik doğrulaması gerektirir. Bu kimlik doğrulama işlemi tamamlandıktan sonra uygulamanıza geri yönlendirmek bir kimlik doğrulama sistemi sağlar. Bu öğreticide, kullandığımız URL şeması _appname_ boyunca. Ancak, seçtiğiniz herhangi bir URL şeması kullanabilirsiniz. Mobil uygulamanız için benzersiz olmalıdır. Sunucu tarafında yeniden yönlendirmeyi etkinleştirmek için:

1. İçinde [Azure portal], App Service'ı seçin.

2. Tıklayın **kimlik doğrulama / yetkilendirme** menü seçeneği.

3. İçinde **izin verilen dış yönlendirme URL'leri**, girin `appname://easyauth.callback`.  _Appname_ bu dizesinde mobil uygulamanız için URL şeması aşağıdaki gibidir.  Bu, bir protokol (kullanım harf ve yalnızca sayı ve bir harfle) için normal URL belirtimi izlemeniz gerekir.  Çeşitli yerlerde URL şeması ile mobil uygulama kodunuzu ayarlamak kullanmanız gerektiğinden, seçtiğiniz dizenin Not.

4. **Tamam**'ı tıklatın.

5. **Kaydet**’e tıklayın.

## <a name="permissions"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

* Android Studio'da öğreticiyi tamamladınız proje açmak [Mobile Apps'i kullanmaya başlama]. Gelen **çalıştırın** menüsünde tıklatın **uygulamayı çalıştırma**ve uygulama başladıktan sonra işlenmeyen bir özel durum ile bir durum kodu 401 (yetkisiz) tetiklenir doğrulayın.

     Uygulama Kimliği doğrulanmamış bir kullanıcı olarak arka uca erişmeye çünkü bu özel durum gerçekleşir ancak *Todoıtem* tablo artık kimlik doğrulaması gerektirir.

Ardından, Mobile Apps arka ucundan kaynakları istemeden önce kullanıcıların kimliklerini doğrulamak için uygulamayı güncelleştirin.

## <a name="add-authentication-to-the-app"></a>Uygulamaya kimlik doğrulaması ekleme
[!INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]



## <a name="cache-tokens"></a>İstemci kimlik doğrulama belirteçlerini önbelleğe alma
[!INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bu temel kimlik doğrulama öğreticisini tamamladığınıza göre aşağıdaki öğreticilerden birine açın etmeden göz önünde bulundurun:

* [Android uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-android-get-started-push.md).
  Anında iletme bildirimleri göndermek için Azure notification hubs'ı kullanmak için Mobile Apps arka ucu yapılandırmayı öğrenin.
* [Android uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-android-get-started-offline-data.md).
  Mobile Apps arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı Eşitleme ile kullanıcılar mobil uygulama ile etkileşim kurabilir&mdash;görüntüleme, ekleme veya verileri değiştirme&mdash;hiçbir ağ bağlantısı olduğunda bile.

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Mobile Apps'i kullanmaya başlama]: app-service-mobile-android-get-started.md
[Azure portal]: https://portal.azure.com/
