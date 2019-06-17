---
title: Apache Cordova ile mobil uygulamalar üzerinde kimlik doğrulaması ekleme | Microsoft Docs
description: Apache Cordova uygulamanızı kimlik sağlayıcıları, Google, Facebook, Twitter ve Microsoft gibi çeşitli kullanıcıların kimliğini doğrulamak için Azure App Service'ta Mobile Apps kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: javascript
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: 23b5967782cf237ed5af2b802aabbbf9c2f781e7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62114219"
---
# <a name="add-authentication-to-your-apache-cordova-app"></a>Apache Cordova uygulamanıza kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Özet
Bu öğreticide, todolist hızlı başlangıç projesi Apache Cordova desteklenen kimlik sağlayıcısı kullanarak şirket için kimlik doğrulaması ekleyin. Bu öğreticide dayanır [Mobile Apps'i kullanmaya başlama] Öğreticisi, öncelikle tamamlamanız gerekir.

## <a name="register"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve App Service'ı yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Benzer adımları gösteren bir video izleyin](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <a name="permissions"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Şimdi arka ucunuzu anonim erişim devre dışı olduğunu doğrulayabilirsiniz. Visual Studio'da:

* Öğretici tamamlandığında, oluşturduğunuz projenin açın [Mobile Apps'i kullanmaya başlama].
* Uygulamanızı çalıştırmak **Google Android öykünücüsü**.
* Uygulama başladıktan sonra beklenmeyen bir bağlantı hatası gösterildiğini doğrulayın.

Ardından, uygulamayı bir mobil uygulama arka ucundan kaynakları istemeden önce kullanıcıların kimliklerini doğrulamak için güncelleştirin.

## <a name="add-authentication"></a>Uygulamaya kimlik doğrulaması ekleme
1. İçerisinde projenizi açın **Visual Studio**ve daha sonra `www/index.html` dosyayı düzenlemek için.
2. Bulun `Content-Security-Policy` baş kısmında meta etiketi.  OAuth konağa izin verilen kaynaklar listesine ekleyin.

   | Sağlayıcı | SDK sağlayıcı adı | OAuth konak |
   |:--- |:--- |:--- |
   | Azure Active Directory | aad | https://login.microsoftonline.com |
   | Facebook | Facebook | https://www.facebook.com |
   | Google | Google | https://accounts.google.com |
   | Microsoft | MicrosoftAccount | https://login.live.com |
   | Twitter | Twitter | https://api.twitter.com |

    Örnek içerik-güvenlik-Policy (Azure Active Directory için uygulanan) aşağıdaki gibidir:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    Değiştirin `https://login.microsoftonline.com` önceki tabloda OAuth konaktan ile.  İçerik Güvenliği İlkesi meta etiketi hakkında daha fazla bilgi için bkz: [içerik Güvenliği İlkesi belgeleri].

    Bazı kimlik doğrulama sağlayıcıları, kullanıma uygun mobil cihazlarda içerik güvenliği ilkesi değişiklikleri gerektirmez.  Örneğin, bir Android cihazında Google kimlik doğrulaması kullanırken, içerik güvenliği ilkesi değişiklik gerekmez.

3. Açık `www/js/index.js` dosyasını düzenlemek için bulun `onDeviceReady()` yöntemi ve istemci oluşturmanın altında kod aşağıdaki kodu ekleyin:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Bu kod, tablo başvurusu oluşturur ve kullanıcı Arabirimi yeniler mevcut kodu değiştirir.

    Tanımlar: login() yöntemi kimlik doğrulama sağlayıcısı ile başlar. JavaScript Promise döndüren bir zaman uyumsuz işlev tanımlar: login() yöntemidir.  Böylece tanımlar: login() yöntem tamamlanana kadar yürütülmez başlatma geri kalanını promise yanıt yerleştirilir.

4. Yeni eklediğiniz kod içinde `SDK_Provider_Name` , oturum açma sağlayıcısı adı. Örneğin, Azure Active Directory kullanın `client.login('aad')`.
5. Projeyi çalıştırın.  Projeyi başlatma tamamlandığında, uygulamanız OAuth oturum açma sayfası seçilen kimlik doğrulama sağlayıcısı için gösterir.

## <a name="next-steps"></a>Sonraki Adımlar
* Daha fazla bilgi edinin [Kimlik doğrulaması hakkında] Azure App Service ile.
* Öğreticiye devam etmek ekleyerek [anında iletme bildirimleri] Apache Cordova uygulamanıza.

SDK'ları kullanmayı öğrenin.

* [Apache Cordova SDK]
* [ASP.NET Sunucusu SDK]
* [Node.js Sunucusu SDK]

<!-- URLs. -->
[Mobile Apps'i kullanmaya başlama]: app-service-mobile-cordova-get-started.md
[İçerik Güvenliği İlkesi belgeleri]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Anında İletme Bildirimleri]: app-service-mobile-cordova-get-started-push.md
[Kimlik doğrulaması hakkında]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Sunucusu SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Sunucusu SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
